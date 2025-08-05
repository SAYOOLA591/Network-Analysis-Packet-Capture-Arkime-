## Network Analysis Packet Capture (ARKIME)

Introduction to Arkime

Arkime is a powerful open-source full-network capture system, ideal for organizations needing to capture and analyze PCAPs data at high speed. Its scalability makes it an excellent choice for growing needs, it can handles multiple PCAPs by ingesting, indexing, and enabling seamless search across them using Elasticsearch database, the technology that powers Arkime.

##

## Architecture

**Capture Nodes**: Arkime uses capture nodes that collect and store network packets. These nodes can handle high volumes of traffic, making Arkime suitable for large organizations.

**Elasticsearch**: Arkime integrates with Elasticsearch to index packet data, making it easy to search through terabytes of PCAP files.

**Web Interface**: Highlight Arkime's web interface, which allows for easy packet analysis, visualization, and the application of search filters.

##

## Key Features

**Full Packet Capture**: Arkime can capture and store full network packets, providing deep insights into network traffic, unlike some tools that only capture metadata.

**Search and Query**: Arkime's most powerful features is its ability to quickly search and query massive amounts of data. You can search using fields like IP addresses, protocols, or even specific string patterns in packets.

**Scalability**: Arkime is highly scalable, with the ability to run on multiple nodes across large networks, indexing millions of packets per second.

**Integration with Threat Intelligence**: Arkime can integrate with external threat intelligence feeds to flag suspicious activity in real-time.

##


### Skills Learned

- Capturing, indexing, and analyzing PCAP files to detect anomalies and threats.
- Installing and managing Linux-based services like Elasticsearch and Arkime.
- Configuring Arkime with Elasticsearch, MaxMind, and cont3xt for enriched threat intelligence.
- Investigating Indicators of Compromise (IoCs) and proactively responding to threats.
- Using Linux commands for service management, configuration, and automation.
- Creating a safe sandbox environment for malware traffic analysis.
- Using the Arkime GUI for traffic insights and actionable intelligence.

##

### Technologies

- Arkime
- Elasticsearch
- GeoIP databases for enriching network traffic data (MaxMind)
- Malicious sample from Malware Traffic Analysis.
- The contextual intelligence gathering to enhance technical investigations with cont3xt.

##

## Steps

*Ref 1: **Download and Install Arkime***

Download the Ubuntu 22.04 amd64 version of Arkime from GitHub.
Install the package, ensuring all dependencies are resolved:

```plaintext
sudo apt-get -f install
sudo dpkg -i <package-name>

The package will be installed under /opt/arkime/bin.
Use double-tab for auto-completion when navigating this directory.
```

<img src="https://github.com/mullarcyber/Arkime-images/blob/0f1be4a84d872dfd72d911047045f45de48843be/1.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/0f1be4a84d872dfd72d911047045f45de48843be/1.1.png" />

---

*Ref 2: **Configure Arkime***

Navigate to the configuration file to define the network interfaces.

Example interfaces:
loopback;
ens33 (semicolon-separated list for multiple interfaces).
In a production environment, there may be many interfaces, but for this lab setup, we’ll use ens33 for monitoring and capturing traffic.

During configuration:

Put in a super secure P@ssw0rd! accept the prompt to download GEO files (requires a MaxMind account).

<img src="https://github.com/mullarcyber/Arkime-images/blob/130504df6d2bfd9d9b499992de86532d743a4336/2.png" />

---

*Ref 3: **Continue with the Additional Configuration Steps***

```plaintext
Start Elasticsearch:

sudo systemctl start elasticsearch.service
Initialize the Database:

sudo /opt/arkime/db/db.pl http://localhost:9200 init

Add an Admin User:
sudo /opt/arkime/bin/arkime_add_user.sh mullar "admin" P@ssw0rd! --admin

Start the Arkime Capture Service (responsible for capturing PCAPs):
sudo systemctl start arkimecapture.service

Start the Arkime Viewer Service (responsible for the Arkime GUI):
sudo systemctl start arkimeviewer.service

Verify Installation
Check the logs to confirm that Arkime is capturing data and the viewer service is running:
sudo cat /opt/arkime/logs/viewer.log
sudo cat /opt/arkime/logs/capture.log
```

<img src="https://github.com/mullarcyber/Arkime-images/blob/212ad0123f8b5be57eea925561dd2974368dfaa3/3.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/212ad0123f8b5be57eea925561dd2974368dfaa3/3.1.png" />

---

*Ref 4: **Access the Arkime GUI***

```plaintext
Web interface
(http://localhost:8005 in my case is 192.168.237.135:8005)
```

This is the landing page, offering a variety of features to explore and understand Arkime's capabilities, including Sessions, SPIView, SPIGraph, Communication, and much more.

