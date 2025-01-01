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

  - The [SYN] packet is the initial request from an employee visitor trying to connect to a web page hosted on the web server. SYN stands for “synchronize.” 
  - The [SYN, ACK] packet is the web server’s response to the visitor’s request agreeing to the connection. The server will reserve system resources for the final step of the handshake. SYN, ACK stands for “synchronize acknowledge.”
  - The [ACK] packet is the visitor’s machine acknowledging the permission to connect. This is the final step required to make a successful TCP connection. ACK stands for “acknowledge.”

**The Attack**

Malicious actors can take advantage of the TCP protocol by flooding a server with SYN packet requests for the first part of the handshake. However, if the number of SYN requests is greater than the server resources available to handle the requests, then the server will become overwhelmed and unable to respond to the requests. This is a network level denial of service (DoS) attack, called a SYN flood attack, that targets network bandwidth to slow traffic. A SYN flood attack simulates a TCP connection and floods the server with SYN packets. A DoS direct attack originates from a single source. A distributed denial of service (DDoS) attack comes from multiple sources, often in different locations, making it more difficult to identify the attacker or attackers.


# How the attack is causing the website to malfunction and suggestions

When website visitors try to establish a connection with the web server, a three-way handshake occurs using the TCP protocol. The 3-way handshake is a process used by TCP to establish a reliable connection between a client and a server. It ensures that both parties are ready to communicate and synchronizes the connection between them. The process begins when the client sends a SYN (synchronize) message to the server. The server responds with a SYN-ACK (synchronize-acknowledgment) message, which acknowledges the client's request and also sends its own sequence number. Finally, the client sends an ACK (acknowledgment) message back to the server, confirming the receipt of the server’s sequence number. When a malicious actor sends a large number of SYN packets all at once, it is typically part of a SYN flood attack, a type of Denial of Service (DoS) attack. The goal of this attack is to overwhelm the target server and prevent legitimate users from establishing connections. 

The log indicates that the IP address “203.0.113.0” sent multiple requests which caused the system to crash. The user started sending the requests slowly and gradually increased the flow of requests, which caused the server to crash. 

This can be fixed as follows:
 - Implementing rate limiting to control the number of requests from any single IP address within a given time frame. 
 - Use a WAF to filter out malicious traffic before it reaches the server.
 - Load balancing to distribute incoming traffic across multiple servers to prevent a single server from being overwhelmed.


# Conlusion

In conclusion, the SYN flood attack on our web server has resulted in a significant disruption, as the malicious actor overwhelmed the server with a high volume of SYN requests, causing it to crash. To prevent such attacks in the future, implementing rate limiting, deploying a Web Application Firewall (WAF), and utilizing load balancing are crucial steps to protect the server from being overwhelmed by malicious traffic. These measures will not only enhance our defense against SYN flood attacks but also ensure that legitimate users can access the website without interruptions, maintaining the integrity and availability of our services.
