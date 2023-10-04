Using a Bashrc File
=====================================

To ensure the pipeline runs within the same environment each time, 
it's advisable to have a dedicated ``.bashrc_sbci file``. This file should be sourced whenever the pipeline is executed.

Below is a sample ``.bashrc`` configuration tailored for the Longleaf environment at the time of writing. 
Remember to update the placeholder ``/PATH/TO/PSC_PIPELINE/`` with the actual path to your PSC installation.

::

    # .bashrc_sbci for Longleaf environment

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
