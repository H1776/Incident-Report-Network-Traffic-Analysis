<h1> Incident Report: Network Traffic Analysis</h1>

<h2>Description</h2>
In this lab, I will analyze DNS and ICMP traffic in transit using data from a network protocol analyzer tool, tcpdump. I will also identify which network protocol was utilized in the assessment of the cybersecurity incident. In the internet layer of the TCP/IP model, the IP formats data packets into IP datagrams. The information provided in the datagram of an IP packet can provide security analysts with insight into suspicious data packets in transit. Knowing how to identify potentially malicious traffic on a network can help cybersecurity analysts assess security risks in a network and reinforce network security.
<br />

<h2>Scenario</h2>


I am a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. 

I am tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, I first attempt to visit the website and then I receive the error “destination port unreachable.” To troubleshoot the issue, I load my network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, my browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. My browser then uses this IP address as the destination IP for sending an HTTPS request to the webserver to display the webpage. The analyzer shows that when I send UDP packets to the DNS server, I receive ICMP packets containing the error message: “udp port 53 unreachable.” <br/> <br/>

<p align="center">
TCPDUMP Log <br/> <br/>
<img src="https://imgur.com/4WQosYZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

<h2>Information found in the tcpdump log </h2>
1. The first two lines of the log file show the initial outgoing request from my computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.
<br />
<br />
2. The third and fourth lines of the log show the response to my UDP packet. In this case, the ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.
<br />
<br />
3. In front of each request and response, I find timestamps that indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. This means the time is 1:24 p.m., 32.192571 seconds.
<br />
<br />
4. After the timestamps, I will find the source and destination IP addresses. In the first line, where the UDP packet travels from my browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than (>) symbol is the source address, which in this example is my computer’s IP address. The IP address to the right of the greater than (>) symbol is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is the client's computer's IP address 192.51.100.15.
<br />
<br />
5. After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source, and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocol of the response message to the browser: "ICMP," which is followed by an ICMP error message.
<br />
<br />
6. The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through to the DNS server because no service was listening on the receiving DNS port.
<br />
<br />
7. The remaining lines in the log indicate that ICMP packets were sent two more times, but the same delivery error was received both times. 

<br />
<br />

<h2>Summary of the problem found in the DNS and ICMP traffic log </h2>

The UDP protocol reveals that DNS requests for the domain "yummyrecipesforme.com" were sent from the client's browser to the DNS server. This is based on the results of the network analysis, which show that the ICMP echo reply returned the error message "udp port 53 unreachable". The port noted in the error message is used for DNS service (port 53). The most likely issue is that the DNS service on the server is not listening on port 53, hence the DNS requests are unable to reach the server. <br /> <br />

<h2>Explanation of my analysis of the data and cause of the incident </h2>

The Incident first happened at 1:24 PM and occurred again at times: 1:26 PM and 1:28 PM. The IT team became aware of the incident when customers reported being unable to access the client company's website, www.yummyrecipesforme.com. The IT department used a network analyzer tool, tcpdump, to capture data packets and analyze the network traffic. They observed that DNS requests were sent from the client's browser to the DNS server, but ICMP error messages were returned, indicating that the DNS server's port 53 was unreachable. <br /> <br />  

Key findings of the IT department's investigation:  <br /> <br />
1. DNS requests for the domain "yummyrecipesforme.com" were being sent from the client's browser to the DNS server.  <br /> <br />
2. ICMP error messages were returned, indicating that the DNS server's port 53 was unreachable.  <br /> <br />
3. The issue was consistent across multiple attempts to access the website.

<h2>Likely cause of the Incident </h2>

The likely cause of the incident is a misconfiguration or failure of the DNS service on the server, resulting in the DNS server not listening on port 53 and therefore being unable to process DNS requests for the domain “yummyrecipesforme.com”. This could be due to various reasons such as a misconfigured firewall blocking traffic on port 53, DNS service not running on the server, or hardware/software failure. Further investigation is required to pinpoint the exact cause of the issue. 
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
