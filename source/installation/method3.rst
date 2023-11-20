.. _method3:

Method 3: Installation in Computer Cluster for Large Scale Project Study
===============================================================================

Required Dependencies
---------------------

The following libraries/modules need to be prepared in advance in order for the installation to work properly afterwards:

1. ``Qt library``, version 5.8.0.
2. ``GNU Compiler Collection (GCC)``, version 6.3.0.
3. ``MRtrix3 software for magnetic resonance image processing``, version 3.0.3.
4. ``FreeSurfer software for cortical reconstruction and MRI processing``, version 6.0.0.
5. ``ANTs (Advanced Normalization Tools) for neuroimaging analysis``, version 2.3.1.
6. ``Java``, version 10.0.2.
7. ``MATLAB``, version 2019b.
8. ``dcm2niix software for DICOM to NIfTI conversion``, version 1.0.20190902.
9. ``pigz``, a parallel implementation of gzip for modern multi-processor, multi-core machines, version 2.6.
10. ``Anaconda distribution``, which includes Python and a variety of scientific packages, version 4.3.0.
11. ``git``

If the software versions in the HPC do not exactly match those mentioned above, you can select a version that is similar, as the software is backward compatible.

Otherwise, steps for installing some of the prerequisite software are included later in this document.

If using a managed system such as Bluehive (UofR), Longleaf (UNC), or Sherlock (Stanford) with Slurm and module management 
included, prerequisites should be able to be installed using commands similar to these:
::

    module purge
    module load qt/5.8.0 gcc/6.3.0 mrtrix3/3.0.3 freesurfer/6.0.0 ants/2.3.1 fsl/5.0.9
    module rm python
    module load java/10.0.2 matlab/2019b dcm2niix/1.0.20190902 pigz/2.6 anaconda/4.3.0
    module load git

.. tip::
    When working with the pipeline, particularly when sending tasks to a cluster, it's important to have specific configurations 
    in place. For this purpose, we recommend you create a ``.bashrc_sbci`` file in your home directory and populate it with necessary 
    configurations. These configurations will be sourced every time the pipeline is dispatched to a cluster. Detailed instructions 
    and the importance of this setup are elaborated in the ':ref:`Post-installation`' section.


Alternative Installation of Dependencies
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are using a system that does not have module management capabilities, 
or is missing some of the necessary files listed above, you can refer to the " :ref:`DependencyInstallationinLinux` " section.


Setting up the Python Environment
----------------------------------

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

.. note::
    During the installation of tractconverter, you may encounter errors. 
    If this happens, simply rerunning the installation often resolves the issue. 
    You can install it using the following command: ``pip install https://github.com/MarcCote/tractconverter/archive/master.zip``.

To guarantee the activation of the correct Python environment each time the pipeline is used, run the following command:

::

    source activate sbci



Steps for Installing the SBCI Pipeline
----------------------------------------

**Clone the SBCI pipeline from GitHub**::

    git clone https://github.com/sbci-brain/SBCI_Pipeline.git

**Move the downloaded SBCI pipeline to your desired local folder**::

    mv SBCI_Pipeline /PATH/

.. note:: 

    Replace ``/PATH/`` with the path to your preferred directory.

**Unzip the third-party software bundled with the pipeline**::

    unzip /PATH/SBCI_Pipeline/third_party/scilpy_set.zip -d /PATH/Software/set

**Install SET**::

    cd /PATH/Software/set
    python setup.py build_all
    pip install -e .

**Test the SET installation**:

After the installation, you can test SET using the ``scil_surface.py`` command. Running it should produce an output indicating the expected usage::
    
        scil_surface.py

The output should look like::

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

.. note:: 

    Script ``scil_surface.py`` is located under the path ``/PATH/Software/set/scilpy/scripts/``. 
    Before running the scripts, ensure that you are using the ``sbci`` virtual environment and not the default ``base`` environment.

**Final Check**:

With SET installed, the SBCI pipeline should now be operational. You can verify its functionality 
by checking the scripts in the ``HCP_example`` directory. This folder contains examples of how to use 
the SBCI pipeline with sample HCP (Human Connectome Project) data.


