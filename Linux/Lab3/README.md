# Lab 3: Create a shell script  
## ping every server in the 172.16.17.x subnet (where x is a number between 0 and 255). If the ping succeeds, display the message “Server 172.16.17.x is up and running” If the ping fails, display the message “Server 172.16.17.x is unreachable”.
## Script
```
#!/bin/bash

# Loop through all possible values of x (0 to 255)
for x in {0..255}
do
  # Ping the current IP with a timeout of 1 second (-c 1 sends 1 packet, -W 1 sets timeout to 1 second)
  if ping -c 1 -W 1 172.16.17.$x > /dev/null 2>&1
  then
    # If ping succeeds
    echo "Server 172.16.17.$x is up and running"
  else
    # If ping fails
    echo "Server 172.16.17.$x is unreachable"
  fi
done

```
## Steps to Use the Script:
### 1- Create the Script File: Save the script to a file :
```
vim ping_servers.sh
```
### 2- Make the Script Executable: Run the following to make the script executable:
```
chmod +x ping_servers.sh
```
### 3- Run the Script: Execute the script:
```
./ping_servers.sh
```
## Explanation of the Script:
- for x in {0..255}: Loops through numbers from 0 to 255.
- ping -c 1 -W 1: Sends one ping request to the target IP with a 1-second timeout.
- &> /dev/null: Suppresses the output of the ping command.
- if condition checks the result of the ping command:
- If successful, it prints "Server ... is up and running."
- If unsuccessful, it prints "Server ... is unreachable."
## Output Example:
```
Server 172.16.17.0 is unreachable
Server 172.16.17.1 is up and running
Server 172.16.17.2 is unreachable
...
```
This script will check all IPs in the 172.16.17.x subnet and display their status.
