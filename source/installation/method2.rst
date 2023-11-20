.. _method2:

Method 2: Configure Your Own Environment Locally
===============================================================
This guide provides detailed instructions for installing SBCI on a Linux environment. 
Docker is our recommended installation method to help avoid common issues. 
If you prefer to perform a manual installation, please follow the instructions below:

**Please note that SBCI is not compatible with Windows OS. For Windows users, we suggest running a Linux virtual machine.**

.. _DependencyInstallationinLinux:

Linux Installation
-----------

1. Install Dependencies
^^^^^^^^^^^^^^^^^^^^^^

Before proceeding with the installation, ensure the following dependencies are installed:

- Qt library, version 5.9.5.
- GNU Compiler Collection (GCC), version 6.5.0.
- MRtrix3 software for magnetic resonance image processing, version 3.0.4.
- FreeSurfer software for cortical reconstruction and MRI processing, version 6.0.0.
- ANTs (Advanced Normalization Tools) for neuroimaging analysis, version 2.3.1.
- fsl, version 5.0.9.
- Java, version 10.0.2.
- MATLAB, version 2019b.
- dcm2niix software for DICOM to NIfTI conversion, version 1.0.20190902.
- pigz, a parallel implementation of gzip for modern multi-processor, multi-core machines, version 2.8.
- Anaconda distribution, which includes Python and a variety of scientific packages, version 4.3.0.
- git

To check the version of an existing installation, use the following commands:

.. code-block:: bash

   # For each dependency, run the respective command to check its version:
   # Qt
   qmake --version
   # GCC
   gcc --version
   # MRtrix3
   mrinfo --version
   # FreeSurfer
   freesurfer --version
   # ANTs
   antsRegistration --version
   # fsl
   fsl
   # Java
   java -version
   # MATLAB
   matlab -version
   # dcm2niix
   dcm2niix -version
   # pigz
   pigz --version
   # Anaconda
   conda --version

If you need to install any of the above dependencies, follow the instructions below:

.. note:: 
    In the instructions below, replace ``/PATH_/`` with the desired installation directory. We recommend ``/home/yourusername/opt/`` for ease of access.




1.1. **System Dependencies**

Updating your system and installing required packages is essential before proceeding:

.. code-block:: bash

   sudo apt-get update
   sudo apt-get upgrade
   sudo apt install build-essential libgl1-mesa-dev libmpc-dev libmpfr-dev libgmp-dev g++ python3 python3-numpy libeigen3-dev zlib1g-dev libqt5opengl5-dev libfftw3-dev libtiff5-dev libqt5svg5-dev curl cmake wget unzip

1.2. **Install Qt**

Install Qt and verify the installation with:

.. code-block:: bash

   sudo apt install qt5-default
   qmake --version


1.3. **Install GCC**

Install GCC version 6.5.0 and confirm it's the default version using:

.. code-block:: bash
    
    sudo apt install gcc-6 g++-6
    gcc --version

    # Set GCC 6.5.0 as the default if necessary
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 60 --slave /usr/bin/g++ g++ /usr/bin/g++-6
    sudo update-alternatives --config gcc

1.4. **Install MRtrix3**

Clone and build MRtrix3, then add it to your PATH:

.. code-block:: bash

    cd /PATH_mrtrix/
    git clone https://github.com/MRtrix3/mrtrix3.git
    ./configure
    ./build
    # check version
    export PATH=$PATH:/PATH_mrtrix/mrtrix3/bin
    mrinfo --version

1.5. **Install FreeSurfer**

Download and install FreeSurfer along with necessary patches:

