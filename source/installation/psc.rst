Detailed Installation PSC Pipeline
=====================================

Before proceeding with the PSC pipeline installation, ensure that the SET module has been successfully installed during the SBCI pipeline setup.

Steps for Installing the PSC Pipeline
----------------------------------------

**Clone the PSC pipeline from GitHub**::

    git clone https://github.com/zhengwu/PSC_Pipeline.git

**Configure Environmental Variables**: Add the following lines to the ``.bashrc_sbci file`` (or whichever script executes upon user login)::

    export PATH="/home/username/Software/PSC_Pipeline/scripts:$PATH"
    export PYTHONPATH="/home/username/Software/PSC_Pipeline:$PYTHONPATH"

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


Configuration and Setup Notes
--------------------------------

1. **Dependencies**: PSC relies on MRtrix, FSL, and Freesurfer. Ensure these are installed on your system prior to using PSC.

2. **Script Modifications**: Some directories or paths in the provided scripts may require adjustments to suit your specific system setup or to ensure accurate file locations.
