# Install NVIDIA Driver on Ubuntu 20.04

*for ubuntu 20.04 lts*

## 准备工作

检查是否安装显卡驱动

```bash
nvidia-smi
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2000-47-19.png)

从硬件级别检测显卡

```bash
lspci | grep -i nvidia
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2000-50-00.png)

根据 PCI 号查询对应的显卡型号

[PCI devices (ucw.cz)](https://admin.pci-ids.ucw.cz//mods/PC/10de?action=help?help=pci)

根据显卡型号查询对应驱动版本

[Official Drivers | NVIDIA](https://www.nvidia.com/Download/index.aspx)

## 安装

可通过图形化界面或命令行安装驱动

### 图形化界面

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2000-50-58.png)

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2000-53-09.png)

### 命令行

检查可安装的驱动

```bash
ubuntu-drivers devices
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2000-50-37.png)

```bash
sudo apt install nvidia-driver-535
```

或自动安装默认推荐驱动

```bash
sudo ubuntu-drivers autoinstall
```

重启后运行

```bash
nvidia-smi
```

可显示结果则安装成功且驱动已运行

## 问题与解决方案

报错

```
NVIDIA-SMI has failed because it couldn't communicate...
```

![iamge](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2001-47-51.png)

检查 NVIDIA 内核是否成功加载

```bash
lsmod | grep nvidia
```

如无输出，说明驱动未加载，尝试手动启动

```bash
sudo modprobe nvidia
```

如报错，可能是 Secure Boot 阻止了操作

```
...Operation not permitted
```

大致有 3 种可能导致包括重启后黑屏在内的以上问题

### Nouveau 未禁用

Nouveau 是默认的驱动程序，需要将 Nouveau 加入黑名单，禁止它在系统启动时加载，否则会与 NVIDIA 驱动冲突。

检查 Nouveau 是否已禁用

```bash
lsmod | grep nouveau
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2001-23-21.png)

在终端打开

```bash
sudo gedit /etc/modprobe.d/blacklist.conf
```

在末尾添加

```bash
blacklist nouveau
options nouveau modeset=0
```

返回终端执行命令应用更改

```bash
sudo update-initramfs -u
```

重启后再次验证，如无输出，则说明已禁用

### 系统内核版本与驱动版本冲突

如果系统内核更新后，NVIDIA 驱动没有相应更新，或者驱动不兼容当前内核版本，会导致 NVIDIA 驱动无法正确加载。

显示当前正在运行的内核版本号

```bash
uname -r
```

显示与 NVIDIA 显卡驱动相关的 DKMS 模块的当前状态

```bash
dkms status nvidia
```

列出所有已安装的系统内核

```bash
dpkg --get-selections | grep linux-image 
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2001-53-41.png)

确保当前运行的内核版本号与 DKMS 模块匹配

### Secure Boot 阻止了驱动加载

如果启用了 Secure Boot，系统会阻止未签名的驱动加载。NVIDIA 驱动需要在 Secure Boot 禁用或正确配置的环境中加载。

检查 Secure Boot

```bash
mokutil --sb-state 
```

重启时进入 BIOS 修改或通过终端禁用

```bash
sudo mokutil --disable-validation
```

## 重新安装

如问题仍存在，参考以下流程

卸载已有的 NVIDIA 显卡驱动

重启时，按住`shift`访问 GRUB 菜单，选择`Advanced options for Ubuntu`，进入指定内核的`Recovery Mode`，选择`root`进入命令行，输入以下代码卸载已安装的驱动后重启

```bash
# To remove CUDA Toolkit
sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*" "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" "*nvvm*"

# To remove NVIDIA Drivers
sudo apt-get --purge remove "*nvidia*" "libxnvctrl*"

# To clean up the uninstall
sudo apt-get autoremove
```

确认 Nouveau 和 Secure Boot 已禁用后重新下载驱动。Secure Boot 如未禁用，下载驱动后会被要求设置 Secure Boot 的密码。重启进入蓝屏后，选择 Enroll MOK 并输入先前设置的 Secure Boot 的密码。选择 reboot 再次重启后，确认内核版本与驱动版本匹配，确认 NVIDIA 驱动已加载。检查显卡驱动。

```bash
nvidia-smi
```

![image](img/Install%20NVIDIA%20Driver%20on%20Ubuntu%2020.04/Screenshot%20from%202024-02-05%2001-51-10.png)

如果仍然存在无法开机等问题，在重装 Ubuntu 时禁用 Nouveau 以解决冲突问题。

参考 [Install Ubuntu 20.04 in a CUDA compatible way.md](Install%20Ubuntu%2020.04%20in%20a%20CUDA%20compatible%20way.md)
