[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/ciscotee/CiscoSecureEndpoint-Ansible-CheckStatus)
# CiscoSecureEndpoint-Ansible-CheckStatus
The Code use for gathering Cisco Secure Endpoint Linux connector status by [Ansible](https://docs.ansible.com/ansible/latest/index.html)
This will export the output to the local of control machine to verify and validate the status of connector that might cannot connect to Cisco Secure Endpoint portal.

## Requirements

  > [!IMPORTANT]
  >  -  The control machine requires to reach the Linux machines via SSH version 2.
  >  -  Your user should have right to run sudo.
  >  -  The control machine requires to install Ansible.
  >  -  The control machine requires to follow structure of folders of Ansible.
  
  ### For this code, we use following structure of folders.
          ├── ansible.cfg
          ├── group_vars
          │   ├── type1.yml
          │   ├── type2.yml
          │   └── type3.yml
          ├── hosts
          ├── playbooks
          │   ├── cse_check_status.yml
          │   ├── templates
          │   │   ├── cse_status_report.j2
          │   └── vars
          │       └── main.yml
          └── reports
              └── cse_status_report.txt


## Installation
  1. Install Ansible on control machine

          sudo apt install ansible

  2. This code use username and password to authenticate on remote machines then run ansible-vault to create crypted credentials and keep in group_vars. In this code, we give example that you have different credentail in each group so that we   created 3 type.yml

          ansible-vault create group_vars/type1.yml
     It will ask for New Vault password then create your own vault to protect credential. Then add following on it.

         ansible_user: your_username #user to connect Linux machine on type X via SSH
         ansible_password: your_password
     You can create as much as you need if they using different credentails but need to link to hosts.
     
     
  3. Create your hosts file, following is example as mentioned earlier need to link on your vault as we use type1 style then your hosts group by type1 style. Also, you can specific port of your SSH for some machines if not use standard.

         [type1]
         192.168.0.1

         [type2]
         192.168.1.1

         [type3]
         192.168.2.1:2222
     
  4. Create **playbook/vars/main.yml** this file use for variable of your report directory in control machine (local).

         report_directory: "Report Folder"
         
## Usage
  - Run ansible-playbook on your folder project.
  
        ansible-playbook playbooks/cse_check_status.yml --ask-vault-pass
    It will ask for sudo password and your vault to access credentails.
  - After it run successful, you will get output on your report folder with the file-name cse_status_report.txt
  

## Code Steps
  - Check hosts.
  - Connect to hosts and check ampcli avaialble or not.
  - If ampcli available then run next tasks to check status and version of Cisco Secure Endpoints.
  - Create outputs in report folder.

## Notes
You can use debug tasks on the playbook via enable extra variable.

      ansible-playbook playbooks/cse_check_status.yml --ask-vault-pass --extra-vars "debug_enabled=true"
