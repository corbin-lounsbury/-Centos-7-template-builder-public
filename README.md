# Centos-RHEL-image-creation

NOTE: This is a sanitized version of a process used in production. I will try to update this as much as possible with tested and working changes. 

NOTE: This repo is maintained on an internal Git server and synced to Github for sharing and backup. I welcome any feedback and suggestions; if you would like to participate, please fork to your personal repo. 

# Synopsis #

This role will create a new VM, install the OS using a kickstart file available via http, updates and installs required packages, then converts the build VM to a template. This is written to be used in conjunction with vCenter.

# Requirements #

* VMware community package for Ansible (and all dependencies)
* Ansible (Latest)
* A web server from which to host the kickstart file.

# Prestaging considerations #

* Review all the variables in the role defaults and vars files. Make sure they line up with your environment. 
* The VMware user can be tricky to configure. The role as-written accounts for being run an an AWX/Tower environment with the ability to preload vault passwords and present them as extra_vars. You can do it the same way or a multitude of others. Make sure you account for this or else all calls to vCenter will fail. 

# Overall process: #

1. Creates a minimal VM in vCenter.
2. Powers on and specifies kickstart files.
3. Waits until port 22 is available for further configuration.
4. Runs yum updates.
5. Install general packages.
6. Install security packages.
7. Creates svc-config user. Adds to wheel group and allows to run passwordless. 
8. Powers down and converts to template.

Process by role:
1-3: Create
4-7: Provision
8: Finalize

# General todo goals #

* Main CICD pipeline: Deprecates and rebuilds image after merge to main. (Run on a schedule?)
* Dev CICD pipeline: Deploys, tests, and deletes after commits to dev. (Two roles? Testing folder?)
* Figure out how to update kickstart with a push/merge
* Populate handlers in Provision role.
* Add support/test for RHEL 7