CHAPTER 2.1 : Threat Hunting Lab - RECON
====

The following lab will show how to perform recon by living off the land - using native windows tools and commands to get some situational awareness.

- net commands
- powershell commands
- wmic (Windows Management instrumentation)

> It is impossible to block this kind of activity, since these are valid windows commands. It is however unusual for regular users to perform these commands. 

On your windows machine, open a command prompt and type the following commands. Please note that he actual output will probably be different for your windows machine and user name.

![Screenshot command](./2.1/assets/01-command.png)

```yml
 whoami
```

![Screenshot whoami](./2.1/assets/02-whoami.jpg)

```yml
 whoami /groups
```
![Screenshot whoamigroups](./2.1/assets/02-whoamigroups.jpg)

