# Defending-live-attacks-with-SNORT

Utilising Snort's IDS/IPS capabilities to defend against attacks.

## Defending against a brute force attack

Running Snort in detailed/dump mode reveals current network traffic, interestingly a  machine repeatedly trying to access local port 22 - an SSH brute force attempt.


![image](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/b5b81445-a4be-42fc-a561-5997e397079d)

Editing the /snort/local.rules file allows to use this file to provide rules by which Snort will allow/disallow traffic.
Adding the following rule to the file prevents the malicious IP from brute forcing port 22:

![image](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/64dadc25-71f0-4972-ae6c-8a82c27ac89f)


This rule works as long as the IP and attack remains the same. Should the attacker change their IP or try a different attack, another rule must be introduced to block their attempts.


Running the following invokes the rule and halts the malicious traffic:
`sudo snort -c /etc/snort/snort.conf -q -Q --daq afpacket -i eth0:eth1 -A full`

Breaking down the command:

`-q`: Enables quiet mode, which reduces the amount of output produced In quiet mode, Snort only prints alerts and error messages. 

`-Q`: : Enables packet dump mode, which allows Snort to log the contents of packets that trigger alerts - useful for capturing and analyzing network traffic associated with detected intrusions.

`--daq afpacket`: Selects the Data Acquisition (DAQ) module for capturing network traffic. af_packet is a Linux-specific packet capture method that offers high performance.

`-i eth0:eth1`: Specifies the network interfaces that Snort should monitor.

`-A full`: Sets the alert mode to "full." This mode instructs Snort to generate detailed alerts with additional information such as packet payloads, flowbits, and session information.

Success!


## Disabling a malicious reverse shell

After logging into the machine, running Snort in sniffer mode with detailed packet description `sudo snort -d` showed the incoming traffic and packets including the following: 

![Screenshot 2023-06-10 115601](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/cc0edf60-1b27-4266-84c5-5275e7e3f98d)

Above, you can see traffic originating from port 4444 from 10.10.144.156 connecting to my machine.  
Later my machine can then be seen connecting back to the same port on the same foreign machine.

![Screenshot 2023-06-10 115611](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/38efda47-dae4-4673-bce3-ecf56fa6dba6)

Port 4444 is commonly associated with Metasploit / Metasploit listeners, so it's pretty safe to assume it is some kind of reverse shell communication. 

With this knowledge it's a simple task to create a rule for Snort to use to block this traffic from this machine to this port, thus thwarting the listener.

![Screenshot 2023-06-10 115625](https://github.com/HattMobb/Defending-live-attacks-with-SNORT/assets/134090089/da949531-d620-4a6e-82c2-1732584dafce)