.. code-block:: bash

    # Create a directory to store downloaded files
    mkdir -p /PATH_freesurfer/dist
    cd /PATH_freesurfer/dist

    # Download and extract FreeSurfer
    wget ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/6.0.0/freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz
    cd /PATH_freesurfer
    tar -zxf dist/freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz

    # Download the MATLAB Runtime and extract it into the FreeSurfer directory
    cd /PATH_freesurfer/freesurfer
    curl -L "http://surfer.nmr.mgh.harvard.edu/fswiki/MatlabRuntime?action=AttachFile&do=get&target=runtime2012bLinux.tar.gz" -o "runtime2012b.tar.gz"
    tar -xvf runtime2012b.tar.gz

    # Download the FreeSurfer patch
    cd /PATH_freesurfer/dist
    wget ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/6.0.0-patch/mri_glmfit-sim

    # Copy the patch file to the FreeSurfer bin directory, backing up the original
    cd /PATH_freesurfer/freesurfer/bin
    cp -p mri_glmfit-sim mri_glmfit-sim.orig
    cp /PATH_freesurfer/dist/mri_glmfit-sim .

    # Update the FreeView binary file
    cp freeview.bin freeview.bin.BKP
    wget ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/dev_binaries/centos6_x86_64/freeview.bin
    chmod +x freeview.bin

    # check version
    export FREESURFER_HOME=/PATH_freesurfer/freesurfer
    source $FREESURFER_HOME/SetUpFreeSurfer.sh
    freesurfer --version

1.6. **Install ANTs**

The installation of ANTs requires gcc version 7.0 or above. Here we take the installation of gcc8 as an example:

.. code-block:: bash

    # Install gcc-8
    sudo apt-get install gcc-8 g++-8
    # Set gcc-8 as default
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-8
    sudo update-alternatives --config gcc


Once gcc8 is configured, you can start the installation of ANTs:

.. code-block:: bash

    # Create a directory to hold the source files and change to that directory
    mkdir -p /PATH_ant/src/dist
    cd /PATH_ant/src/dist

    # Download ANTs from the specified GitHub release
    wget https://github.com/ANTsX/ANTs/archive/v2.3.1.tar.gz

    # Change to the src directory
    cd /PATH_ant/src

    # Extract the tarball
    tar xf dist/v2.3.1.tar.gz

    # Create a build directory and change to that directory
    mkdir build
    cd build

    # Configure git to use https instead of git protocol for github
    git config --global url."https://github.com/".insteadOf git://github.com/

    # Run CMake to configure the ANTs build, specifying the path to the ANTs source directory
    cmake ../ANTs-2.3.1

    # Note: The following ccmake step is an interactive step which must be done in the terminal.
    # Start the ccmake interactive interface
    ccmake ../ANTs-2.3.1

    # In the ccmake interface:
    # 1. Navigate to the BUILD_TESTING option.
    # 2. Set it to OFF.
    # 3. Press 'c' to configure.
    # 4. After configuration is done, press 'g' to generate the makefiles and exit.

    make

    # check version
    export ANTSPATH=/PATH_ant/src/build/bin
    export PATH=$ANTSPATH:$PATH
    antsRegistration --version

The above steps should solve all possible problems. 
If other problems arise, please refer to the ANTs official installation guide: https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS.

1.7. **Install fsl**

Here we need to use **NeuroDebian**. You can install **fsl-complete** by following the prompts at the following URL: http://neuro.debian.net/install_pkg.html?p=fsl-complete.

After the installation is complete, you can check the version with the following code:

.. code-block:: bash

    export FSLDIR=/PATH_fsl/fsl/5.0
    cat ${FSLDIR}/etc/fslversion


1.8. **Install Java**

You can download JDK10 from the following URL: https://www.oracle.com/java/technologies/java-archive-javase10-downloads.html.

After the download is complete, you can use the following code to unzip JDK10:

.. code-block:: bash

    sudo tar xf jdk-10.0.2_linux-x64_bin.tar.gz -C /PATH_Java/

    # check version
    export JAVA_HOME=/PATH_Java/jdk-10.0.2
    export PATH=$PATH:$JAVA_HOME/bin
    java --version


1.9. **Install MATLAB**

