.. _run_Docker:

Run SBCI Pipeline on Docker
==========================

After following the previous steps, all pipeline-related folders can be found under ``/opt``. 
More specifically, all scripts related to the ABCD project can be found under ``/opt/SBCI_Pipeline/ABCD_example``.
It is recommended to create a separate folder containing all subjects and their data, as well as the subjectlist file, where appropriate.

1. Preprocessing the data
^^^^^^^^^^^^^^^^^^^^^^^^^

Preprocessing involves using the ``/opt/SBCI_Pipeline/ABCD_example/preprocess.sh`` script. 
Modify the paths in preprocess.sh to point to the correct locations before running it. 
``preprocess.sh`` requires three input arguments: the subject list location, output location, and script location. 
The subject list should point to a text file containing paths of all data to be processed.

.. tip::
    
    this software includes a follow-up process, it is recommended that the output folder and the data folder be in the same folder.

Example Command for Preprocessing:

.. code-block:: bash

    ./preprocess.sh /Path_to_Datafolder/subjectlist /Path_to_Datafolder /opt/SBCI_Pipeline/ABCD_example

Preprocessing steps include:

1. Registration of T1 and DWI (Diffusion Weighted Imaging) data.
2. Processing of T1 data with FreeSurfer software.
3. Estimation of Fiber Orientation Distribution Functions (FODF).
4. Processing of functional Magnetic Resonance Imaging (fMRI) data.


2. Run PSC Process
^^^^^^^^^^^^^^^^^^

Similar to the previous step, use ``/opt/SBCI_Pipeline/ABCD_example/process_psc.sh``. 
Modify the paths in process_psc.sh before running. 
``process_psc.sh`` also requires three input parameters: subject list location, output location, and script location.

Example Command for PSC Process:

.. code-block:: bash

    ./process_psc.sh /Path_to_Datafolder/subjectlist /Path_to_Datafolder /opt/SBCI_Pipeline/ABCD_example

PSC processing steps include:

1. Fiber tracking.
2. Computing Particle Filtering Tractography (PFT) maps.
3. Streamline tracking.
4. Connectivity matrix computation.


3. Run SBCI Process
^^^^^^^^^^^^^^^^^^

As with the previous steps, use ``/opt/SBCI_Pipeline/ABCD_example/process_sbci.sh``. 
Modify the paths in process_sbci.sh before execution. 
``process_sbci.sh`` requires three input parameters: subject list location, output location, and script location.

Example Command for SBCI Process:

.. code-block:: bash

    ./process_sbci.sh /Path_to_Datafolder/subjectlist /Path_to_Datafolder /opt/SBCI_Pipeline/ABCD_example

SBCI processing steps include:

1. Processing SBCI grids.
2. Preprocessing datasets.
3. Running datasets.
4. Processing surface data.
5. Structural data analysis.
6. Functional data analysis.


4. Quality Control (QC)
^^^^^^^^^^^^^^^^^^^^^^^
The Quality Control (QC) process is integral to ensuring the integrity of data processing. 
It involves checking whether necessary files have been correctly created at each step of the workflow. 
For this purpose, we utilize three specific shell scripts: ``preprocess_qc.sh``, ``psc_qc.sh``, and ``sbci_qc.sh``.
Each of the script takes three arguments: IN (subject list); DATA (path to find data); and  OUT (path to write the QC log). 

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



5. Clean Subject Folder
^^^^^^^^^^^^^^^^^^^^^^^

After the processing is complete, there are some final finishing touches. These include deleting unnecessary files and organizing the files in the output folder into one folder.
These can be done by using the ``PATH/SBCI_Pipeline/ABCD_example/clean_subject_folders.sh`` script.

.. code-block:: bash

    ./clean_subject_folders.sh /Path_to_Datafolder/subject_list /Path_to_Datafolder /opt/SBCI_Pipeline/ABCD_example

The final results have a folder for each subject containing all the result files, both structural and functional, 
named ``psc_sbci_final_files``. The path is ``/Path_to_Datafolder/subject_list/psc_sbci_final_files``.