# Honeypot---Cybersecurity
Prerequisites
To start, will need:
- Kali Linux installed and updated
- Kali Purple installed for attack simulation
" Before we beginningwe need to change to the root user "

STEPS INVOLVED :
    1. Update the kali system
    2. Switching to root user
    3. Clone and install 'Cowrie' from github -- https://github.com/cowrie/cowrie.git
    4. Installing a Python3 virtual environment and creating within honeypot directory
    5. Activating the environment and installing the required dependencies
    6. Configuring the cowrie -- etc/cowrie.cfg.dist etc/cowrie.cfg and changing the network traffic from ports 22 to 2222(default port of cowrie) and 23 to 2223 (for telnet)
    7. Changing 'hostname' , 'ssh' and 'telnet' settings
    8. Assign ownership and permissions to normal user -- /var/lib/cowrie , /var/run
    9. Switch to normal user and start cowrie
    10. Check active cowrie processes and verify logs -- var/log/cowrie/cowrie.log
    11. Open another instance (kali purple) for setting attacks
    12. Establish communication between both instances
    13. Attempt to ssh the cowrie instance with the kali purple , Perform bruteforce attacks using 'Hydra'
    14. Verify logs in cowrie -- var/log/cowrie/cowrie.log
    15. Stop the cowrie.
 
COMMANDS USED :
sudo su
sudo apt update && sudo apt upgrade -y
git clone https://github.com/cowrie/cowrie.git /home/kali/Honeypot
ls
sudo apt install python3-venv -y
python3 -m venv cowrie-env
source cowrie-env/bin/activate
pip install -r requirements.txt
cp etc/cowrie.cfg.dist etc/cowrie.cfg
ls etc/
nano etc/cowrie.cfg
iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
iptables -t nat -A PREROUTING -p tcp --dport 23 -j REDIRECT --to-port 2223
chown -R kali:kali /home/kali/Honeypot/var/run
chown -R kali:kali /home/kali/Honeypot/var/lib/cowrie
chmod -R 755 /home/kali/Honeypot/var/run
ls -l /home/kali/Honeypot/var/lib/cowrie
ls -l /home/kali/Honeypot/var/run
 
su kalibin/cowrie start
ps aux | grep cowrie
tail -f var/log/cowrie/cowrie.log
ip a
ping <Target_IP>
ssh <username>@<Target_IP>
hydra -l kali -P /usr/share/wordlists/rockyou.txt ssh://<Target_IP> -t 4
bin/cowrie stop

*** REFERENCES --> https://iritt.medium.com/creating-a-simple-honeypot-project-on-kali-linux-a-step-by-step-guide-with-attack-simulation-d2aacf5e35ea ***
