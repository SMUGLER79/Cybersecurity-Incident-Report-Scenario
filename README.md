# Cybersecurity-Incident-Report-Scenario

# Introduction

Earlier this afternoon, our monitoring system alerted us to an issue with the company’s web server, which led to connection timeouts when trying to access the website. Upon further investigation, I used a packet sniffer to analyze the traffic and identified a large volume of TCP SYN requests coming from an unfamiliar IP address. This sudden surge in SYN requests was overwhelming the server, causing it to lose its ability to respond and resulting in service disruptions for both employees and users trying to access the site. Based on the traffic pattern, it appears the server was targeted by a SYN flood attack, which I am currently working to address and mitigate.

# Scenario

You work as a security analyst for a travel agency that advertises sales and promotions on the company’s website. The employees of the company regularly access the company’s sales webpage to search for vacation packages their customers might like. 

One afternoon, you receive an automated alert from your monitoring system indicating a problem with the web server. You attempt to visit the company’s website, but you receive a connection timeout error message in your browser.

You use a packet sniffer to capture data packets in transit to and from the web server. You notice a large number of TCP SYN requests coming from an unfamiliar IP address. The web server appears to be overwhelmed by the volume of incoming traffic and is losing its ability to respond to the abnormally large number of SYN requests. You suspect the server is under attack by a malicious actor. 

You take the server offline temporarily so that the machine can recover and return to a normal operating status. You also configure the company’s firewall to block the IP address that was sending the abnormal number of SYN requests. You know that your IP blocking solution won’t last long, as an attacker can spoof other IP addresses to get around this block. You need to alert your manager about this problem quickly and discuss the next steps to stop this attacker and prevent this problem from happening again. You will need to be prepared to tell your boss about the type of attack you discovered and how it was affecting the web server and employees.

# Workflow

1. [Read the Wireshark TCP/HTTP Log.](https://github.com/SMUGLER79/Cybersecurity-Incident-Report-Scenario/blob/main/Wireshark%20TCP_HTTP%20log.xlsx)
2. [Identify the type of attack causing the network interuption.](#Identify-the-type-of-attack-causing-the-network-interuption.)
3. [How the attack is causing the website to malfunction and suggestions](#How-the-attack-is-causing-the-website-to-malfunction-and-suggestions)

# Identify the type of attack causing the network interuption.

**Log Entry number and Timestamp**
![image](https://github.com/user-attachments/assets/aaf84d53-41b4-4aaf-871f-6fd58345af94)
This Wireshark TCP log section provided to you starts at log entry number (No.) 47, which is three seconds and .144521 milliseconds after the logging tool started recording. This indicates that approximately 47 messages were sent and received by the web server in the 3.1 seconds after starting the log. This rapid traffic speed is why the tool tracks time in milliseconds. 

**Source and Destination IP Address**
![image](https://github.com/user-attachments/assets/0aba484b-c30a-4ac8-80fc-c1629f8a0b55)
The source and destination columns contain the source IP address of the machine that is sending a packet and the intended destination IP address of the packet. In this log file, the IP address 192.0.2.1 belongs to the company’s web server. The range of IP addresses in 198.51.100.0/24 belong to the employees’ computers.

**Protocol type and related information**
![image](https://github.com/user-attachments/assets/b718d611-8cf0-4b8b-aeea-3a0e22bc8602)
The Protocol column indicates that the packets are being sent using the TCP protocol, which is at the transport layer of the TCP/IP model. In the given log file, you will notice that the protocol will eventually change to HTTP, at the application layer, once the connection to the web server is successfully established.

The Info column provides information about the packet. It lists the source port followed by an arrow → pointing to the destination port. In this case, port 443 belongs to the web server. Port 443 is normally used for encrypted web traffic.

The next data element given in the Info column is part of the three-way handshake process to establish a connection between two machines. In this case, employees are trying to connect to the company’s web server: 
-The [SYN] packet is the initial request from an employee visitor trying to connect to a web page hosted on the web server. SYN stands for “synchronize.” 
-The [SYN, ACK] packet is the web server’s response to the visitor’s request agreeing to the connection. The server will reserve system resources for the final step of the handshake. SYN, ACK stands for “synchronize acknowledge.”
-The [ACK] packet is the visitor’s machine acknowledging the permission to connect. This is the final step required to make a successful TCP connection. ACK stands for “acknowledge.”


# How the attack is causing the website to malfunction and suggestions
