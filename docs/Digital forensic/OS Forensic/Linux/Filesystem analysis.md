# Filesystem analysis
This page aims to give tricks regarding the Linux filesystem such as the interesting files for example.

**WARNING !**

*Conducting investigation directly on the compromised machine will alter the forensic integrity of the evidences and the compromised machine, You must proceed with extreme caution ! Before going further, make sure that you have made a copy of the compromised machine.*

## Manual analysis
In case you need to search specific informations regarding the computer, you can check the following path.

### System and OS informations
The path mentionned in this section will help you to gather informations on the computer. 
#### OS release information

??? faq "OS release information"

    === "Path N°1"
		```
		/etc/os-release
		```




#### User accounts information

??? faq "User accounts information"

    === "Path N°1"
		```
		/etc/passwd
		```


#### User group information

??? faq "User group information"

    === "Path N°1"
		```
		/etc/group
		```


#### Sudoers list

??? faq "Sudoers list"

    === "Path N°1"
		```
		/etc/sudoers
		```

#### Login information

??? faq "Login information"

    === "Path N°1"
		```
		/var/log/wtmp
		```

#### Authentication logs

??? faq "Authentication logs"

    === "Path N°1"
		```
		/var/log/auth.log
		```
    === "Path N°2"
		```
		/var/log/auth.log.2
		```
    === "Path N°3"
		```
		/var/log/auth.log.3
		```

### Persistence mechanism
The path mentionned in this section will help you to to detect any backdoor that might have been installed on a machine.
#### Cron jobs

??? faq "Cron jobs"

    === "Path N°1"
		```
		/etc/crontab
		```
#### Services

??? faq "Services"

    === "Path N°1"
		```
		/etc/init.d
		```


#### Bash shell startup

??? faq "Bash shell startup"

    === "Path N°1"
		```
		/home/<user>/.bashrc
		```
    === "Path N°2"
		```
		/etc/bash.bashrc
		```
    === "Path N°2"
		```
		/etc/profile
		```


### System configuration
The path mentionned in this section will help you to gather informations on the system configuration of the computer
#### Hostname

??? faq "Hostname"

    === "Path N°1"
		```
		/etc/hostname
		```


#### Timezone information

??? faq "Timezone information"

    === "Path N°1"
		```
		/etc/timezone
		```

#### Network interfaces

??? faq "Network interfaces"

    === "Path N°1"
		```
		/etc/network/interfaces
		```

#### Running processes

??? faq "Running processes"

    === "Path N°1"
		```
		?
		```

#### DNS information

??? faq "DNS information"

    === "Path N°1"
		```
		/etc/hosts
		```
    === "Path N°2"
		```
		/etc/resolv.conf
		```




### Evidence of execution
The path mentionned in this path will help you to gather informations on the processes executed by the computer
#### Authentication logs
??? faq "Authentication logs"

    === "Path N°1"
		```
		/var/log/auth.log* 
		```
#### Bash History
??? faq "Bash history"

    === "Path N°1"
		```
		/home/<user>/.bash_history
		```

#### Text editor history
??? faq "Text editor history"

    === "Vim - Path N°1"
		```
		/home/<user>/.viminfo
		```
    === "Nano - Path N°1"
		```
		/home/<user>/.bash_history
		```






### Log files
The path mentionned in this section will help you to gather informations on what happened on a given computer
#### Syslogs

??? faq "Syslogs logs"

    === "Path N°1"
		```
		/var/log/syslog 
		```

#### Authentication logs

??? faq "Authentication logs"

    === "Path N°1"
		```
		/var/log/auth.log* 
		```
	
### Third-party logs




## Resources 

- https://tryhackme.com/room/linuxforensics

