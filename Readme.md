
# First load Anaconda3:
module load Anaconda3/2022.05

conda create --yes --name mylab python=3.9.7

conda activate mylab

# Install ASE, Jupyter, and scientific calculation environment
conda install -c conda-forge ase jupyterlab ipykernel

# Install any package you want
conda install --quiet --yes ipython ipykernel scikit-learn tqdm

