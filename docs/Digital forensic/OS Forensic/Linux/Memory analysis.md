# Linux memory analysis
## Volatility 2 and 3
This page aims to give tricks regarding the analysis of Linux/Android memory dump by using Volatility.

### Get a memory dump
### Get a memory profile 
This section will help you to get find the right memory profile to use for your investigations.
### Check memory profile 
cat memory.dump | grep BOOT | tail -n 10
#### Identify the OS and kernel versions used
Before you start doing anything, I suggest you to identify the OS and kernel version  used by the computer at the same moment when the memory was dumped.  

Use one of the following commands to determine the OS and kernel version of your memory dump.

??? faq "Identifying the OS and kernel version"

    === "Using Linux commands"
		 ```
		 cat ...
		 ```
    === "Using Volatility 3"
		 ```
		 cat ...
		 ```

Based on the results, you have two possibilities : 

- Consult the *Resources* section and check the link to see if the memory profile you're searching have been already created
- Create your own memory profile if the memory profile you're searching doesn't exist. 
If you found the right memory profile, you can directly skip to the section called ["Install the memory profile"](#Install the memory profile)

#### Create a virtual machine with the same specs
#### Install the prerequisites
#### Compile the kernel
#### Generate the memory profile
##### Volatility 2
##### Volatility 3



### Install the memory profile
#### Volatility 2
#### Volatility 3
## Resources

??? faq "Precompiled volatility profile"

    === "Volatility 2"
	    Here is a list of precompiled memory profile you can use to find the right memory profile for your investigation.
	    
		|Description|Link|
		|-|-|
		|Volatility2 symbols from Podalirius|[Link](https://github.com/p0dalirius/volatility2-profiles)|
		|Volatlity2 Compilation of volatility2 profiles by volatility foundation|[Link](https://github.com/volatilityfoundation/profiles)|

    === "Volatility 3"
	    Here is a list of precompiled symbols you can use to find the right symbol for your investigation
	    
		|Description|Link|
		|-|-|
		|Volatility3 symbols from Podalirius|[Link](https://github.com/p0dalirius/volatility3-symbols)|
		|Isf server from techanarchy|[Link](https://isf-server.techanarchy.net/)|
		|Volatility 3 symboles tables|[Link](https://volatility3.readthedocs.io/en/latest/symbol-tables.html#mac-linux-symbol-tables)|