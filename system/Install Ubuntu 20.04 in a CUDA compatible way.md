# Install Ubuntu 20.04 in a CUDA compatible way

*for Ubuntu 20.04 LTS*

*from USB drive*

## Preparations

Insert the bootable Ubuntu USB drive

`F2` enter `UEFI Settings`

`F7` enter `Advanced Mode`

​	`Security` - `Secure Boot` - `Disabled` - `F10` Save

​	Set `SanDisk` as `Boot Option #1` - `F10` Save

`F8` enter `Boot Menu`

​	Select `SanDisk`

## Install Ubuntu

Enter `GRUD Menu` 

​	Press `E` to edit

​	Find line `file=/cdro/preseed/ubuntu.seed maybe-ubiquity quiet splash ---`

​	Delete `---` and add `nouveau.modeset=0` to disable the default driver 

​	The line will be like `file=/cdro/preseed/ubuntu.seed maybe-ubiquity quiet splash nouveau.modeset=0`

​	`F10` to boot

Install Ubuntu

Remove the installation USB and restart

## Install CUDA

Install CUDA dependencies

```bash
sudo apt-get update -y
sudo apt upgrade -y
sudo apt-get install build-essential ca-certificates g++ curl -y
sudo apt-get install -y software-properties-common
```

Remove older/other CUDA

```bash
# To remove CUDA Toolkit
sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*" "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" "*nvvm*"

# To remove NVIDIA Drivers
sudo apt-get --purge remove "*nvidia*" "libxnvctrl*"

# To clean up the uninstall
sudo apt-get autoremove
```

Go [CUDA Toolkit Downloads | NVIDIA Developer](https://developer.nvidia.com/cuda-downloads) to choose version then follow the instructions to install

The CUDA Toolkit installation includes a specific version of the NVIDIA Driver to ensure compatibility with CUDA versions. 

Therefore, the NVIDIA Driver will be installed automatically for the current CUDA version when installing CUDA Toolkit.

Use CUDA Toolkit 11.8 as an example

![image](img/Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way/cuda.png)

Restart

```bash
sudo reboot
```

Check NVIDIA Driver and CUDA version

```bash
nvidia-smi
nvcc -V
```

What may happen is that

The NVIDIA Driver runs normally but the CUDA Toolkit version cannot be found.

The system will recommend installing CUDA Toolkit. 

![image](img/Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way/Screenshot%20from%202024-02-08%2004-16-31.png)

## Attention

**<u>DO NOT</u>** do that, or another CUDA Toolkit will be installed, most likely version 10.1.xxx

In fact, CUDA Toolkit 11.8 has been installed, but it is not configured in the environment variables.

Enter the CUDA installation directory, most likely

```bash
cd /usr/local 
```

or you can use this command to directly see whether the folder exists

```bash
nautilus /usr/local
```

The cuda in the directory is usually a **symbolic link** that points to a specific version of CUDA directory. This makes it easier for users to call in an environment where multiple CUDA coexist.

![image](img/Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way/Screenshot%20from%202024-02-08%2004-14-27.png)

Edit environment configuration files

```bash
nano ~/.bashrc
```

Add at the end of the file

```bash
# cuda
export LD_LIBRARY_PATH=/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
```

Press `Ctrl + O` and then press the `Enter` key to save changes

Press `Ctrl + X` to exit from the nano text editor

![image](img/Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way/Screenshot%20from%202024-02-08%2004-25-33.png)

Make configuration effective

```bash
source ~/.bashrc
```

Check whether nvcc runs normally

```bash
nvcc -V
```

![image](img/Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way/Screenshot%20from%202024-02-08%2004-27-50.png)