You can download Matlab from the following URL: https://www.mathworks.com/login?uri=%2Fdownloads%2Fweb_downloads.

After the download is complete, you can use the following code to unzip and install MATLAB:

.. code-block:: bash

    sudo unzip matlab_R2019b_glnxa64.zip -d /PATH_matlab/matlab
    cd /PATH_matlab/matlab
    sudo ./install


1.10. **Install dcm2niix**

.. code-block:: bash

    wget https://github.com/rordenlab/dcm2niix/releases/download/v1.0.20190902/dcm2niix_lnx.zip
    unzip dcm2niix_lnx.zip
    chmod +x dcm2niix
    sudo mv dcm2niix /usr/local/bin/
    # check version
    dcm2niix -version


1.11. **Install pigz**

.. code-block:: bash

    # Download the source code package
    wget http://www.zlib.net/pigz/pigz-2.8.tar.gz

    # Unzip the source code package
    tar -xvf pigz-2.8.tar.gz

    # Enter the unzipped directory
    cd pigz-2.8

    # Compile the source code
    make

    # Add the compiled executable file to the path
    sudo cp pigz /usr/local/bin/
    sudo cp unpigz /usr/local/bin/
    # check version
    pigz -version


1.12. **Install Anaconda**

.. code-block:: bash

    wget https://repo.anaconda.com/archive/Anaconda3-4.3.0-Linux-x86_64.sh
    bash Anaconda3-4.3.0-Linux-x86_64.sh


1.13. **Install git**

.. code-block:: bash

    sudo apt-get install git



2. Setting up the Python environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The pipeline installation requires a clean Python 2.7 environment as a base.

If you don't have Anaconda2 installed on your system yet, you can install it using the following code:

.. code-block:: bash

   wget https://repo.continuum.io/archive/Anaconda2-2019.10-Linux-x86_64.sh
   bash Anaconda2-2019.10-Linux-x86_64.sh

After the installation, create and activate a conda environment named 'sbci':

.. code-block:: bash

   conda create -n sbci python=2.7
   conda activate sbci

Install the required Python packages:

.. code-block:: bash

    conda install numpy
    conda install scipy
    conda install matplotlib
    conda install ipython
    conda install jupyter
    conda install cython

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

3. Installation SBCI Pipeline
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Clone the SBCI pipeline from GitHub:

.. code-block:: bash

   git clone https://github.com/sbci-brain/SBCI_Pipeline.git

Move the downloaded SBCI pipeline to your desired local folder:

.. code-block:: bash

   mv SBCI_Pipeline /PATH_SBCI/

Unzip the third-party software bundled with the pipeline:

.. code-block:: bash

    mkdir -p /PATH_SBCI/Software/set
    unzip /PATH_SBCI/SBCI_Pipeline/thirdparty/scilpy_set.zip -d /PATH_SBCI/Software/set

Install SET:

.. code-block:: bash

    cd /PATH_SBCI/Software/set
    python setup.py build_all
    pip install -e .

Test the SET installation:

After the installation, you can test SET using the **scil_surface.py** command. Running it should produce an output indicating the expected usage:

.. code-block:: bash

    cd /PATH_SBCI/Software/set/scilpy/scripts/
    scil_surface.py

The output should look like:

.. code-block:: bash

    usage: scil_surface.py [-h] [--vts_mask VTS_MASK]
                   [-a ANNOT | -l LABEL | -m MORPH | --vts_scalar VTS_SCALAR | --vts_color VTS_COLOR | --vts_label VTS_LABEL | --image_mask IMAGE_MASK | --vts_val VTS_VAL]
                   [-i INDICES [INDICES ...]] [--inverse_mask]
                   [--save_vts_mask SAVE_VTS_MASK]
                   [--save_vts_scalar SAVE_VTS_SCALAR]
                   [--save_vts_color SAVE_VTS_COLOR]
                   [--save_vts_label SAVE_VTS_LABEL]
                   [--masked_labels_value MASKED_LABELS_VALUE]
                   [-v | --save_image SAVE_IMAGE] [--no_scalar_for_masked]
                   [--no_scalar_at NO_SCALAR_AT] [--white] [-f]
                   surface
    scil_surface.py: error: too few arguments

