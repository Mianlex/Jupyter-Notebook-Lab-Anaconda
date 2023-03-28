How to launch Jupyter Notebook/Lab on Vega?

On Vega, the tutorial in webpage(https://doc.vega.izum.si/jupyter/) has a method for launching Jupyter notebook.
However, the default method in webpage require to login to Vega gpulogin node and book a compute node for running Jupyter Notebook. After my test, the Jupyter Notebook can also launch in cpu/gpu login node or CPU compute nodes, which is quite similar to Nilfhim. 
The Jupyter notebook on Vega is based on Anaconda3 package. When launch the default one, the base environment in Anaconda, some error may occur, like the NumPy conflict or ase module fail to load. 
According to run Jupyter notebook fluently with our requirement, here is a method to build a kernel based on the Anaconda3-python environment.

module load Anaconda3/2022.05

conda create --yes --name mylab python=3.9.7

conda activate mylab

#install ASE,jupyter,and scitific calculation environment
conda install -c conda-forge ase jupyterlab ipykernel


#install any package you want
conda install --quiet --yes ipython ipykernel scikit-learn tqdm

After building your environment, build a jupyter notebook/lab configuration before running it. This is strongly recommended that we can run multi-client jupyter notebook/lab in different work dir which is correlated to different research project and save your lab everytime. You can restore your jupyter notebook tags when you launch next time.

jupyter lab --generate-config

Once the configuration file generated, copy it to your jupyter configuration dir(defined by yourselves). In my case:

cp /ceph/hpc/home/eumianlex/.jupyter/jupyter_lab_config.py Mylab1.py
cp /ceph/hpc/home/eumianlex/.jupyter/jupyter_lab_config.py Mylab2.py

copy multiple times for different purposes, if you want to build more than one lab.

In an example: Mylab1.py

# Configuration file for lab.

c = get_config()  #noqa
c.ServerApp.root_dir = '/ceph/hpc/data/r2020235596-users/mianle/test'
c.ServerApp.open_browser = False



Now it is time to launch jupyter notebook/lab:

#!/bin/bash
# Get the hostname
hostname=$(hostname)
# Split the hostname using "." as the separator and get the first part
node=$(echo "$hostname" | cut -d'.' -f1)
port=40000
module load Anaconda3/2022.05
source activate mylab
echo "ssh -N -f -L "$port":"$node":"$port" <your_user_name>@vglogin0005.vega.izum.si"
jupyter-lab --port="$port" --ip="$node" --config=/ceph/hpc/home/eumianlex/Jupyterlab/Mylab1.py --LabApp.name="Lab1"

run this bash script everytime you want to launch your notebook. copy and paste the first line to you new terminal in your local computer(ssh to the ip and port), and copy the output http address to your local browser. You will launch your jupyter lab. 


Strongly recommend that each lab work for one of your projects, save the bash script to different project dir and launch then when you work on specific project. Remember to use different port, configuration and lab name when running multi-client jupyter notebook/lab.


For example: 
In Lab1 : 
#!/bin/bash
# Get the hostname
hostname=$(hostname)
# Split the hostname using "." as the separator and get the first part
node=$(echo "$hostname" | cut -d'.' -f1)
port=40000
module load Anaconda3/2022.05
source activate mylab
echo "ssh -N -f -L "$port":"$node":"$port" <your_user_name>@vglogin0005.vega.izum.si"
jupyter-lab --port="$port" --ip="$node" --config=/ceph/hpc/home/eumianlex/Jupyterlab/Mylab1.py --LabApp.name="Lab1"


In Lab2 : 
#!/bin/bash
# Get the hostname
hostname=$(hostname)
# Split the hostname using "." as the separator and get the first part
node=$(echo "$hostname" | cut -d'.' -f1)
port=40001
module load Anaconda3/2022.05
source activate mylab
echo "ssh -N -f -L "$port":"$node":"$port" <your_user_name>@vglogin0005.vega.izum.si"
jupyter-lab --port="$port" --ip="$node" --config=/ceph/hpc/home/eumianlex/Jupyterlab/Mylab2.py --LabApp.name="Lab2"
