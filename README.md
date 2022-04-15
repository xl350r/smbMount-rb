# smbmounter
Requirements:
OS: OSX 10.10 +
Ruby: 1.9.3+ 

## Overview
smbmounter is an OSX script written to make mounting SMB (Windows) shares easier and repeatable. With proper configuration a user will only need to double click on smbmounter for the drives to be mounted. No installation of other tools is required for the script to work. It uses purely the Standard library of ruby and builtin OSX binaries to perform the task.

## Configuration

1.	Transfer the ìsmbmounterî program and the ìConfig.jsonî file to a directory accessable to the user on the system. 
EX: ```/Users/bob/Desktop/script/```
2.	open the Config.json file in a text editor
3.	The config file has 4 entries on each line.
A.	server : hostname or ip address of server
B.	user : domain user that will be used to authenticate
C.	share : shares that need to be mounted (delimeted between ì[ and ]î with î,î)
D.	path  : paths to mount shares to, in-order (delimeted between ì[ and ]î with î,î) 
4.	Multiple servers can have shares mounted from them but each complete json object must be contained on one line. 
EX: ```{"server": "my_server", "user": "Test_user", "share": ["Company Data", "Employee Data"] , "path": ["/Users/dude/mnt/D", "/Users/dude/mnt/E"]}```
5.	Each Key and value must be in double quotes. Arrays such as share, and path must have their valus contained within square brackets and each sub entry double quoted delimited by commas.
6.	Json objects are started and ended with curly braces ì{ and }î
7.	Keys are seperate from values with colons, and each key value pair is seperate by commas.
8.	Each new line is a new JSON objects. Any entry that spans more than one line will error.
9.	With each line being itís own Object, shares can be mounted from multiple servers.
10.	Config files can be edited with any clear text editor (textedit , nano, vi, vim, etc...)
11.	Shares and Paths are case sensitive.
12.	Paths must be full paths, '~/'' alias does not interpret and relative pathing can be dangerous. Target Paths MUST be EMPTY.
13.	The user mut have read and execute permission (r-x) to be able to run the program. Often users do not have execute permissions ona  script. By opening the terminal and running ëchmod +x /Path/to/smbmounterí will allow the user to execute the program.
14.	Smbmounter and Config.json must be in the same directory. Config.json is a case sensitive name for the program. 

## Example

### Senario
John needs 3 shares mounted from 2 seperate servers. He wants them in a folder on his desktop, Called ëWorkí. The servers are part of active directory and he has permissions to access the shares. Server1 has shares ìHRî and ìManagementî, Server2 has the share ìQuickbooksî. The user says his AD account is jsmith	.
1.	Connect to Johnís machine and open Terminal. Additionally download the Script into a folder he can access.
2.	In the terminal navigate to the directory the script is located and run ëchmod +x smbmounterí.  Allowing the user to now be able to execute the script from finder.
3.	Determinte the username for the user with whoami (in this case john).
4.	Create the Config.json file, and open it (nano is recomended if using terminal). See below for example.	
```
{"server" : "Server1" "user": "jsmith" ["HR", "Management"], "path" : ["/Users/john/Desktop/Work/C","/Users/john/Desktop/Work/D"]}
{"server" : "Server2", "user": "jsmith" share" ["Quickbooks"], "path": ["/Users/john/Desktop/Work/E"]}
```

5.	This file should be save in the same directory as the script.
6.	Run the script by double clicking, it will prompt for a password for EACH server.
7.	Ensure the shares mounted, they should have their names be that of the shares and looks like drives rather than folders in finder.

## Note
As of v1.1 the `~/` is supported. Instead of writing `/Users/user/Desktop/Some_folder_mount` you can now write `~/Desktop/Some_folder_mount` for the path target.
