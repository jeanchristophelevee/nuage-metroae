# Customizing the Nuage MetroAG Ansible Environment

The *BUILD* phase refers to the modeling part of your environment. It involves setting all variables correctly so that the automated deployment scripts later are actually configuring the right thing. 

It involves:
1. Customizing Variables
1. Unzip NuageNetworks Software
1. Execute `build.yml` or `upgrade_build.yml` to generate necessary Ansible host and group variable files


This phase assumes you have already set up the Nuage MetroAG Ansible environment per `SETUP.md`.

Note that if you have issues during the *BUILD* phase, you can always start over fresh as detailed later in this document.

## Customizing Variables

`build_vars.yml` contains a dictionary of configuration parameters for each component. You determine which components MetroAG operates on, as well as *how* those components are operated on, by including them or excluding them in the `build_vars.yml` file.

If you are using zero factor bootstrapping on VNS, also refer to `ZFB.md` for more information.

Note: Precise syntax is crucial for success.

## Make Unzipped Nuage Software Files Available

Before installing or upgrading with Nuage MetroAG for the first time, ensure that the required unzipped Nuage software files (QCOW2, OVA, and Linux Package files) are available for the components being installed or upgraded. Use one of the two methods below.
* Specify the appropriate source and target directories in `build_vars.yml` as follows and let MetroAG do the heavy lifting for you:  
```
 nuage_zipped_files_dir: "<your_path_with_zipped_software>"    
 nuage_unzipped_files_dir: <your_path_for_unzipped_software  
```
  Then execute the following command:

  `./metro-ansible nuage_unzip.yml`

* Or you can manually copy the proper files to their locations as shown below, as applicable.

  ```
  <nuage_unzipped_files_dir/vsd/qcow2/
  <nuage_unzipped_files_dir/vsd/ova/ (for VMware)
  <nuage_unzipped_files_dir/vsc/
  <nuage_unzipped_files_dir/vrs/el7/
  <nuage_unzipped_files_dir/vrs/u14_04/
  <nuage_unzipped_files_dir/vrs/ul16_04/
  <nuage_unzipped_files_dir/vrs/vmware/
  <nuage_unzipped_files_dir/vrs/hyperv/
  <nuage_unzipped_files_dirh/vstat/
  <nuage_unzipped_files_dir/vns/nsg/
  <nuage_unzipped_files_dir/vns/util/
  ```

## Execute build.yml

After you've set up your variables and made the required software files available, run the playbook with the following command to automatically populate the Ansible variable files:

`./metro-ansible build.yml`

Note: `metro-ansible` is a shell script that executes `ansible-playbook` with the proper includes and command line switches. Use `metro-ansible` (instead of `ansible-playbook`) when running any of the playbooks provided herein.

  When you execute `build.yml`, it takes the variables that you defined in `build_vars.yml` and performs the following tasks for you.
* creates a `host` file populated with the hostnames of all components in the list. (The host file defines the inventory that the playbooks operate on.)
* populates a `host_vars` subdirectory with the variable files for each component in the list. (These variable files contain configuration information specific to each component in the list.)
* populates a `group_vars` directory
* sets additional variables that configure the overall operation of the playbooks
***
## Having Issues? Do You Want to Start Over?
If you have issues with running the build, you can reset to factory settings and start over.

WARNING: **You may lose your work!** A timestamped backup copy, in the form of `build_vars.yml.<date and time>~` is created (in case you change your mind.) Make sure you have enough storage for it.


Reset the build with the following command.


`./metro-ansible reset_build.yml`

Note: `metro-ansible` is a shell script that executes `ansible-playbook` with the proper includes and command line switches. Use `metro-ansible` (instead of `ansible-playbook`) when running any of the playbooks provided herein.

`reset_build.yml` performs the following tasks for you.
* overwrites the contents of `build_vars.yml`
* detroys the `host` file
* destroys the `host_vars` directory
* destroys the `group_vars` directory
* resets the variable configuration of Metro to factory settings

## Ready to Deploy?

Proceed to `DEPLOY.md`.

---
Report bugs you find and suggest new features and enhancements via the [GitHub Issues](https://github.com/nuagenetworks/nuage-metro/issues "nuage-metro issues") feature.

 You may also ask questions and get support on the [nuage-metro-interest@list.nokia.com](mailto:nuage-metro-interest@list.nokia.com "send email to nuage-metro project") mailing list.