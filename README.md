[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/4906)
# nesi-virtual-desktops


## Usage
### Basic
See [Connecting to a Virtual Desktop](https://support.nesi.org.nz/hc/en-gb/articles/360001600235-Connecting-to-a-Virtual-Desktop).
### Through JupyterHub
...coming soon

## Files
```
vdt/
├─ lockfiles/
├─ tools/
│  ├─ singularity_runscript
│  ├─ singularity_startscript
│  ├─ lic.sh
│  ├─ nesi_websockify_patch
│  └─ start_from_jupyter
├─ templates/
│  ├─ default
│  ├─ eng/
|  └─ eng_dev/
├─ vdt
├─ vdt_start
├─ vdt_list
├─ vdt_shell
├─ vdt_kill
└─ vdt_clean
```
### `lockfiles/`
Where lockfiles are currently being put. Should be empty in this repo.

### `tools/`
For various scripts that arn't supposed to be user facing.

#### `singularity_runscript.sh`
Script for singularity %runscript
Currently just points to singularity_startscript
#### `singularity_startscript.sh`
Script for singularity %startscript
#### `lic.sh`
Checks for licences on set paths, exports relevent env variables if user can read.
#### `nesi_websockify.diff`
A patchfile used during container build.
#### `start_from_jupyter.sh`
Used by the jupyterHub socket proxy.

### `templates/` 
Determines the type of desktop launched.
'default' will be used if no base type specified.
Currently only templates are:
* `default` - symlink to 'eng'
* `eng` - Generic engineering desktop. Creates icons for engineering applciations user has a licence to use, as well as some useful tools and links to all of the users. Currently using image [here](https://github.com/nesi/nesi-singularity-recipes/tree/master/centos/turbo_xfce_centos) directories.
* `eng_dev` - Same as above but for testing.
Template contents.

*for the template named 'example'*
```
..
├─ templates/
|  ├─ default
|  ├─ example/
|  |  ├─ image
|  |  ├─ Singularity.example
|  |  ├─ pre.sh
|  |  ├─ posh.sh
|  |  └─ setup.sh
..
```
#### `image`
singularity image file (or symlink to image file).

#### `Singularity.example` (optional)
Recipie file for 'image'

#### `pre.sh` (optional)
Script to be sourced in base enviroment before launching container.

#### `post.sh` (optional)
Script to be sourced in container on launch.

#### `setup.sh` (optional)
Script to be sourced in container on the first launch.
Currently determined by whether $XDG_CONFIG_HOME/xfce/ exists, but this is bad.

-- generate these from help cmd --
### `vdt`

### `vdt_start`
### `vdt_list`
### `vdt_shell`
### `vdt_kill`
### `vdt_clean`

## Enviroment Variables
All of these variables are passed to the container during start.

### VDT_ROOT

Location of this repo.

### VDT_HOME 

Location of this repo.

`"$HOME/.vdt"`

### VDT_LOCKFILES
Location of lockfiles.

`"$VDT_ROOT/lockfiles"`
### VDT_TEMPLATES 
Location of templates. See templates.

`"$VDT_ROOT/templates"`
### VDT_BASE
Selected template.

`"default"`
### VDT_INSTANCE_NAME
Name used for Singularity instance.

`"${VDT_BASE}_${USER}"`
### VDT_LOGFILE
Location of logfile.
Amalgumation of
* Messages from these scripts.
* vncserver -log

`"${VDT_HOME}/${VDT_INSTANCE_NAME}.${remote:-$(hostname)}:${VDT_SOCKET_PORT}.log"}"`
### VDT_SOCKET_PORT
Forwarded port used to connect to websockify. 
Must be input by user.
### VDT_DISPLAY_PORT
Display port used by vnc.
Random nubmer between 1100 and 2000
### VDT_WEBSOCKOPTS
Additional options to pass to websockify.
### VDT_VNCOPTS
Additional options to pass to vnc.

## Other Enviroment Variables
### XDG_CONFIG_HOME
Location of desktop setup.

`"$HOME/.config"`
