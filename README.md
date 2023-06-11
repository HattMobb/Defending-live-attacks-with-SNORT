# Defending-live-attacks-with-SNORT
Utilising Snort's IPS capabilities to defend against attacks

## Defending against a brute force attack


## Disabling a malicious reverse shell

After logging into the machine, running Snort in sniffer mode showed the incoming traffic and packets including the following: 

![Screenshot 2023-06-10 115601](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/cc0edf60-1b27-4266-84c5-5275e7e3f98d)

Here you can see traffic originating from port 4444 from 10.10.144.156 connecting to my machine.  
Later my machine can then be seen connecting back to the same port on the same foreign machine.

![Screenshot 2023-06-10 115611](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/38efda47-dae4-4673-bce3-ecf56fa6dba6)

Port 4444 is commonly associated with Metasploit / Metasploit listeners, so it's pretty safe to assume it is some kind of reverse shell communication. 

With this knowledge it's a simple task to create a rule for Snort to use to block this traffic from this machine to this port, thus thwarting the listener.

![Screenshot 2023-06-10 115625](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/da949531-d620-4a6e-82c2-1732584dafce)
