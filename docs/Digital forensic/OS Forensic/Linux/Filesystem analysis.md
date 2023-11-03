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

    === "crontab"
		```
		/etc/crontab
		```
    === "cron"
		```
		/var/spool/cron
		```
    === "anacron"
		```
		/var/spool/anacron
		```
#### Services

??? faq "Services"

    === "Path N°1"
		```
		/etc/init.d
		```

#### Drivers

??? faq "Get the list of installed drivers"

    === "Command N°1"
		```
		lsmod
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
    === "Path N°3"
		```
		/etc/profile
		```

??? faq "SystemV"

    === "Path N°1"
		```
		/etc/init.d/*
		```
    === "Path N°2"
		```
		/etc/rc*.d/*
		```
    === "Path N°3"
		```
		/etc/init/*
		```

??? faq "Systemd"

    === "Path N°1"
		```
		/lib/systemd/system/*
		```
    === "Path N°2"
		```
		/etc/systemd/system/*
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
		/home/*/.*_history
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

#### Process information
??? faq "Past process"

    === "Retrieve the executed binary"
		```
		/proc/pid_number/exe
		```

### Log files
The path mentionned in this section will help you to gather informations on what happened on a given computer
#### Syslogs

??? faq "Syslogs logs"

    === "Path N°1"
		```
		/var/log/syslog* 
		```

#### Authentication logs

??? faq "Authentication logs"

    === "Get event connection logs"
		```
		/var/log/auth.log* 
		```
    === "Get all successful connections"
		```
		/var/log/wtmp
		```
    === "Get all failed connections"
		```
		/var/log/btmp
		```

#### Daemon logs

??? faq "Authentication logs"

    === "Path N°1"
		```
		/var/log/daemon.log*
		```
### Third-party logs
#### Web server
??? faq "Web server logs"

    === "Apache2"
	    === "Access.log"
			```
			/var/log/apache2
			```
			See the list of user-agent who sent request
			```
			cat /var/log/apache2/access.log | awk '{print $(NF)}' | uniq
			```
			Get the list of path requested (only 200)
			```
			cat /var/log/apache2/access.log | grep 200 | awk '{print $7}' | uniq
			```
			Get the list of path requested by nmap 
			```
			cat /var/log/apache2/access.log | awk '{print $7}' | uniq
			```
    === "Nginx"
		```
		/var/log/auth.log* 
		```
	



## Resources 

- https://tryhackme.com/room/linuxforensics

