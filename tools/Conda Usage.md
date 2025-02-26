# Conda Usage

## Environment management

Managing environments is crucial for maintaining different projects with distinct dependencies.

### Create and manage environments
List all existing environments.

```bash
conda env list
```

Create a new environment.

```bash
conda create --name env_name python=3.8
```

Activate the specified environment.

```bash
conda activate env_name
```

Deactivate current environment and return to the base environment.

```bash
conda deactivate
```

Delete the specified environment.

```bash
conda remove --name env_name --all
```

### Export and re-create environments
Export current environment settings to a yaml file.

```bash
conda env export --name myenv > myenv.yml
```

Re-create an environment from a yaml file.

```bash
conda env create -f myenv.yml
```

Produce a file listing all packages in the current environment with their exact versions and download urls, suitable for replicating the environment precisely across different systems or without original channel access.

```bash
conda list --explicit > spec-file.txt
conda create --name myenv --file spec-file.txt
```

## Package management
Effective package management is key to ensuring that software runs reliably.

### Basic operations
List packages installed in the active environment.

```bash
conda list
```

Install a new package to the active environment.

```bash
conda install package_name
```

Use `-c` to install from a specific channel.

Update an installed package.

```bash
conda update package_name
```

Remove a package from the environment.

```bash
conda uninstall package_name
```

### Cleanup
Remove unused packages and clear caches to free disk space.

```bash
conda clean -p  # Remove unused packages
conda clean -t  # Remove zip packages
conda clean -y  # Remove with auto-confirm
```
Use `-h` for detailed help.
