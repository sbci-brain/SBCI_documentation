Detailed Installation SBCI Pipeline
=====================================

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
--------------------------------

1. **Update the sbci_config File**: Ensure the ``/PATH/SBCI_Pipeline/sbci_config`` file reflects your local computing environment 
settings. Adjust paths and parameters accordingly.

2. **Pointing to the Right Template in the Script**: Line 12 of the ``/PATH/SBCI_Pipeline/pipeline_scripts/preproc_step2_t1_dwi_registration.sh`` 
will need to be modified to point to the appropriate template. A template is already provided in the repository 
under the ``data/mni_152_sym_09c directory`` , edit the line as follows::

    export template_dir="/PATH/SBCI_PIPELINE/data/mni_152_sym_09c"

3. **Adjustments for fMRI Data Processing**: Depending on how you intend to process the fMRI data, you might need to 
adjust the script ``/PATH/SBCI_Pipeline/pipeline_scripts/preproc_step4_fmri.sh``.

4. **Configuration for Freesurfer, FSL, and ANTs**: To ensure that Freesurfer, FSL, and ANTs operate correctly, 
append the provided lines to the ``.bashrc_sbci file`` (or whichever script executes upon user login). 
Modify these lines based on your specific installation path for Freesurfer::

    export PATH="/YOUR_FREESURFER_PATH/fsfast/bin:$PATH"
    export PATH="/YOUR_FREESURFER_PATH/fsfast/toolbox:$PATH"

    export PATH="/YOUR_ANTS_PATH/ANTs-2.3.1/Scripts:$PATH"
    export ANTSPATH="/YOUR_ANTS_PATH/build/bin/"

    source /YOUR_FREESURFER_PATH/SetUpFreeSurfer.sh

Replace ``YOUR_FREESURFER_PATH`` and ``YOUR_ANTS_PATH`` with the correct directory paths on your system.

5. **Validate the Installation**: Run the following commands to validate the installation::

    which antsRegistration # This should display the full path to antsRegistration.
    antsRegistrationSyN.sh # This will display the usage instructions for this script.