Configuration and Setup Notes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. **Update the sbci_config File**: Ensure the ``/PATH/SBCI_Pipeline/sbci_config`` file reflects your local computing environment 
settings. Adjust paths and parameters accordingly.

2. **Pointing to the Right Template in the Script**: Line 12 of the ``/PATH/SBCI_Pipeline/pipeline_scripts/preproc_step2_t1_dwi_registration.sh`` 
will need to be modified to point to the appropriate template. A template is already provided in the repository 
under the ``data/mni_152_sym_09c directory`` , edit the line as follows::

    export template_dir="/PATH/SBCI_PIPELINE/data/mni_152_sym_09c"

3. **Adjustments for fMRI Data Processing**: Depending on how you intend to process the fMRI data, you might need to 
adjust the script ``/PATH/SBCI_Pipeline/pipeline_scripts/preproc_step4_fmri.sh``.

Steps for Installing the PSC Pipeline
----------------------------------------

Before proceeding with the PSC pipeline installation, ensure that the SET module has been successfully installed during the SBCI pipeline setup.

**Clone the PSC pipeline from GitHub**::

    git clone https://github.com/zhengwu/PSC_Pipeline.git

**Test the PSC installation**:

After the installation, you can test PSC using the ``extraction_sccm_withfeatures_cortical.py`` command. Running it should produce an output indicating the expected usage::

    extraction_sccm_withfeatures_cortical.py

The output should look like::

    usage: extraction_sccm_withfeatures_cortical.py [-h] [--save_sl ]
                                               [--save_diffusion ]
                                               TRACTS FAIMG MDIMG APARC
                                               LABELS_TXT LUT_TXT SUB_ID
                                               MINLEN MAXLEN DILATION_DIST
                                               DILATION_WINDSIZE INROILEN PRE
    extraction_sccm_withfeatures_cortical.py: error: too few arguments

.. note:: 

    Script ``extraction_sccm_withfeatures_cortical.py`` is located under the path ``/PATH/PSC_Pipeline/scripts/``. 
    Before running the scripts, ensure that you are using the ``sbci`` virtual environment and not the default ``base`` environment.


.. _Post-installation:

Post-installation usage
-----------------------

To ensure the pipeline runs within the same environment each time, 
it's advisable to have a dedicated ``.bashrc_sbci file``. This file should be sourced whenever the pipeline is executed.

Below is a sample ``.bashrc`` configuration tailored for the UNC Longleaf environment at the time of writing. 
Remember to update the placeholder ``/PATH/TO/PSC_PIPELINE/`` with the actual path to your PSC installation.

::

    # .bashrc_sbci for UNC Longleaf environment

    # Load necessary modules
    module load qt/5.8.0 gcc/6.3.0 mrtrix3/3.0.3 freesurfer/6.0.0 ants/2.3.1 fsl/5.0.9
    module load java/10.0.2 matlab/2017b dcm2niix/1.0.20190902 pigz/2.6 anaconda/4.3.0
    module load git

    # Unload the python module to avoid conflicts
    module unload python

    # Configure conda for Longleaf compatibility
    source /nas/longleaf/apps/anaconda/4.3.0/anaconda/etc/profile.d/conda.sh

    # Direct to Freesurfer installation
    export PATH="/nas/longleaf/apps/freesurfer/6.0.0/freesurfer/fsfast/bin:$PATH"
    export PATH="/nas/longleaf/apps/freesurfer/6.0.0/freesurfer/fsfast/toolbox:$PATH"

    # Update PATH and PYTHONPATH for the PSC installation
    export PATH="/PATH/TO/PSC_PIPELINE/scripts:$PATH"
    export PYTHONPATH="/PATH/TO/PSC_PIPELINE:$PYTHONPATH"

    # Point to the Ants installation directory
    export ANTSPATH="/nas/longleaf/apps/ants/2.3.1/src/build/bin/"

    # Initialize Freesurfer environment
    source /nas/longleaf/apps/freesurfer/6.0.0/freesurfer/SetUpFreeSurfer.sh

    # Activate the specific Python environment for the pipeline
    conda activate sbci
