---
title: Securely Transfer Files from/to a Remote Server using SCP
date: 2019-11-27 23:14:00 +05:30
categories:
- blog
tags:
- linux
- ubuntu
- productivity
- beginners
category: blog
---

The [SCP](https://en.wikipedia.org/wiki/Secure_copy) ( Secure Copy Protocol ) is a network protocol, based on the BSD RCP protocol, which supports file transfers between hosts on a network. SCP uses Secure Shell (SSH) for data transfer and uses the same mechanisms for authentication.

Using SCP you can copy file/directory :
- From your local machine to a remote system.
- From a remote system to your local system.
- From one remote system to another remote system from your local system

While transferring data using SCP, files and password is encrypted, so that anyone snooping on the traffic doesn’t get anything sensitive.

Things to keep in mind before the start - 

- The scp command relies on ssh for transferring the data, so it requires an ssh key or password to authenticate on the remote systems.
- To be able to copy file/directory you must have at least read permissions on the source file and write permission on the target system.

### Syntax
The syntax for the **scp** command is:
```
scp [options] username@source_host:directory/filename  /where/to/put
```
In the below examples I Recursively copying entire directories -
### Examples
 
**From remote to local** - 
```
scp -r username@ipaddress:/directory/to/send /local/where/to/put
```

**From local to remote** -
```
scp -r /local/directory/to/send username@ipaddress:/where/to/put
```
**Copying between two remote hosts** -
```
scp -r username@ipaddress1:/file/to/send username@ipaddress2:/where/to/put
```
You can use **scp** with the following options according to your requirements.


### Options

**scp –P port** : Generally 22 as a default port of scp. You can also specify a specific port.
**scp –p** : An estimated time and the connection speed will appear on the screen.
**scp –q** : Disable progress meter and warning.
**scp –r** : Recursively copy entire directories.
**scp –v** :  Print debug information into the screen. It can help you debugging connection, authentication and configuration problems.
**scp -c** : By default SCP using “AES-128” to encrypt files. If you want to change to another cipher to encrypt it, you can use “-c”.


I hope that you now have understood how to make the best use of **scp** command to securely transfer files between the systems.

If you have any suggestions on your mind, please let me know in the mail