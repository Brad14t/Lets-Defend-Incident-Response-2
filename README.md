# Lets Defend Incident Response 2

**Javascript Code Detected in Requested URL**

This incident is a XSS alert. Which is caused by an attacker injecting malicious code/scripts into legitimate web applications.

**Summary**

In this incident, I responded to a possible Cross-Site Scripting (XSS) attack after receiving an alert for "JavaScript code detected in URL." Upon investigation, I confirmed the alert as a true positive, identifying malicious JavaScript embedded in the request URL that indicated an attempt to exploit a server vulnerability. Fortunately, the attack was unsuccessful, as the server correctly triggered the alert and did not execute the malicious code. I contained and isolated the affected server to prevent further access, identified the malicious IP address and domain, and emphasized the importance of input validation, output encoding, and monitoring to mitigate similar threats in the future.

**Incident Response:**

* First step is to select `">>"` to create a case and accept responsibility

<img width="699" alt="1" src="https://github.com/user-attachments/assets/85ef009b-4251-409e-b2fa-7dce0c44ffbb">

* Below is the information given to start this investigation.

<img width="699" alt="Screenshot 2024-11-18 151923" src="https://github.com/user-attachments/assets/6fd08ce0-7310-45a1-9b57-0fcdb772ba89">

In `Case Managment` tab I can start my playbook. This asks questions to confirm you are investigating the incident with the correct format.

<img width="690" alt="Screenshot 2024-11-18 152100" src="https://github.com/user-attachments/assets/7257cdf4-ef6f-4f8f-9d8e-bb7727d56fba">

First question is askign us to understand the alert and why it was triggered.

In the information provided in the SIEM I can see what triggered the alert.

<img width="701" alt="1" src="https://github.com/user-attachments/assets/fe76b484-4a13-40d5-80c6-3747e9192e67">

Says there is a malicious code in the URL. Looking at the request URL:
`https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>`

Using my knowledge from this section, I can see the "script" and "alert" are key indicators of a XSS attempt. 

First is to find the trafic between the source and destination address, to do this in the `Log Managmanent` tab I search for all traffic to do with the source address.
<img width="387" alt="Screenshot 2024-11-19 083201" src="https://github.com/user-attachments/assets/6ceceb7a-b4bb-4162-aeed-32827415c95e">

I can see all traffic between the source and destination below.

<img width="636" alt="Screenshot 2024-11-19 083220" src="https://github.com/user-attachments/assets/86a47987-05ea-46c8-a5f8-4eb634955c37">

Looking at some of the files. I can see some with a request code of 200, meaning they did make successful contact.

<img width="485" alt="Screenshot 2024-11-19 093342" src="https://github.com/user-attachments/assets/b4f7e793-3137-4406-88e5-c75348df72cf">

Looking through the data, the latest file has the matching request URL.

<img width="649" alt="Screenshot 2024-11-19 083433" src="https://github.com/user-attachments/assets/04154e76-cdb5-44ab-9212-93f579cdacb7">

Since I am looking at the raw file I can see the HTTP respose status of 302: 

The HTTP 302 Found redirection response status code indicates that the requested resource has been temporarily moved to the URL in the Location header.

Looks like the attacker was able to make contact with the server and send needed data to a temperary source.

Now to do some more OSINT research of the source IP.

Virus Total:

This shows the IP as malicious and that the address originated from China which wouldnt make sense for this incident.

<img width="947" alt="Screenshot 2024-11-19 084419" src="https://github.com/user-attachments/assets/cb4e730b-0d0c-4d1c-9378-dec7514723d7">

<img width="362" alt="Screenshot 2024-11-19 084457" src="https://github.com/user-attachments/assets/db097898-8d63-42b0-91d3-d7df8f03a106">

Next I am just going to double check and gather more information on the source address using Cisco Talos:

This confirmed it is malicious and it originating from China is also confirmed.

<img width="947" alt="Screenshot 2024-11-19 084702" src="https://github.com/user-attachments/assets/69993e46-6f5a-4a68-a7bd-cf1f1892da29">

OSINT gathered information on source IP:

* IP -	112.85.42.13
* Hostname/ Label - CHINA UNICOM China169 Backbone
* Country of origin - China
* Known malicious range - 112.80.0.0 - 112.87.255.255

Next I want to see the affected server. Checking inside the `Endpoint Security` tab I search for destination address: `172.16.17.17`

I can see the address is the WebServer 1002

<img width="395" alt="1" src="https://github.com/user-attachments/assets/f4ef4e42-e50a-4128-9844-d20e8ec172cb">

Looking through the server's process, network, terminal and browser history, nothing is jumping out to me right away.

Going through the questions given by the case, this will quicly summarize some information about the case.

<img width="532" alt="Screenshot 2024-11-19 085935" src="https://github.com/user-attachments/assets/8c7cebd5-be47-4c70-b044-e3368a9779de">

Ownership of the IP addresses and devices.
* Source IP - 112.85.42.13 - China Unicom
* Destination IP - 172.16.17.17 - WebServer 1002

If the traffic is coming from outside (Internet);
* Yes it's coming from the china IP so the data is from over the internet.

Ownership of IP address (Static or Pool Address? Who owns it? Is it web hosting?)
* Source IP is owned by China Unicom Liaoning Company
* Destination IP is owned by LetsDefend
  
Reputation of IP Address (Search in VirusTotal, AbuseIPDB, Cisco Talos)
* Bad reputation of source IP

If the traffic is coming from company network;
* NO
Hostname of the device
- Source - ChinaUnicom Hostmaster

Continuing on with the questions.

Yes the address 112.85.42.13 is maicious according to all the findings.

<img width="548" alt="Screenshot 2024-11-19 091153" src="https://github.com/user-attachments/assets/909f40e7-fbc3-42b2-b40a-1fd5cfdbdcbb">

Obviously this incident was a XSS attack.

<img width="504" alt="Screenshot 2024-11-19 091257" src="https://github.com/user-attachments/assets/0b249e11-d37f-4c31-a14e-78b1637dea38">

Next one is wondering if its planned. I dont think this was planned, I didnt find anything of intrest in the EDR logs.

<img width="479" alt="Screenshot 2024-11-19 092215" src="https://github.com/user-attachments/assets/cc3e485b-1c73-4122-b86f-806ced23fe51">

Next is the direction, as seen before the traffic is coming from China. So it will be Internet -> Comapny Network

<img width="471" alt="Screenshot 2024-11-19 092233" src="https://github.com/user-attachments/assets/574e8864-603e-4571-a976-588efa0aaaeb">

Was the attack successful? 

No but the attacker did make contact. And it was a correct positive.

<img width="486" alt="Screenshot 2024-11-19 092648" src="https://github.com/user-attachments/assets/cc90cb05-146e-497c-8ad4-384810f474fa">

Next I add my artifacts.

<img width="494" alt="Screenshot 2024-11-19 093114" src="https://github.com/user-attachments/assets/715bd295-ff9b-4aee-b216-549cae904805">

Their is no need to escalte due to the attack being unsuccessful.

<img width="462" alt="Screenshot 2024-11-19 093136" src="https://github.com/user-attachments/assets/d7e83af4-7d33-42dd-b55b-6b78c30f46cd">

Add a quick note.

<img width="485" alt="Screenshot 2024-11-19 093342" src="https://github.com/user-attachments/assets/0ad31302-cb49-40cc-a090-17d847fac5a0">






