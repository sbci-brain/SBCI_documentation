Setting up the Python Environment
=======================================
The pipeline installation requires a clean Python 2.7 environment as a base.

If you haven't installed Anaconda yet, you can download the latest version of Anaconda2 by visiting the following URL: https://repo.anaconda.com/archive/

If you have Anaconda installed (either through a module or manually, see below), the following commands set up a clean Python 2.7 environment with all the neccesary packages installed:

::

    conda create -n sbci python=2.7
    conda activate sbci

    conda install numpy scipy matplotlib ipython jupyter cython

    pip install h5py==2.9.0
    pip install imageio==2.4.1
    pip install moviepy==0.2.3.5
    pip install openpyxl==2.4.8
    pip install pandas==0.20.3
    pip install Pillow==5.2.0
    pip install requests==2.19.1
    pip install scikit-learn==0.19.0
    pip install vtk==8.1.2
    pip install PyMCubes==0.0.9
    pip install nibabel==2.4.0
    pip install https://github.com/MarcCote/tractconverter/archive/master.zip
    pip install fury==0.4.0
    pip install dipy==0.16.0
    pip install trimeshpy==0.0.2

.. warning::
    During the installation of tractconverter, you may encounter errors. 
    If this happens, simply rerunning the installation often resolves the issue. 
    You can install it using the following command: ``pip install https://github.com/MarcCote/tractconverter/archive/master.zip``.

To ensure that the correct Python environment is activated each time the pipeline is used, 
append the following line to the ``.bashrc_sbci`` file (or any other script file executed upon user login):

::

    source activate sbci

This sets up the necessary Python environment automatically whenever you log in or initiate the pipeline.