<img src="https://github.com/mullarcyber/Arkime-images/blob/e5b63c87c6ea54318f767f854d7888b1f1b912b9/4.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/e5b63c87c6ea54318f767f854d7888b1f1b912b9/4.1.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/e5b63c87c6ea54318f767f854d7888b1f1b912b9/4.2.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/e5b63c87c6ea54318f767f854d7888b1f1b912b9/4.3.png" />

---

*Ref 5: **Set Up MaxMind GEO Account***

Register for a MaxMind account and create a license key.

Integrating MaxMind Geo with Arkime is crucial because it enriches network traffic data with geographical context, enhancing threat analysis and response.

<img src="https://github.com/mullarcyber/Arkime-images/blob/257c3f1d113bf4c55b59d0c354bc29d941b1cb5d/5.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/257c3f1d113bf4c55b59d0c354bc29d941b1cb5d/5.1.png" />

---

*Ref 6: **Install the GeoIP update tool***

```plaintext
Install the GeoIP update tool:
sudo apt install geoipupdate

Edit the GeoIP configuration file:
sudo nano /etc/Geo<TAB>
Enter the MaxMind License Key and Account ID.
Add the GeoLite2-ASN database.
```

<img src="https://github.com/mullarcyber/Arkime-images/blob/a0b7ad60e7ca152904262c4c5a3e02cc5b371e13/6.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/a0b7ad60e7ca152904262c4c5a3e02cc5b371e13/6.1.png" />

---

*Ref 7: **Run the GeoIP update tool***

```plaintext

sudo geoipupdate

Restart the Arkime capture service to apply the updates:
sudo systemctl restart arkimecapture.service

Check the status to ensure the service is running:
sudo systemctl status arkimecapture.service
```

<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/7.png" />

---

*Ref 8: **Ingest Malicious PCAP into Arkime***

Download PCAP files from Malware Traffic Analysis.
Place the PCAP files in your working directory and ingest them into Arkime:

```plaintext
sudo /opt/arkime/bin/capture --copy -m -R
```

Refer to ref4 to identify the differences to when malicious PCAP file is ingested.

<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/8.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/malicious%20pcap%20ingested.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/8.1.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/8.2.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/20f294103a5cd8dc284aea0bd668b548c0207e51/8.3.png" />

---

### Arkime cont3xt Integration Overview

Cont3xt is a powerful add-on for Arkime that enhances investigations by providing contextual intelligence. It allows analysts to gather additional details about network traffic such as IPs, domains, URLs, emails, phone numbers, or hashes, making it easier to connect the dots during threat investigations,by enabling cont3xt, you enhance arkime’s capabilities, making it a more effective tool for proactive threat detection and response.

```plaintext
Configure cont3xt
sudo /opt/arkime/bin/Configure --cont3xt
Edit the configuration file:

sudo nano /opt/arkime/etc/cont3xt.ini
Provide the Elasticsearch location: http://localhost:9200
Add Password.
Uncomment the line: port=3218
(This is the port on which cont3xt will listen.)
Restart the cont3xt service:

systemctl restart arkimecont3xt.service
```

<img src="https://github.com/mullarcyber/Arkime-images/blob/60eae1074dad8a9cb3015d8972fa09a25c6a7b41/cont3xt1.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/60eae1074dad8a9cb3015d8972fa09a25c6a7b41/cont3xt2.png" />

---

### Enrichment of IOCs

In the settings, navigate to the integrations section, where you’ll find numerous vendors available to enrich the IoCs. For this lab, I registered accounts with AbuseIPDB and AlienVault OTX. I obtained a license key from AlienVault and also created a license key under the API section with AbuseIPDB, which I will demonstrate.

<img src="https://github.com/mullarcyber/Arkime-images/blob/36e28a5cdc862881e7c6d63fe39ffa63d0eea830/iocs%20integration.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/36e28a5cdc862881e7c6d63fe39ffa63d0eea830/AbuseIPDB.png" />
<img src="https://github.com/mullarcyber/Arkime-images/blob/36e28a5cdc862881e7c6d63fe39ffa63d0eea830/AbuseIPDB.png" />

---

### Project Conclusion
Arkime is an incredibly powerful tool that is highly recommended for organizations seeking to capture and monitor their network traffic in real time. Its ability to index, analyze, and visualize packet data makes it an essential asset for enhancing network visibility and security. With features like seamless integration with Elasticsearch, support for threat intelligence enrichment, and tools like cont3xt for contextual analysis, Arkime empowers security teams to detect and respond to threats more efficiently.

This project demonstrates how Arkime provides a robust platform for investigating suspicious activities, performing proactive threat hunting, and enriching security data with additional context. Its scalability and flexibility make it suitable for both lab environments and production networks, allowing organizations to gain actionable insights and improve their overall security posture.

By adopting Arkime, organizations can enhance their incident response capabilities, improve forensic investigations, and maintain better control over their network traffic. This project has showcased the value of Arkime as a critical tool for modern cybersecurity operations.



















