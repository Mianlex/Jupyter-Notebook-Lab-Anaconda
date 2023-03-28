# How to launch Jupyter Notebook/Lab on Vega?

This guide explains how to launch Jupyter Notebook/Lab on Vega, the Slovenian national supercomputer. The default method provided on the Vega documentation page requires users to login to the Vega gpulogin node and book a compute node for running Jupyter Notebook. However, after testing, it has been found that Jupyter Notebook can also be launched on the cpu/gpu login node or CPU compute nodes, which is similar to Nilfhim.

The Jupyter notebook on Vega is based on the Anaconda3 package. When launching the default environment in Anaconda, some errors may occur, like NumPy conflict or the ase module failing to load. To run Jupyter Notebook fluently with your requirements, the following method can be used to build a kernel based on the Anaconda3-python environment:
