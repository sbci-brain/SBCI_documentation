Prerequisites
=================
Required Software
-----------------------------------------
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
    and the importance of this setup are elaborated in the 'Using a Bashrc File' section later in this documentation.


Alternative Installation of Prerequisites
-----------------------------------------
If you're using a system without module management or if some of the necessary files listed above are missing, the prerequisites can be installed manually:

    * Anaconda: Follow the insctructions from `Anaconda`_. After following the installation guidelines, verify the setup by running ``which python``. This will return the correct Python version. If the result doesn't reflect the Anaconda Python path or you encounter issues, adjust your ``.bashrc_sbci`` to include Anaconda's directory: ``export PATH="/PATH/TO/ANACONDA/bin:$PATH"``, replace ``/PATH/TO/ANACONDA/`` with your installation location.

.. _Anaconda: https://docs.anaconda.com/free/anaconda/install/linux/

    * Freesurfer: Follow the insctructions from `Freesurfer_6`_. If using Ubuntu, ``libpng12`` might be an issue; solution is provided by the following link: https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html.

.. _Freesurfer_6: https://surfer.nmr.mgh.harvard.edu/fswiki/rel6downloads

    * mrtrix: Use the following command to install mrtrix3: ``conda install -c mrtrix3 mrtrix3``.

..

    * ANTs: Download and unzip the file from `ANTs`_ and copy the contents to an appropriate directory, such as ``/home/yourname/Software/ANTs``. After placing ANTs in your chosen directory, you'll need to set up your system to recognize its path. To do this, open your ``.bashrc_sbci`` file (or another script file that's executed upon login) and append the following lines to ensure your system can locate ANTs:

.. _ANTs:  http://stnava.github.io/ANTs/

:: 

    export ANTSPATH=/home/yourname/Software/ANTs/install/bin #This sets the path to the ants bin directory.
    export PATH=${ANTSPATH}:$PATH
    export PATH="$HOME/Software/ANTs/Scripts:$PATH"

..

    Once you've added these lines and saved the file, validate your installation. Running the command ``which antsRegistration`` should display the full path to ``antsRegistration``. Similarly, executing ``antsRegistrationSyN.sh`` should output the script's usage instructions, confirming its accessibility and execution readiness.

    * FSL: Follow the instructions from `FSL`_.

.. _FSL: https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation/