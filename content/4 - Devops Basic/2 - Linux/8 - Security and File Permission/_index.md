---
title: "Security and File Permission    "
date: "`r Sys.Date()`"
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

- [Access Control](#access-control)
- [Managing Users](#managing-users)


## Access Control
- Before diving into access control, we need to understand the concept of account in Linux.
- Every user in linux has an account, it may associate with username and password to login to the system, user account is associate with and unique identifier called `UID`.
- The information about user account is stored in `/etc/passwd` file.
![User Account](images/_index.png)
- The group is to group users into common in role and permission.
- The information about group is stored in `/etc/group` file. Each group has a unique identifier called `GID`.
![Group Account](images/_index-1.png)
- `The super user` is a user with UID 0, it has all the permission to the system named `root`.
- `The system accounts` are user with UID < 100 OR between 500 - 1000 created while installing the system. They don't have a home directory.
- `The service accounts` are similar to system account, but they created while service is installed.
- `The normal account` is user with UID > 1000, they are created by system administrator to run the system service.
- `id`: to check the user information.
![User Information](images/_index-2.png)
- `who`: to check the current user information.
![Current User Information](images/_index-3.png)
- `last`: to check the last login information.
![Last Login Information](images/_index-4.png)
-`su -`: swich to root user.
- `sudo <command>`: to run the command with root permission. The config for sudo is in `/etc/sudoers` file. Only user listed in sudoers file can use sudo.
![Sudoer](images/_index-5.png)
- Let's discuss about the sudoers file
| Field | Description                  | Example                    |
| ----- | ---------------------------- | -------------------------- |
| 1     | User or group                | bob,%sudo(group)           |
| 2     | Host                         | localhost, ALL(default)    |
| 3     | User                         | ALL (default)              |
| 4     | Command user or group can do | /bin/ls, ALL(unrestricted) |

- To prevent user from login to the system as root user, we can disable it by edit the `/etc/passwd` file.
```shell
/root:x:0:0:root:/root/usrc/sbin/nologin
```
- Most of access control files are stored in `/etc/` directory. This directory is read by any people by default, but only root can write to it. These access control files are designed in a way that we should not edit them using text editor, instead we should use built-in command to edit them.
- The password file is stored in `/etc/shadow` file, passwords are hashed 
**Let's see the structure of the /etc/passwd file**
```
USERNANE:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL
```
- `USERNANE`: the username of the user.
- `PASSWORD`: the hashed password of the user.
- `UID`: the unique identifier of the user.
- `GID`: the unique identifier of the group.
- `GECOS`: CSV format of the user information such as full name, office phone number, home phone number, etc.
- `HOMEDIR`: the home directory of the user.
- `SHELL`: the shell of the user.

**Let's dissect the /etc/shadow file**
```
USERNAME:PASSWORD:LASTCHANGE:MIN_AGE,MAX_AGE:WARN:INACTIVE:EXPDATE
```
- `USERNAME`: the username of the user.
- `PASSWORD`: the hashed password of the user. Empty mean the password is not set.
- `LASTCHANGE`: the last time the password was changed.
- `MIN_AGE`: the minimum age the user can change the password.
- `MAX_AGE`: the maximum age the user have to change the password.
- `WARN`: the number of days before the password expires that the user will be warned.
- `INACTIVE`: the number of days after the password has expired that the account still can be used before it is locked. Empty mean it is not enforced.
- `EXPDATE`: The date when the password expires. Empty mean it is not enforced.

**Let's dissect the /etc/group file**
```
NAME:PASSWORD:GID:MEMBERS
```
- `NAME`: the name of the group.
- `PASSWORD`: the hashed password of the group. Empty mean the password is not set.
- `GID`: the unique identifier of the group.
- `MEMBERS`: the list of users in the group.

## Managing Users
- `sudo useradd USER_NAME`: to add a new user.
- `sudo passwd USER_NAME`: to change the password of the user.
- `whoami`: to check the current user.
- `passwd`: to change the password of the current user.
- `useradd -u UID -g GROUP_ID -d HOME_DIR -s DEFAULT_SHELL -c COMMENT USER_NAME`: to add a new user with specific UID, GROUP_ID, HOME_DIR, DEFAULT_SHELL, COMMENT.
![Adding user with full option](images/_index-6.png)
- `id USER_NAME`: to check the user information.
![Check user information](images/_index-7.png)
- `sudo userdel USER_NAME`: to delete the user.
- `sudo groupadd -g GID GROUP_NAME`: to add a new group with specific GID.
- `sudo groupdel GROUP_NAME`: to delete the group.
- `sudo groupmod -g GID GROUP_NAME`: to modify the group.
- `sudo usermod -aG GROUP_NAME USER_NAME`: to add the user to the group.
