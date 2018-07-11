# Scapy
#This is simply a repo of some Scapy stuff I'm keeping that has proven useful. 
# These scripts/code are meant for educational purposes ONLY so use them at your own risk and please don't DoS networks you don't own with them. That's just not nice...
#As you can see, this script will take the destination IP as input, and will create connections from different ports. Random custom field values are used for TTL (Time to live) and ID, to obfuscate the identity in case any IDS/IPS (Intrusion Detection System/Intrusion Prevention System) is present at the target side. Every OS has typical TTL values (e.g., Windows 128, Linux 64, etc.), which any firewall or IDS/IPS like Snort can use to detect the attacker’s OS version.

The randshort() function is used to generate random port numbers for the sport (source port) of the TCP packet. The destination port (dport) is set to port 22 (SSH) and 80 (Apache Web server). The TCP connect flag is set to SYN using the flags option.

The srloop function sends p crafted packets at intervals of 0.3 seconds. The results of srloop are collected in ans (for answered packets) and unans (for unanswered packets). The gathered results are displayed in a table format for the reply flags and TTL values.

Finally, the script reports SA (SYN-ACK) responses, and gives the results as answered/unanswered packets.

The target’s reply of SA shows that it “thinks” the ACK from attacker/initiator was lost; hence, it keeps re-sending it, for an interval specified by the kernel. The connection, on the target server, remains in the SYN_RECV condition for 3 minutes for each port, as per the net.ipv4.tcp_synack_retries parameter, which is set to 5 in Linux. After these retries, the kernel closes the connection.

Here is the seed of a SYN flood. Millions of unanswered SYN requests to the target server can fill the buffer up completely, leaving it unable to serve legitimate clients. Now let’s look into custom prevention methods.

ref: http://opensourceforu.com/2011/10/syn-flooding-using-scapy-and-prevention-using-iptables/
