
# NETWORK THREAT DETECTION WITH SURICATA PLUS VPC TRAFFIC MIRRORING
Introduction: Here is a walkthrough guide to complete the Network Threat Detection with Suricata plus VPC Traffic Mirroring project. The objective of this project is to implement a network design that identify and gives an early alert log for any incoming threat attack. Suricata is an open-source IDS/IPS packet inspection tool that is use and collect the incident logs for analysis in json format as described later.
 
Prerequisite:
- Two EC2 instances (Victim and Monitor)
- VPC Traffic Mirroring (Make sure VPC has public-subnet route)
- Suricata (open-source IDS/IPS packet inspection tool)

Step 1: Prepare AWS Environment
- Create a VPC (if you don’t already have one).
- Use the default VPC or create a custom VPC only option with at least one public subnet.

2.	Launch Two EC2 Instances in the same VPC:
- Victim EC2 Instance: In this case Ubuntu is deployed
- Monitor EC2 Instance: Deployed Ubuntu (Suricata is installed on this machine).
 
3.	Assign Security Groups:
- Victim EC2: Allow inbound SSH and other test ports (e.g., 80, 443).
- Monitor EC2: Allow SSH inbound and all inbound traffic from the VPC. (Allow UDP 4738)

4.	Enable VPC Traffic Mirroring:
- Create a Traffic Mirror Target (use the ENI of the Monitor EC2).
- Create a Traffic Mirror Filter (allow all traffic).
- Create a Traffic Mirror Session to mirror Victim EC2’s ENI to the Monitor EC2.
 
Step 2: Install Suricata on Monitor EC2
1.	SSH into the Monitor EC2.
2.	Update the system:
3.	sudo apt-get update && sudo apt-get upgrade
4.	Install and Start Suricata:
- sudo apt-get install suricata
- start and enable Suricata
- sudo systemctl enable suricata
- sudo systemctl start suricata
 
Step 3: Generate Traffic on Victim EC2
1.	SSH into Victim EC2.
2.	Install testing tools:
3.	sudo apt-get install nmap
4.	Simulate malicious activity:
- Perform a port scan:
- nmap -p 1-1000 -sS -T4 10.0.0.18
- while true; do curl http://testmynids.org/uid/index.html; sleep 5; done
 
Step 4: Monitor Traffic with Suricata
1.	On the Monitor EC2, check Suricata logs:
2.	sudo tail -f /var/log/suricata/fast.log
3.	Look for alerts triggered by Nmap scans or suspicious activity.

Step 6: Verify Detection
- Confirm that Suricata detects port scans or abnormal patterns.
- Review alerts in /var/log/suricata/fast.log or /var/log/suricata/eve.json.

Step 7: Cleanup
- Stop traffic mirroring session to avoid charges.
- Terminate or rather tear down both Victim-EC2 and Monitor-EC2 instances when finished.
