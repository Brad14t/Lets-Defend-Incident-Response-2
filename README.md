# Lets Defend Incident Response 2

**Javascript Code Detected in Requested URL**

This incident is a XSS alert. Which is caused by an attacker injecting malicious code/scripts into legitimate web applications.

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

Says there is a malicious code in the URL. Looking at the request URL: `https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>`

Using my knowledge from this section, I can see the "script" and "alert" are key indicators of a XSS attempt. 























