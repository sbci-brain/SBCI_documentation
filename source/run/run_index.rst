General Instructions for the Run
=============================================

Before running the software, ensure the data are followed the ``BIDS`` format, refer to the BIDS website for more details: https://bids.neuroimaging.io/.

1. Subject-Specific Directories
For each subject, prepare the following three folders under ``Data_Path/Subject_ID/``:

``anat``: Contains T1-weighted Imaging Files.

``dwi``: Includes Diffusion Weighted Imaging (DWI) Files, b-value Files, and b-vector Files.

``func``: Stores Functional MRI Data Files.

2. SBCI Configuration File

``PATH/SBCI_Pipeline/sbci_config.json``: Modify the sbci_config.json file according to your specific task requirements.

3. Subject List File

``Data_Path/subjectlist.txt``: This file should list the specific data storage locations for all subjects.


For more details, please refer to the following pages:

1. :ref:`run_HPC`

2. :ref:`run_Docker`







