# HPC Workspace Data and Software Tools

!!! attention "under Construction"
    Workspaces are still in testing phase and not publicly available yet.
    This is prelimnary information.
    Detailed information will follow soon.

## Description
The Workspace module provide support for userfriendly file system access, custom software stacks in HPC Workspaces, and SLURM accounting. 

## Workspace module
The Workspace module adjust the environment to work in a spacific Workspace. 
```
module load workspace
```
sets the following environment variables ([Shortcuts](#shortcuts)) and [Software stacks](#software-stacks)

There are the following possibilites:

- you belong to **no** Workspace: load `module load Workspace/home` to use your software stack in your HOME directory
- you belong to **one** Workspace: this one gets loaded when `module load Workspace`
- you belong to multiple Workspaces: you need to specify one. The module presents you the options, e.g.
    ```
    $ module load Workspace
    Workspaces are available:
        HPC_SW_test, hpc_training, 
    Please select and load ONE of the following:
        export HPC_WORKSPACE=HPC_SW_test; module load Workspace
        export HPC_WORKSPACE=hpc_training; module load Workspace
    ```
    - Thus you load a specific Workspace using:
    ```
    export HPC_WORKSPACE=<WorkspaceName>; module load Workspace
    ```

### Shortcuts
The workspace module provides the following variables:

|  <div style="width:180px">Variable</div> | Function |
| -------- | -------- |
| `$WORKSPACE` | full path to the Workspace. Thus, you can access the workspace using: `cd $WORKSPACE` |
| `$SCRATCH`  | full path to the Workspace SCRATCH diretory. Thus you can access it using: `cd $SCRATCH` |


### Additional Settings
the module provides the following settings for a more user-friendly usage of applications. You may not need to use them directly, but tools like SLURM, Singularity and Python will use them. 

|  <div style="width:180px">Variable</div> | Function |
| -------- | -------- |
| `$SBATCH_ACCOUNT` | sets the SLURM account to the Workspace account. Thus all submitted jobs with that module are accounted to the Workspace account automatically. No need to set it in the sbatch script |
| `$SINGULARITY_BINDPATH` | using singularity, the Workspace directory will be bind into the container without manual specification. The WORKSPACE variable will also be ported into the container. Thus, you can specify locations with $WORKSPACE within the container. | 
| `$PYTHONPATH` | if `Python` or `Anaconda` is loaded beforehand, it is set to: `$WORKSPACE/PyPackages/lib/pythonXXX/site-packages` where `XXX` is the Python major and minor version. And also add the `bin` dorectory to `$PATH`. |
| `$PYTHONPACKAGEPATH` | if `Python` or `Anaconda` is loaded beforehand, it is set to: `$WORKSPACE/PyPackages`. This can be used for e.g. `pip install --prefix $PYTHONPACKAGEPATH` |

### Software stacks
Beside, a set of software packages we provide for our different CPU architecture, the Workspace module provides tools to install custom software stacks within your Workspace. 
Especially with easybuild shortcuts are provided to install and use custom software stacks easily build for all architectures. 

For installing packages with EasyBuild, see [Easybuild description](../software/EasyBuild.md). 
Manual package can also be installed in the similar manner. Adding a Modulefile provides the users to load packages as used to. Please see [Installing Custom Software](../software/installing-custom-software.md). 

As a result all users of the Workspace can use the software packages by loading the Workspace module and the software product module. 

### Python packages
The Workspace module also provides support to install and use Python packages in a shared manner, by installing them into the Workspace. 
Please see [Additional Packages](../software/python.md#additional-packages)

### UMASK
The Workspace module sets the umask to 002. Thus files and directories get group-writeable, e.g.:
```Bash
-rw-rw-r-- 1 user group 0 Mar 15 15:15 /path/to/file
```
