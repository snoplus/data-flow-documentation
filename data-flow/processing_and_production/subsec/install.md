---
layout: default
title: Install tagged software
nav_exclude: true
---

## Installing tagged software on CVFMS

For older documentation which contains more information, see here: [DocDB 2528](https://www.snolab.ca/snoplus/private/DocDB/cgi/ShowDocument?docid=2528).

Requirements:
* Access to the SNO+ CVMFS server at RAL; email Fady Shaker (fshaker@ualberta.ca) with your certificate DN to gain access. Fady will then send a request to the CVMFS support team on your behalf.
* For reference, your certificate DN should be of a format similar to: 
  ```
  subject : /C=CA/O=Grid/OU=ORGANIZATION/CN=NAME/CN=NUMBER issuer : /C=CA/O=Grid/OU=ORGANIZATION/CN=NAME identity : /C=CA/O=Grid/OU=ORGANIZATION/CN=NAME
  ```
* Sign up for cvmfs notifications and service disruptions by sending an email to (listserv@jiscmail.ac.uk).
* Virtual machine for the appropriate operating system:
  1. Install [VirtualBox](https://www.virtualbox.org/).
  1. Follow the VirtualBox instructions to setup a virtual machine for the desired OS (e.g. for SL6 on a MacBook Air 1.5 GB RAM & 20 GB HD are appropriate).
  1. Download an appropriate (64-bit) disk image for the desired OS (e.g. for SL6 download the InstallDVD from the SL [downloads page](https://scientificlinux.org/downloads/)).
  1. Install the OS (note that after installation of SL6 you may have no network connection on the VM, this may be solved using nm-connection-editor via this [solution](https://unix.stackexchange.com/questions/78295/centos-no-network-interface-after-installation-in-virtualbox)).
  
  You should now start the VM and setup the following:
  * Install git (via yum) and GitHub access (either ssh keys or with OAuth tokens).
  * Setup a Grid UI and copy of your grid certificate.
  * Configure access to CVMFS, ensure that `CVMFS_HTTP_PROXY=DIRECT` is set in `/etc/cvmfs/default.local`.
  * Install [snoing](https://github.com/snoplus/snoing). Note that you may need to update both versions/ratversion.py and versions/rattoolsversions.py to include the most recent release.
  
  You are now ready to install new SNO+ libraries to CVMFS. Any system libraries that are required from this point on (i.e. not installed within the SNO+ cvmfs repository) must be added to the SNO+ VOID card at [https://operations-portal.egi.eu/vo](t [https://operations-portal.egi.eu/vo). This list currently consists of:
  * gcc-c++
  * python-devel
  * uuid-devel
  * zlib-devel
  
All of which should be installed via yum on the virtual machine.

We do not want to install all dependency packages (both to save install time and to avoid library clashes if two separate VMs are used to install software to CVMFS). This step will only need to occur the first time you set up your system and any time a new dependency or dependency version is added. Each time software is installed, setup symbolic links between a local install directory and cvmfs dependencies:
```bash
# First, create a local install folder:
mkdir $HOME/cvmfs_link

# Also create a cvmfs folder with a structure that mirrors that on the cvmfs server:
# Replace sl6 with sl7 when you are running an SL7 virtual box
mkdir -p $HOME/cvmfs/sl6/sw/dependencies

# Now link all libraries that are already available in CVMFS (ensure you're copying libraries for the correct OS):
cd $HOME/cvmfs_link
# Replace sl6 with sl7 when you are running an SL7 virtual box
for d in $(ls /cvmfs/snoplus.egi.eu/sl6/sw/dependencies); do (ln -s /cvmfs/snoplus.egi.eu/sl6/sw/dependencies/$d); done

# For some reason, curl must be installed locally (you should only need to do this the first time!)
unlink curl-7.26.0
```
Now, run snoing to install the version of SNO+ software you desire:
```bash
# Replace x.y.z with the RAT version!
./snoing.py -i $HOME/cvmfs_link -x -u [github-username] rat-x.y.z
```
Move any installed libraries over to local cvmfs directory structure:
```bash
# E.g. if we just installed root-5.34.30, move it to the dependencies folder
# Replace sl6 with sl7 when you are running an SL7 virtual box
mv $HOME/cvmfs_link/root-5.34.30 $HOME/cvmfs/sl6/sw/dependencies
```
**Option A: Upload by hand**
```bash
# E.g. move any SNO+ specific software to a folder with the named version number
# Replace x.y.z with the RAT version!
# Replace sl6 with sl7 when you are running an SL7 virtual box
mkdir $HOME/cvmfs/sl6/sw/x.y.z mv
mv $HOME/cvmfs_link/rat-x.y.z $HOME/cvmfs/sl6/sw/x.y.z
mv $HOME/cvmfs_link/env_rat-x.y.z.* $HOME/cvmfs/sl6/sw/x.y.z
```
Before pushing to cvmfs, we have to ensure that the environment files are setup correctly. E.g. for RAT:
* Replace the local path to the dependencies folder (e.g. /home/user/cvmfs_link) with `$SNOPLUS_CVMFS_DIR/sw/dependencies`
* Replace the local path to the SNO+ folder (e.g. /home/user/cvmfs_link/rat-x.y.z) with `$SNOPLUS_CVMFS_DIR/sw/x.y.z/rat-x.y.z`
* All of the following will require editing:
  * `$HOME/cvmfs/sl6/sw/x.y.z/env_rat-x.y.z.sh`
  * `$HOME/cvmfs/sl6/sw/x.y.z/env_rat-x.y.z.csh`
  * `$HOME/cvmfs/sl6/sw/x.y.z/rat-x.y.z/env.sh`
  * `$HOME/cvmfs/sl6/sw/x.y.z/rat-x.y.z/env.csh`

Now ensure that software is readable by all by running the following two commands:
```bash
# Run the following from $HOME/cvmfs
chmod -R a+r .
find . -perm /u+x -print0 | xargs -L1 -0 chmod a+x
```
Finally, we're ready to upload the new software to CVMFS:
```bash
# Ensure you have a valid grid proxy
rsync -avz -e 'gsissh -p 1975' $HOME/cvmfs/sl6 cvmfs-upload01.gridpp.rl.ac.uk:~/cvmfs_repo
```

**Option B: Upload by script**
It can be better to have a small script to handle all the manipulations instead of doing everything by hand each time. Split into two step, there here is a prepare_cvmfs.sh and a upload_cvmfs.sh that can be used instead. You will need to adjust the script to match with your computer and if you are running on SL6 or SL7.

#### prepare_cvmfs.sh
This script will take an input of a rat version string and first move the copy of RAT to the right directory, then replace all the local paths with cvmfs paths. It will also ensure all permissions are correct for the upload to the grid. This script is then run from the $HOME directory by `./prepare_cvmfs.sh x.y.z`.
```bash
#!/bin/bash

# create new directory for rat and move output of snoing
mkdir $HOME/cvmfs/sl7/sw/${1}
mv $HOME/cvmfs_link/rat-${1} $HOME/cvmfs/sl7/sw/${1}
mv $HOME/cvmfs_link/env_rat-${1}.* $HOME/cvmfs/sl7/sw/${1}

# Replace some stuff
OLD_LINK="/home/gilje/cvmfs_link"
NEW_LINK="\$SNOPLUS_CVMFS_DIR/sw/dependencies"
OLD_RAT="/home/gilje/cvmfs_link/rat-${1}"
NEW_RAT="\$SNOPLUS_CVMFS_DIR/sw/${1}/rat-${1}"


echo $OLD_LINK $NEW_LINK $OLD_RAT $NEW_RAT

sed -i "s|${OLD_RAT}|${NEW_RAT}|g" $HOME/cvmfs/sl7/sw/${1}/env_rat-${1}.sh
sed -i "s|${OLD_LINK}|${NEW_LINK}|g" $HOME/cvmfs/sl7/sw/${1}/env_rat-${1}.sh
sed -i "s|${OLD_RAT}|${NEW_RAT}|g" $HOME/cvmfs/sl7/sw/${1}/env_rat-${1}.csh
sed -i "s|${OLD_LINK}|${NEW_LINK}|g" $HOME/cvmfs/sl7/sw/${1}/env_rat-${1}.csh
sed -i "s|${OLD_RAT}|${NEW_RAT}|g" $HOME/cvmfs/sl7/sw/${1}/rat-${1}/env.sh
sed -i "s|${OLD_LINK}|${NEW_LINK}|g" $HOME/cvmfs/sl7/sw/${1}/rat-${1}/env.sh
sed -i "s|${OLD_RAT}|${NEW_RAT}|g" $HOME/cvmfs/sl7/sw/${1}/rat-${1}/env.csh
sed -i "s|${OLD_LINK}|${NEW_LINK}|g" $HOME/cvmfs/sl7/sw/${1}/rat-${1}/env.csh

# Run the following from $HOME/cvmfs
cd $HOME/cvmfs
chmod -R a+r .
find . -perm /u+x -print0 | xargs -L1 -0 chmod a+x
```
#### upload_cvmfs.sh
This script will simply start a proxy and sync the local RAT copy to the cvmfs RAT copy. This is run from the $HOME directory by `./upload_cvmfs.sh`.
```bash
#!/bin/bash

# Initiate voms
voms-proxy-init --voms snoplus.snolab.ca
# Sync new rat over
rsync -avz -e 'gsissh -p 1975' $HOME/cvmfs/sl7 cvmfs-upload01.gridpp.rl.ac.uk:~/cvmfs_repo
```

### Checking the 

Before allowing anyone else or production clients to use the software, ensure it has been tested properly! Either from your VM, or from a machine with the appropriate OS:
```bash
source /cvmfs/snoplus.egi.eu/sl6/env_cvmfs.sh
source /cvmfs/snoplus.egi.eu/sl6/sw/x.y.z/env_rat-x.y.z.sh
# Run rat as usual
```
Alternatively, log into the CVMFS interactive machine and run rat. The CVMFS interactive machine is an SL6 machine, so you can test the SL6 install but not the SL7 install at that machine.
```bash
gsissh -p 1975 cvmfs-upload01.gridpp.rl.ac.uk
source cvmfs_repo/sl6/sw/x.y.z/
source cvmfs_repo/sl6/sw/x.y.z/env_rat-x.y.z.sh
cd cvmfs_repo/sl6/sw/x.y.z/
# Run rat as usual. (For instance, create and run a small test macro, e.g. tutorial #1, with no output, 3 events).
```
