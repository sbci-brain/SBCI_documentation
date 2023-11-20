.. SBCI documentation documentation master file, created by
   sphinx-quickstart on Tue Oct  3 19:24:07 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to SBCI documentation!
==============================================
Surface-Based Connectivity Integration (SBCI) is an advanced MRI post-processing pipeline that builds structural connectivity (SC) and functional connectivity (FC) on the white matter surface of the brain and returns three imaging biomarkers representative of the relationships between SC and FC.

What you will need:

1. T1w anatomical data
2. Multi-shell dwi data
3. Resting-state fMRI data

Before utilizing the FSL software's pipeline, the premise is that the data have only undergone minimal preprocessing. Specifically, within this pipeline, eddy correction and susceptibility-induced distortion correction are performed.

Reference:

   Martin Cole, Kyle Murray, Etienne St‐Onge, Benjamin Risk, Jianhui Zhong, Giovanni Schifitto, Maxime Descoteaux, and Zhengwu Zhang. "Surface‐Based Connectivity Integration: An atlas‐free approach to jointly study functional and structural connectivity." Human Brain Mapping 42, no. 11 (2021): 3481-3499.

.. toctree::
   :maxdepth: 1
   :caption: Install

   installation/install_index
   installation/method1
   installation/method2
   installation/method3

