# How was this VM Built?

## Cloned from basePyDocker

These [directions](https://rasa.com/docs/rasa-x/installation-and-setup/install/docker-compose) were followed.

## First try of install.sh failed

```
kvmuser@rasax-server:~/setup
$ sudo bash ./install.sh

Bla bla...

TASK [geerlingguy.docker : Ensure old versions of Docker are not installed.] ****************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure dependencies are installed.] ******************************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure additionnal dependencies are installed (on Ubuntu < 20.04 and any other systems).] ***
skipping: [localhost]

TASK [geerlingguy.docker : Ensure additionnal dependencies are installed (on Ubuntu >= 20.04).] *********************
ok: [localhost]

TASK [geerlingguy.docker : Add Docker apt key.] *********************************************************************
changed: [localhost]

TASK [geerlingguy.docker : Ensure curl is present (on older systems without SNI).] **********************************
skipping: [localhost]

TASK [geerlingguy.docker : Add Docker apt key (alternative for older systems without SNI).] *************************
skipping: [localhost]

TASK [geerlingguy.docker : Add Docker repository.] ******************************************************************
An exception occurred during task execution. To see the full traceback, use -vvv. The error was: apt_pkg.Error: E:Conflicting values set for option Signed-By regarding source https://download.docker.com/linux/ubuntu/ focal: /usr/share/keyrings/docker-archive-keyring.gpg != , E:The list of sources could not be read.
fatal: [localhost]: FAILED! => {"changed": false, "module_stderr": "Traceback (most recent call last):\n  File \"/root/.ansible/tmp/ansible-tmp-1617785875.6870582-2565-120608749045475/AnsiballZ_apt_repository.py\", line 102, in <module>\n    _ansiballz_main()\n  File \"/root/.ansible/tmp/ansible-tmp-1617785875.6870582-2565-120608749045475/AnsiballZ_apt_repository.py\", line 94, in _ansiballz_main\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\n  File \"/root/.ansible/tmp/ansible-tmp-1617785875.6870582-2565-120608749045475/AnsiballZ_apt_repository.py\", line 40, in invoke_module\n    runpy.run_module(mod_name='ansible.modules.packaging.os.apt_repository', init_globals=None, run_name='__main__', alter_sys=True)\n  File \"/usr/lib/python3.8/runpy.py\", line 206, in run_module\n    return _run_module_code(code, init_globals, run_name, mod_spec)\n  File \"/usr/lib/python3.8/runpy.py\", line 96, in _run_module_code\n    _run_code(code, mod_globals, init_globals,\n  File \"/usr/lib/python3.8/runpy.py\", line 86, in _run_code\n    exec(code, run_globals)\n  File \"/tmp/ansible_apt_repository_payload_hoy43z9t/ansible_apt_repository_payload.zip/ansible/modules/packaging/os/apt_repository.py\", line 564, in <module>\n  File \"/tmp/ansible_apt_repository_payload_hoy43z9t/ansible_apt_repository_payload.zip/ansible/modules/packaging/os/apt_repository.py\", line 547, in main\n  File \"/usr/lib/python3/dist-packages/apt/cache.py\", line 170, in __init__\n    self.open(progress)\n  File \"/usr/lib/python3/dist-packages/apt/cache.py\", line 232, in open\n    self._cache = apt_pkg.Cache(progress)\napt_pkg.Error: E:Conflicting values set for option Signed-By regarding source https://download.docker.com/linux/ubuntu/ focal: /usr/share/keyrings/docker-archive-keyring.gpg != , E:The list of sources could not be read.\n", "module_stdout": "", "msg": "MODULE FAILURE\nSee stdout/stderr for the exact error", "rc": 1}

PLAY RECAP **********************************************************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=1    skipped=4    rescued=0    ignored=0   

kvmuser@rasax-server:~/setup
$ sudo bash ./install.sh 
[sudo] password for kvmuser: 
Installing pip and ansible
E: Conflicting values set for option Signed-By regarding source https://download.docker.com/linux/ubuntu/ focal: /usr/share/keyrings/docker-archive-keyring.gpg != 
E: The list of sources could not be read.

```

## Key conflict removal resolved

After deleting docker.gpg and docker.list, it worked:

```
$ sudo rm /usr/share/keyrings/docker-archive-keyring.gpg
$ sudo rm /etc/apt/sources.list.d/docker.list 

```

## Installation successful!
```
$ sudo bash ./install.sh 
Installing pip and ansible
Hit:1 https://download.docker.com/linux/ubuntu focal InRelease
Hit:2 http://us.archive.ubuntu.com/ubuntu focal InRelease                                               
Get:3 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]                               
Get:4 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:5 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease [101 kB]
Fetched 324 kB in 2s (157 kB/s)   
Reading package lists... Done
Reading package lists... Done
Building dependency tree       
Reading state information... Done
python3 is already the newest version (3.8.2-0ubuntu2).
0 upgraded, 0 newly installed, 0 to remove and 207 not upgraded.
Reading package lists... Done
Building dependency tree       
Reading state information... Done
python3-distutils is already the newest version (3.8.5-1~20.04.1).
python3-distutils set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 207 not upgraded.
Reading package lists... Done
Building dependency tree       
Reading state information... Done
wget is already the newest version (1.20.3-1ubuntu1).
0 upgraded, 0 newly installed, 0 to remove and 207 not upgraded.
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1882k  100 1882k    0     0  1445k      0  0:00:01  0:00:01 --:--:-- 1446k
Collecting pip
  Using cached pip-21.0.1-py3-none-any.whl (1.5 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 21.0.1
    Uninstalling pip-21.0.1:
      Successfully uninstalled pip-21.0.1
Successfully installed pip-21.0.1
Requirement already satisfied: ansible<2.10,>-2.9 in /usr/local/lib/python3.8/dist-packages (2.9.19)
Requirement already satisfied: PyYAML in /usr/lib/python3/dist-packages (from ansible<2.10,>-2.9) (5.3.1)
Requirement already satisfied: cryptography in /usr/lib/python3/dist-packages (from ansible<2.10,>-2.9) (2.8)
Requirement already satisfied: jinja2 in /usr/local/lib/python3.8/dist-packages (from ansible<2.10,>-2.9) (2.11.3)
Requirement already satisfied: MarkupSafe>=0.23 in /usr/local/lib/python3.8/dist-packages (from jinja2->ansible<2.10,>-2.9) (1.1.1)
Installing docker role
[WARNING]: - geerlingguy.docker (3.1.1) is already installed - use --force to change version to unspecified
Docker role for ansible has been installed
Downloading Rasa X playbook
Running playbook

PLAY [Install Rasa X] ***********************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [geerlingguy.docker : include_tasks] ***************************************************************************
skipping: [localhost]

TASK [geerlingguy.docker : include_tasks] ***************************************************************************
included: /root/.ansible/roles/geerlingguy.docker/tasks/setup-Debian.yml for localhost

TASK [geerlingguy.docker : Ensure old versions of Docker are not installed.] ****************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure dependencies are installed.] ******************************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure additionnal dependencies are installed (on Ubuntu < 20.04 and any other systems).] ***
skipping: [localhost]

TASK [geerlingguy.docker : Ensure additionnal dependencies are installed (on Ubuntu >= 20.04).] *********************
ok: [localhost]

TASK [geerlingguy.docker : Add Docker apt key.] *********************************************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure curl is present (on older systems without SNI).] **********************************
skipping: [localhost]

TASK [geerlingguy.docker : Add Docker apt key (alternative for older systems without SNI).] *************************
skipping: [localhost]

TASK [geerlingguy.docker : Add Docker repository.] ******************************************************************
ok: [localhost]

TASK [geerlingguy.docker : Install Docker.] *************************************************************************
ok: [localhost]

TASK [geerlingguy.docker : Ensure Docker is started and enabled at boot.] *******************************************
ok: [localhost]
[WARNING]: flush_handlers task does not support when conditional

TASK [geerlingguy.docker : include_tasks] ***************************************************************************
included: /root/.ansible/roles/geerlingguy.docker/tasks/docker-compose.yml for localhost

TASK [geerlingguy.docker : Check current docker-compose version.] ***************************************************
ok: [localhost]

TASK [geerlingguy.docker : Delete existing docker-compose version if it's different.] *******************************
changed: [localhost]

TASK [geerlingguy.docker : Install Docker Compose (if configured).] *************************************************
changed: [localhost]

TASK [geerlingguy.docker : include_tasks] ***************************************************************************
skipping: [localhost]

TASK [Status of Rasa Enterprise license file] ***********************************************************************
ok: [localhost]

TASK [Check if a license exists] ************************************************************************************
ok: [localhost] => {
    "msg": "There is no Rasa Enterprise license file. We will install Rasa X (Community Edition)."
}

TASK [Load configuration file of the customer (includes e.g. the license)] ******************************************
skipping: [localhost]

TASK [Telemetry status] *********************************************************************************************
ok: [localhost] => {
    "msg": "Telemetry, RASA_TELEMETRY_ENABLED=true"
}

TASK [Create Rasa X root directory] *********************************************************************************
changed: [localhost]

TASK [Create Rasa X log directory] **********************************************************************************
changed: [localhost]

TASK [Create Rasa X scripts directory] ******************************************************************************
changed: [localhost]

TASK [Create the directory to store terms accept file] **************************************************************
changed: [localhost]

TASK [Create the directory for the database persistence] ************************************************************
changed: [localhost]

TASK [Create the directory to store the models] *********************************************************************
changed: [localhost]

TASK [Create the directory to store the rasa global configuration] **************************************************
changed: [localhost]

TASK [Create the directory for the authentication] ******************************************************************
changed: [localhost]

TASK [Create the directory for the credentials] *********************************************************************
changed: [localhost]

TASK [Create the directory to store the certificate files] **********************************************************
changed: [localhost]

TASK [Prompt for Rasa Enterprise terms and conditions] **************************************************************
skipping: [localhost]

TASK [Store user input value] ***************************************************************************************
skipping: [localhost]

TASK [Prompt for Rasa X (Community Edition) terms and conditions] ***************************************************
[Prompt for Rasa X (Community Edition) terms and conditions]
Do you accept the Rasa X terms and conditions? Please read them at https://storage.googleapis.com/rasa-x-releases/rasa_x_ce_license_agreement.pdf. Answer with 'YES' or simply press return to agree.:
ok: [localhost]

TASK [Store user input value] ***************************************************************************************
ok: [localhost]

TASK [Check terms and conditions were accepted] *********************************************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}

TASK [write agree file] *********************************************************************************************
changed: [localhost]

TASK [Adapt permissions of agreement file] **************************************************************************
changed: [localhost]

TASK [stat user credentials] ****************************************************************************************
ok: [localhost]

TASK [backwards compatible credentials] *****************************************************************************
skipping: [localhost]

TASK [Status of Rasa X credentials folder] **************************************************************************
ok: [localhost]

TASK [Status of Rasa X env file] ************************************************************************************
ok: [localhost]

TASK [Ensure we do not overwrite the env if we do not have the credentials] *****************************************
ok: [localhost] => {
    "changed": false,
    "msg": "All assertions passed"
}

TASK [Create the environment for the docker compose runner] *********************************************************
changed: [localhost]

TASK [Update Rasa X version in `.env` file if `.env` file already existed] ******************************************
skipping: [localhost]

TASK [Check if Rasa Open Source telemetry has been configured via `.env`] *******************************************
skipping: [localhost]

TASK [Create Rasa Open Source telemetry config if it wasn't present (Enterprise only)] ******************************
skipping: [localhost]

TASK [Update Rasa version in `.env` file if `.env` file already existed] ********************************************
skipping: [localhost]

TASK [Update RASA X demo version in `.env` file if `.env` file already existed] *************************************
skipping: [localhost]

TASK [Read the the content of the .env file] ************************************************************************
changed: [localhost]

TASK [Status of deployment environments file] ***********************************************************************
ok: [localhost]

TASK [Create deployment environments file if not present] ***********************************************************
changed: [localhost]

TASK [Status of Redis credentials file] *****************************************************************************
ok: [localhost]

TASK [Create Redis credentials if not present] **********************************************************************
changed: [localhost]

TASK [Status of RabbitMQ credentials file] **************************************************************************
ok: [localhost]

TASK [Create RabbitMQ credentials if not present] *******************************************************************
changed: [localhost]

TASK [Status of RASA_X_TOKEN credentials file] **********************************************************************
ok: [localhost]

TASK [Create RASA_X_TOKEN credentials if not present] ***************************************************************
changed: [localhost]

TASK [Status of JWT_SECRET credentials file] ************************************************************************
ok: [localhost]

TASK [Create JWT_SECRET credentials if not present] *****************************************************************
changed: [localhost]

TASK [Status of RASA_TOKEN credentials file] ************************************************************************
ok: [localhost]

TASK [Create RASA_TOKEN if not present] *****************************************************************************
changed: [localhost]

TASK [Status of db password credentials file] ***********************************************************************
ok: [localhost]

TASK [Create DB_PASSWORD if not present] ****************************************************************************
changed: [localhost]

TASK [Status of channel credentials file] ***************************************************************************
ok: [localhost]

TASK [Create the environment for the docker compose runner] *********************************************************
changed: [localhost]

TASK [Status of the Rasa endpoint configuration file] ***************************************************************
ok: [localhost]

TASK [Create the endpoints configuration file for Rasa] *************************************************************
changed: [localhost]

TASK [Retrieve docker compose to run Rasa Enterprise] ***************************************************************
skipping: [localhost]

TASK [Retrieve docker compose to run Rasa X (Community Edition)] ****************************************************
changed: [localhost]

TASK [Retrieve the Rasa X commands file] ****************************************************************************
changed: [localhost]

TASK [Dump gcr license to separate file] ****************************************************************************
skipping: [localhost]

TASK [Log in to google cloud registry (gcr)] ************************************************************************
skipping: [localhost]

PLAY RECAP **********************************************************************************************************
localhost                  : ok=57   changed=27   unreachable=0    failed=0    skipped=17   rescued=0    ignored=0   


```