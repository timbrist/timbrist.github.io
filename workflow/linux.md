# The workflow of using Linux


## [placeholder]

### Who Am I ? 
To check what linux distribution we are using. 
For me it is very important, because different distribution has different commandlines.
```bash
cat /etc/os-release
```
The outputs will indicate the 

```bash
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

or you can try this: 
```bash
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.4 LTS
Release:	22.04
Codename:	jammy
```

### Where Am I ? 

Usually, the first time we login to the system, we are at **/home/username** also as **~**

**pwd(print working directory')** this command is super import to know which folder we are.
```bash
$ pwd
/home/yan/Downloads
```

### How much disk do I have ? 
```bash
$ df -h .
ilesystem                              Size  Used Avail Use% Mounted on
10.141.0.14@o2ib:10.141.0.13@o2ib:/pfs   50G   24G   27G  47% /projappl

$ du -sh ~/.cache/
2.9G    /users/shenghen/.cache/
```