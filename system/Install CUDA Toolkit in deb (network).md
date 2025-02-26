# Install CUDA Toolkit in deb (network)

Use [CUDA Toolkit 11.8 Downloads](https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_network) as an example, when you follow the instructions in **deb (network)**.

![cuda](img/Install%20CUDA%20Toolkit%20in%20deb%20(network)/cuda.png)

**Do not** run the last command

```bash
sudo apt-get -y install cuda # DO NOT
```

The command `install cuda` will install the **latest version of all CUDA Toolkit and Driver packages** once it is released, which is shown in [cuda-installation-guide-linux/package-manager-metas](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#package-manager-metas).

![package-manager-metas](img/Install%20CUDA%20Toolkit%20in%20deb%20(network)/package-manager-metas.png)

List all packages start with `cuda` instead, then choose the specific one.

```bash
sudo apt list cuda*
```
