# Survey
# Installation 
````
git clone https://github.com/ice-wzl/red-team-tools.git
cd red-team-tools/
pip install requirements.txt
````
# survey.py
## Overview
- Script conducts a hosted based survey on a remote linux system with either a key or a password.  
- Place the commands you want to run in a file or use the one provided.  Ensure commands are seperated with newline
````
cat config.cfg 
unset HISTFILE HISTFILESIZE HISTSIZE PROMPT_COMMAND
id
last
who
ps awwux --forest 
netstat -antpu 
ls -la /var/log
````
## Help
````
python3 survey.py -h
usage: survey.py [-h] [-t TARGET] [-u USERNAME] [-c CONFIG] [-P PORT] (-k KEY | -p PASSWORD)

optional arguments:
  -h, --help            show this help message and exit
  -t TARGET, --target TARGET
  -u USERNAME, --username USERNAME
  -c CONFIG, --config CONFIG
                        path to config file with commands to run on the remote host seperated by a new line.
  -P PORT, --port PORT  the remote port to connect to
  -k KEY, --key KEY
  -p PASSWORD, --password PASSWORD
````
# transport.py
## Overview
- Script allows for file upload and download to remote host with a password (key option coming soon)
- Script has two help menus, the first one to help you connect to the target, view with `python3 transport.py -h`
````
python3 transport.py -h
usage: transport.py [-h] [-t TARGET] [-P PORT] [-u USERNAME] [-p PASSWORD]

optional arguments:
  -h, --help            show this help message and exit
  -t TARGET, --target TARGET
  -P PORT, --port PORT
  -u USERNAME, --username USERNAME
  -p PASSWORD, --password PASSWORD
````
- Second help menu is for when you are connected to target, `help`
````
root@10.0.0.5---> help

                exit     --> exit program
                help     --> print this menu
                upload   --> put local file on remote machine
                download --> download remote file to location machine. 
                             save path is your pwd + remote file name 
                lcd      --> change local directory
                lls      --> list current local directory
                cat      --> view file
                pwd      --> see current working directory 
                clear    --> clear screen
````
## How to upload 
````
root@10.0.0.5---> upload
local file to put up: test.txt
remote path for file: /tmp/.hidden.txt
````
## How to download 
````
root@10.0.0.5:# download                                                                                                          
remote file to grab: /etc/passwd
Downloading: [100.0%] [!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!]  
Download Success /etc/passwd
````
- Verify that your download worked 
- Download will rebuilt the target file system on your local machine 
````
[rocky@rocky red-team-tools]$ tree
.
├── 10.0.0.5
│   └── etc
│       └── passwd
````
- `lls`
````
root@10.0.0.5---> lls .
total 40
drwxrwxr-x. 3 rocky rocky  159 Mar 23 16:48 .
drwxrwxr-x. 3 rocky rocky   44 Mar 23 16:40 ..
-rw-rw-r--. 1 rocky rocky 1790 Mar 23 16:48 passwd
````
- The other commands are common linux commands.
# cred-manager.py 
## Overview 
- Script will manage your credentials for all targets on an engagement.  Ive found this helpful in large enterprises where storing all those key accounts is not practical.
- This helps when you have like 20-50 credentials and you want to keep track of the username, password and ip address of where they came from.
- This is not designed to keep track of 10000 user credentials, unless you want to enter those all in by hand (I do not).
## create
- Creates database and table called `TARGETS`
````
localhost--> create
Table Created
````
## view
- See what is currently saved
````
localhost--> view
Data in Table:
(1, '10.10.10.100', 'Administrator', 'Passw0rd')
(2, '10.10.10.101', 'Ryan', 'Password123!@#')
````
## add
````
localhost--> add
Enter unique ID: 1
Enter IP Address: 10.10.10.100
Enter Username: Administrator
Enter Password: Passw0rd
````
## help
````
localhost--> help
{Create | View | Add | Delete | Exit}
````
