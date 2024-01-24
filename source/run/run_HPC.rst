.. _run_HPC:

Run SBCI Pipeline on HPC
==========================


1. Activate the Environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^

It's recommended to manage and activate environments using the ``~/.bashrc_sbci`` file across all installation systems. 
This ensures consistency in the environment for every software run. 

Example .bashrc_sbci Configuration for UNC Longleaf Environment:


.. code-block:: bash

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



2. Preprocessing the data
^^^^^^^^^^^^^^^^^^^^^^^^^

Preprocessing involves using the ``PATH/SBCI_Pipeline/ABCD_example/preprocess.sh`` script. 
Modify the paths in preprocess.sh to point to the correct locations before running it. 
``preprocess.sh`` requires three input arguments: the subject list location, output location, and script location. 
The subject list should point to a text file containing paths of all data to be processed.

Example Command for Preprocessing:

.. code-block:: bash

    ./preprocess.sh /Path_to_Datafolder/subjectlist /Path_to_Outputfolder /Path/SBCI_Pipeline/ABCD_example

Preprocessing steps include:

1. Registration of T1 and DWI (Diffusion Weighted Imaging) data.
2. Processing of T1 data with FreeSurfer software.
3. Estimation of Fiber Orientation Distribution Functions (FODF).
4. Processing of functional Magnetic Resonance Imaging (fMRI) data.


3. Run PSC Process
^^^^^^^^^^^^^^^^^^

Similar to the previous step, use ``PATH/SBCI_Pipeline/ABCD_example/process_psc.sh``. 
Modify the paths in process_psc.sh before running. 
``process_psc.sh`` also requires three input parameters: subject list location, output location, and script location.

Example Command for PSC Process:

.. code-block:: bash

    ./process_psc.sh /Path_to_Datafolder/subjectlist /Path_to_Outputfolder /Path/SBCI_Pipeline/ABCD_example

PSC processing steps include:

1. Fiber tracking.
2. Computing Particle Filtering Tractography (PFT) maps.
3. Streamline tracking.
4. Connectivity matrix computation.


4. Run SBCI Process
^^^^^^^^^^^^^^^^^^

As with the previous steps, use ``PATH/SBCI_Pipeline/ABCD_example/process_sbci.sh``. 
Modify the paths in process_sbci.sh before execution. 
``process_sbci.sh`` requires three input parameters: subject list location, output location, and script location.

Example Command for SBCI Process:

.. code-block:: bash

    ./process_sbci.sh /Path_to_Datafolder/subjectlist /Path_to_Outputfolder /Path/SBCI_Pipeline/ABCD_example

SBCI processing steps include:

1. Processing SBCI grids.
2. Preprocessing datasets.
3. Running datasets.
4. Processing surface data.
5. Structural data analysis.
6. Functional data analysis.



5. Quality Control (QC)
^^^^^^^^^^^^^^^^^^^^^^^
The Quality Control (QC) process is integral to ensuring the integrity of data processing. 
It involves checking whether necessary files have been correctly created at each step of the workflow. 
For this purpose, we utilize three specific shell scripts: ``preprocess_qc.sh``, ``psc_qc.sh``, and ``sbci_qc.sh``.

.. code-block:: bash

    ./sbci_qc.sh /Path_to_Datafolder/subject_list /Path_to_Datafolder /Path_to_output_logfile


After running the QC script, it's important to rename the QC log file for clarity and future reference.

Renaming the QC Log File
Use the mv command to rename the log file appropriately:

.. code-block:: bash

    mv sbci_qc_log sbci_qc_log_subject_list1

To extract a list of subjects where the QC process failed, use the following shell script command:

.. code-block:: bash

    awk -F ' ' '/FAILED/ {print $1}' sbci_qc_log_subjectlist1 > preprocess_failed_subject_list1


6. Clean Subject Folder
^^^^^^^^^^^^^^^^^^^^^^^
After the processing is complete, there are some final finishing touches. These include deleting unnecessary files and organizing the files in the output folder into one folder.
These can be done by using the ``PATH/SBCI_Pipeline/ABCD_example/clean_subject_folders.sh`` script.


.. code-block:: bash

    ./clean_subject_folders.sh /Path_to_Datafolder/subject_list /Path_to_Datafolder /Path/SBCI_Pipeline/ABCD_example

The final results have a folder for each subject containing all the result files, both structural and functional, 
named ``psc_sbci_final_files``. The path is ``/Path_to_Datafolder/subject_list/psc_sbci_final_files``.