Final Check:

With SET installed, the SBCI pipeline should now be operational. 
You can verify its functionality by checking the scripts in the **HCP_example** directory. 
This folder contains examples of how to use the SBCI pipeline with sample HCP (Human Connectome Project) data.

4. Installation PSC Pipeline
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before proceeding with the PSC pipeline installation, ensure that the SET module has been successfully installed during the SBCI pipeline setup.

Clone the PSC pipeline from GitHub:

.. code-block:: bash

   cd /PATH_SBCI/Software
   git clone https://github.com/zhengwu/PSC_Pipeline.git

Test the PSC installation:

After the installation, you can test PSC using the **extraction_sccm_withfeatures_cortical.py** command. Running it should produce an output indicating the expected usage:

.. code-block:: bash

    cd /PATH_SBCI/Software/PSC_Pipeline/scripts
    export PATH="/PATH_SBCI/Software/PSC_Pipeline/scripts:$PATH"
    export PYTHONPATH="/PATH_SBCI/Software/PSC_Pipeline:$PYTHONPATH"
    extraction_sccm_withfeatures_cortical.py

The output should look like:

.. code-block:: bash

    usage: extraction_sccm_withfeatures_cortical.py [-h] [--save_sl ]
                                            [--save_diffusion ]
                                            TRACTS FAIMG MDIMG APARC
                                            LABELS_TXT LUT_TXT SUB_ID
                                            MINLEN MAXLEN DILATION_DIST
                                            DILATION_WINDSIZE INROILEN PRE
    extraction_sccm_withfeatures_cortical.py: error: too few arguments




5. Post-installation usage
^^^^^^^^^^^^^^^^^^^^^^^^^

We strongly recommend that you configure all environment variables in a single file. You can create a file at a specific path, such as **'~/bashrc_sbci.sh'**, and add the following code to the file:

.. code-block:: bash

    #!/bin/bash

    #ANTs
    export ANTSPATH=/PATH_ant/src/build/bin
    export PATH=$ANTSPATH:$PATH

    # Set up FreeSurfer
    export FREESURFER_HOME=/PATH_freesurfer/freesurfer
    source $FREESURFER_HOME/SetUpFreeSurfer.sh

    # Set up MRtrix3
    export PATH=$PATH:/PATH_mrtrix/mrtrix3/bin

    # GCC 6.5.0
    # We used gcc8 when installing ANTs, the following code switches the default gcc version back to gcc6
    sudo update-alternatives --config gcc


    # FSL
    export FSLDIR=/PATH_fsl/fsl/5.0
    export PATH=${FSLDIR}/bin:${PATH}
    export LD_LIBRARY_PATH=/usr/lib/fsl/5.0:${LD_LIBRARY_PATH}

    #JAVA
    export JAVA_HOME=/PATH_Java/jdk-10.0.2
    export PATH=$PATH:$JAVA_HOME/bin

    #Matlab
    export PATH=$PATH:/PATH_matlab/matlab/R2019b/bin

    #Anaconda 4.3
    export PATH="/PATH_anaconda/anaconda3/bin:$PATH"

    #PSC
    export PATH="/PATH_SBCI/Software/PSC_PIPELINE/scripts:$PATH"
    export PYTHONPATH="/PATH_SBCI/Software/PSC_PIPELINE:$PYTHONPATH"

    conda activate sbci

Afterwards, you can activate the environment with the following code:

.. code-block:: bash

   chmod +x ~/bashrc_sbci.sh
   ~/bashrc_sbci.sh

**Note: For production environments, ensure you have backups and/or use version control systems to manage your codebase and changes.**



Mac OS Installation
-----------
