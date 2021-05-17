## Magnetic Resonance Conductivity Imaging (MRCI) - Preproccessing
___
This is a generally applicable for **MRI B1+ phase** and **Diffusion Tensor Imaging** preprocessing for the reconstruction of anisotropic conductivity imaging. [Conductivity tensor imaging](https://ieeexplore.ieee.org/document/8556029) is a clinically feasible method for anisotropic conductivity imaging. CTI does not require any imaging electrical current to map conductivity of biological tissues such as human brain and also does not need any complicated approach in reconstruction process, thus facilitating its widespread use in neuroscience research and clinical neuromodulation treatment planning. 

CTI technique requires only two MR imaging sequences. 
1. B1 mapping for MREPT reconstruction.
2. Diffusion weighted imaging (DWI) for microsctructure mapping. 

This repository describes the basic preprocessing steps included in the MRCTI reconstructions. These preprocessing steps are as follows:

1. Reconstruction of optmized phase and magnitude maps from RAW MREPT data.
    1.  Basic example-Raw kspace data from siemens & philips MR scanner from MSME sequence used in [human brain study](https://ieeexplore.ieee.org/document/8556029).
    2.  [Denoising of kspace and gibbs artefacts removal](https://pubmed.ncbi.nlm.nih.gov/26745823/)
    3.  [Multi-channel coil combination](https://onlinelibrary.wiley.com/doi/full/10.1002/%28SICI%291522-2594%28200005%2943%3A5%3C682%3A%3AAID-MRM10%3E3.0.CO%3B2-G?sid=nlm%3Apubmed)
    4.  [Phase unwrapping using PUMA](https://ieeexplore.ieee.org/document/4099386) 
    5.  [Weight factor calculation for multi-echoes combination](https://biomedical-engineering-online.biomedcentral.com/articles/10.1186/1475-925X-13-24)

2. Preprocessing steps and artefacts correction of DWI [Courtesy-ISMRM 2021-Diffusion tutorial]
    1. Masking
    2. Signal drift correction
    3. Gibbs ringing correction
    4. Rician bias correction
    5. Denoising
    6. Motion correction within volumes and between volumes
    7. EPI distortion & Eddy-current correction
    8. Susceptibitly distortion correction
    9. Geometrical distortion correction
 
**Important notice:** Most of the preprocessing steps mentioned above uses FSL, mrtrix3 and ANTs. For the installation of these all softwares in you windows-PC. Please follow these instructions. 
___
![image](https://user-images.githubusercontent.com/14322345/118450197-e4cdc000-b72e-11eb-9dde-308d4da7f1dd.png)

How to get FSL, Freesurfer, MRtrix3 and ANTs on to a Windows machine using the Windows Subsystem for Linux (WSL). All of these instructions require Windows 10 or higher. It should be noted that atleast of 40 gb free space is required for the smooth run of all linux based softwares.

**Step 1.**	
  Enable “Windows Subsystem for Linux” feature using 2 options:

*Option 1:* Check the appropriate box within “Turn Windows features on or off” dialog box. (Open Settings -> Search “programs” -> Select “Turn Windows features on or off”) Step 1b: You will need to restart your computer, as prompted, after this feature is enabled.

*Option 2:* Use Command-Line/PowerShell by pasting in the code below:
```
Enable -WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux 
```

**Step 2.**	
  Install Ubuntu 20.04 application from Microsoft store.

  Search Microsoft office on search panel and search Ubuntu and install Ubuntu 20.04. Launch the Ubuntu and set up user name: suggestion starts with small letter only. Set up basic updates like compiler g++, gcc and python3 by running command given below. 

```
sudo apt update && upgrade
sudo apt install build-essential 
sudo apt-get install dc python mesa-utils gedit pulseaudio libquadmath0
```
Note: Every time you give sudo command it will ask for password it the same password used while creating user name. Additional installation will ask for permission for memory space. Please press **y**.
____
![image](https://user-images.githubusercontent.com/14322345/118450555-47bf5700-b72f-11eb-854f-c418092443d4.png)

**Step 1.**
  Download the FSL installer file
  File download link : https://fsl.fmrib.ox.ac.uk/fsldownloads_registration
  Download FSL for Ubuntu 20.04.

**Step 2.**	
	Download installation file of fslinstaller.py in downloads.
  Give this command:
```
python /mnt/c/Users/<WindowsUsersName>/Downloads/fslinstaller.py 
```
**Step 3.**	
	Follow all step and save fsl /home/user/fsl/ and Say yes to all default install options and give password

Restart the Ubuntu terminal again and run: **bet**

**Step 4.**	
	Window do not give permission to use X-windows of glx interface for ubuntu apps. You have to install either X-ming: https://sourceforge.net/projects/xming/ or sudo apt-get install x11-apps
  
 ```
 echo "export DISPLAY=localhost:0.0" >> ~/.bashrc
 ```
For more deatailed information please go to this website: https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation/Windows#WSL_2_Changes_.28_provisional_.29

*Tips:* fsl installer only works with python version below 2.3. If you want to reduce the size of fsl software you can delete the atlases or surface files from standard/data/atlases folder. This will reduces your ubuntu space in C-drive. 
____
![image](https://user-images.githubusercontent.com/14322345/118452123-d680a380-b730-11eb-8abb-a90369b56bfd.png)
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh 
source ~/.bashrc
rm Miniconda3-latest-Linux-x86_64.sh 
conda install –c mrtrix3 mrtrix3
```
conda config --set auto_activate_base false #to deactivate base

![image](https://user-images.githubusercontent.com/14322345/118452178-e1d3cf00-b730-11eb-8e5c-6e98200cc136.png)


**Step 1.**	
	Run these commands for preinstallation of platforms for installation of ANTs
```
sudo apt-get -y install build-essential 
sudo apt-get -y install cmake
sudo apt-get -y install cmake-curses-gui
sudo apt-get -y install zlib1g-dev 
sudo apt-get git
```
**Step 2.**
	Here is a very minimal example. This example works if you have all the necessary tools, including CMake and a supported compiler. The system requirements and installation steps are discussed in more detail below.

*Option 1*
Run installANTs on bash.

```
chmod +x installANTs.sh
```
or
```
./installANTs.sh
```
*Option 2* 
Clone git repository
```
workingDir=${PWD}
git clone https://github.com/ANTsX/ANTs.git 
mkdir build install
cd build 
cmake \
 -DCMAKE_INSTALL_PREFIX=${workingDir}/install \
 ../ANTs 2>&1 | tee cmake.log
make -j 4 2>&1 | tee build.log
cd ANTS-build 
make install 2>&1 | tee install.log 
```
Follow this link for more information: https://github.com/ANTsX/ANTs/wiki/Compiling-ANTs-on-Linux-and-Mac-OS#get-the-latest-code




