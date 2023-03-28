# How to launch Jupyter Notebook/Lab on Vega?

This guide explains how to launch Jupyter Notebook/Lab on Vega. The default method provided on the Vega documentation page requires users to login to the Vega gpulogin node and book a compute node for running Jupyter Notebook. However, after testing, it has been found that Jupyter Notebook can also be launched on the cpu/gpu login node or CPU compute nodes, which is similar to Niflheim.

The Jupyter notebook on Vega is based on the Anaconda3 package. When launching the default environment in Anaconda, some errors may occur, like NumPy conflict or the ase module failing to load. To run Jupyter Notebook fluently with your requirements, the following method can be used to build a kernel based on the Anaconda3-python environment:

1.Load Anaconda3:

`module load Anaconda3/2022.05`

2.Create a new environment named "mylab" with Python 3.9.7:

`conda create --yes --name mylab python=3.9.7`

3.Activate the new environment:

`conda activate mylab`

4.Install ASE, Jupyter, and scientific calculation environment:

`conda install -c conda-forge ase jupyterlab nodejs`

`conda install ipykernel`

5.Install any additional package(s) that you need:

`conda install --quiet --yes ipython scikit-learn tqdm`


After building the environment, build a Jupyter Notebook/Lab configuration before running it. This is strongly recommended so that you can run multi-client Jupyter Notebook/Lab in different work directories that are correlated to different research projects and save your lab every time. You can restore your Jupyter Notebook/Lab tags when you launch next time.

To generate a configuration file, run:

`jupyter lab --generate-config`

Once the configuration file is generated, copy it to your Jupyter configuration directory (defined by yourself). For example:

`cp /ceph/hpc/home/eumianlex/.jupyter/jupyter_lab_config.py /ceph/hpc/home/eumianlex/Jupyterlab/Mylab1.py`

You can copy multiple times for different purposes if you want to build more than one lab.

In `Mylab1.py`:
```
# Configuration file for lab.
c = get_config()  # noqa
c.ServerApp.root_dir = '/ceph/hpc/data/r2020235596-users/mianle/test'
c.ServerApp.open_browser = False
```
The previous ase and jupyter environment settings only need to be done once.

### Now it is time to launch Jupyter Notebook/Lab:
Here is the bash script launchJupyter1.sh:
```
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
```

You just need to run this bash script every time you want to launch your jupyter notebook. Copy and paste the first line to your new terminal in your local computer (SSH to the IP and port), and copy the output HTTP address to your local browser. You will launch your Jupyter Lab.



It is strongly recommended that each lab works for one of your projects. Save the Bash script to a different project directory and launch it when you work on a specific project. Remember to use different port, configuration and lab name when running multi-client jupyter notebook/lab.


#### For example:
In Lab1 : 
```
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
```

In Lab2 :
```
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
```

If you have some heavy test on jupyter notebook/lab, book a compute node and run the bash script in the compute node. Then you can launch your jupyter notebook in compute node.


