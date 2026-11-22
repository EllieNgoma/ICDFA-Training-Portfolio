# CCSA108 HTTPS Inspection

**Student Name:** Ellie Ng’oma  
**Student ID:** C11/26/CCSA/17066  
**Lab Title:** CCSA108 HTTPS Inspection   
**Instructor:** Udam Akume Gabriel  
**Completion Date:** 22 November 26  

## Introduction

This report examines the configuration and operation of HTTPS Inspection in Check Point SmartConsole. It reviews the requirements for deployment, certificate management, policy configuration, inspection rules, global settings, and the security considerations associated with inspecting encrypted HTTPS traffic.

## Part A: Practical

### Task 1: Plan the deployment

<img width="574" height="502" alt="Fig_01 Gateway version" src="https://github.com/user-attachments/assets/5822f6b8-e8fd-4577-b62e-3ee52f0e1a62" />

Fig_01 Gateway version

###
<img width="577" height="464" alt="Fig_02 https Inspection requirements" src="https://github.com/user-attachments/assets/a3fb710b-230b-44d1-b5d8-b5d9d114ce0e" />

Fig_02 https Inspection requirements

###
**Gateway Version:** R82

**Two HTTPS Inspection Requirements/Considerations:**

i. An outbound CA certificate must be created or imported before HTTPS Inspection can be used.

- Shown in Step 1: "Import/Create outbound CA Certificate for HTTPS".

ii. The outbound CA certificate must be deployed to client devices before enabling HTTPS Inspection.

- Step 2 states: "Deploy the outbound certificate in your organization" and warns that SSL errors will occur if HTTPS Inspection is enabled without deploying the certificate.

### Task 2: Enable HTTPS Inspection

<img width="578" height="503" alt="Fig_03 Outbound CA Certificate" src="https://github.com/user-attachments/assets/44f66869-d41d-48ba-96b7-ca1723467c75" />

Fig_03 Outbound CA Certificate


<img width="605" height="68" alt="Fig_04 Certificate in the test host" src="https://github.com/user-attachments/assets/07114167-2e9c-48d1-b1f6-fa23c7ecf704" />

Fig_04 Certificate in the test host


### Task 3: Create the HTTPS Inspection policy

<img width="529" height="79" alt="Fig_05 1 https Inspection policy" src="https://github.com/user-attachments/assets/1a18dff1-fbda-40fd-a93a-a00daac9d051" />

Fig_05.1 https Inspection policy


<img width="211" height="75" alt="Fig_05 2 https Inspection policy" src="https://github.com/user-attachments/assets/fbd596b6-33d6-41a2-88c5-015994ee1735" />

Fig_05.2 https Inspection policy

### Task 4: Publish, install and verify

Changes were published successfully. An attempt was made to install the Standard Access Control Policy on the gateway, but the installation failed due to TCP connectivity failures between the Management Server and gateways A-GW-01 and A-GW-02 (port 18191).

| Task | From | Observation | Result |
|------|------|-------------|--------|
| 1 | Internal host with the CA installed | HTTPS site loaded successfully. The HTTPS Inspection policy was not installed on the gateway. | Unable to verify whether the connection was inspected. |
| 2 | Internal host with the CA installed | HTTPS site loaded successfully. The HTTPS Inspection policy was not installed on the gateway. | Unable to verify whether the connection was bypassed. |
| 3 | A host or browser without the CA installed | The HTTPS Inspection policy was not active, so a certificate warning caused by inspection could not be tested. | Unable to verify expected certificate warning. |

### Task 5: Review the global settings

<img width="401" height="244" alt="Fig_06 Fail mode" src="https://github.com/user-attachments/assets/9d4e0ec4-6ba7-4e0a-8a3a-79932970b5b7"/>

Fig_06 Fail mode

###
<img width="412" height="217" alt="Fig_07 Categorazation mode" src="https://github.com/user-attachments/assets/9833d17c-f4c6-4901-99d3-abe51a093956" />

Fig_07 Categorazation mode

###
<img width="391" height="219" alt="Fig_08 Server validations" src="https://github.com/user-attachments/assets/cf50f885-8e3f-4c12-8b87-f55273a19042" />

Fig_08 Server validations

The security risk of a Fail-Open fail mode: a Fail-Open mode allows HTTPS traffic to continue without inspection if HTTPS Inspection fails, which could allow malicious encrypted traffic to bypass security controls

## Part B: Theory Questions

### B1: Multiple choice

1. What does Outbound HTTPS Inspection do?

   A. Inspects encrypted traffic from internal clients to external servers

2. What does the gateway need in order to perform Inbound HTTPS Inspection?

   B. The target web server's digital certificate and private key

3. A web server uses Static NAT. Which IP address goes in the Destination column of its HTTPS Inspection rule?

   D. The original (pre-NAT) address

4. What is the recommended way to roll out HTTPS Inspection?

   B. Start with a small subset of traffic and expand gradually

## B2: Short answers

5. The way how Outbound HTTPS Inspection works, from the client's request to the server and two separate HTTPS connections.

    - When a client requests an HTTPS website, the Check Point gateway intercepts the request before it reaches the destination server. The gateway verifies the server certificate, decrypts the encrypted traffic, inspects the contents according to the security policy, and then re-encrypts the traffic before forwarding it to the server.
   - The two separate HTTPS connections are client to gateway and gateway to server.

6. Comparing Outbound and Inbound HTTPS Inspection, to mention the traffic is inspected and the gateway uses to complete the HTTPS negotiation.

   - Outbound HTTPS Inspection inspects HTTPS traffic from internal clients to external websites and uses the gateway's CA certificate to complete the HTTPS negotiation with the client. While Inbound HTTPS Inspection inspects HTTPS traffic coming from external users to internal web servers and uses the web server's SSL certificate and private key to complete the HTTPS negotiation.

### Traffic inspected
- Outbound: it is internal to external traffic.
- Inbound: it is external to internal traffic.

### HTTPS negotiation
- Outbound: gateway CA certificate
- Inbound: Web server certificate and private key.

7. The reason of installing outbound CA certificate on client computers and the reason to of bypassing some trusted sites

   - The outbound CA certificate must be installed on client computers so they trust the certificates generated by the gateway during HTTPS Inspection and do not receive certificate warnings.
   - Some trusted sites should be bypassed because HTTPS Inspection can cause compatibility, privacy, or certificate validation issues with certain websites.

8. Three settings in the HTTPS Inspection Global Settings and say what each one controls.

   i. Fail Mode: it controls what happens if HTTPS Inspection cannot process a connection. In Fail-Open mode, traffic is allowed to pass without inspection.

   ii. Categorization Mode: it controls how HTTPS traffic is handled while the website category is being determined. The Hold mode blocks requests until categorization is complete.

   iii. Server Validations: it controls certificate checks performed on destination servers. In this configuration, revoked server certificates are validated using CRL checks.

9. After you enable HTTPS Inspection, every user sees a certificate warning, and one staff member complains that the company is reading their online banking. Give two actions you would take.

   i. To install the outbound CA certificate on all client computers so browsers trust the gateway's certificates and do not display certificate warnings.

   ii. To add online banking and other sensitive sites to the bypass list to protect user privacy and avoid compatibility or certificate validation issues.

## Conclusion

HTTPS Inspection was configured and reviewed to understand how encrypted traffic is managed and inspected by the gateway. The activities demonstrated the role of CA certificates, inspection policies, bypass rules, and global settings in securing HTTPS communications while balancing security, privacy, and operational requirements.

## 
