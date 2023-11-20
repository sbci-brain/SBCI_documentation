Run the SBCI and PSC pipelines
--------------------------------


4. Run the SBCI and PSC pipelines
---------------------------------

a. Change ``sbci_config`` (*Optional)

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    PSC_PATH=/PSC_Pipeline/
    SBCI_PATH=/opt/SBCI_Pipeline/
    FREESURFER_PATH=/usr/local/freesurfer/
    OUTPUT_PATH=/opt/SBCI_Pipeline/ABCD_example/SBCI_AVG/

b. Make a subject list
^^^^^^^^^^^^^^^^^^^^^

.. code-block:: bash

    cd /opt/SBCI_Pipeline/ABCD_example/Original
    ls -d */*/ > subject_list_all

c. Run Preprocess
^^^^^^^^^^^^^^^^
.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/preprocess.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

d. Run PSC
^^^^^^^^^^

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/process_psc.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

e. Run SBCI
^^^^^^^^^^^

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/process_sbci.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

5. QC
-----

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/preprocess_qc.sh.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example/Original

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/psc_qc.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/sbci_qc.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

6. Clean Subject Folder
-----------------------

.. code-block:: bash

    source /opt/SBCI_Pipeline/ABCD_example/clean_subject_folders.sh /opt/SBCI_Pipeline/ABCD_example/Original/subject_list_all /opt/SBCI_Pipeline/ABCD_example/Original /opt/SBCI_Pipeline/ABCD_example

