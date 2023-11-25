# outofband-apidisovery

# Configuring Out-of-Band F5 Distributed Cloud API Discovery using BIG-IP LTM

## Objective
Guide users through the step-by-step process of configuring out-of-band API discovery using F5 Distributed Cloud (XC) and BIG-IP LTM.

## Prerequisites
Ensure the following prerequisites are in place before proceeding:
- Backend API Server (Juiceshop for this demo)
- F5 Distributed Cloud (XC) account
- Test FQDN for F5 XC Load Balancer

## Step-by-Step Configuration

### 1. Configure BIG-IP with Two Virtual Servers

#### a. API Application (SSL Offloading)
- Configure the first virtual server for the API application.
- Implement SSL offloading for enhanced security.

#### b. Internal VS for Encrypted HSL
- Set up the second virtual server for internal communication.
- Configure it to use encrypted HSL for data transmission.

### 2. Attach iRule to API Application VS

- Enhance data collection by attaching an iRule to the API Application VS.
- Configure the iRule to capture HTTP request and response data and forward it to the apilogger container.

### 3. Set Up Apilogger

#### a. Prepare Linux Host and Install Docker
- Set up a Linux host for apilogger.
- Install Docker on the designated Linux host.

#### b. Modify apilogger.js
- Customize the apilogger.js file by setting the FQDN to match the configuration in F5 XC.

#### c. Modify viplist.csv
- Update viplist.csv to synchronize F5 VS (API Application) names with the designated FQDN in F5 XC.

### 4. Configure F5 XC HTTP Load Balancer

#### a. Set Up FQDN
- Define the FQDN for the F5 XC Load Balancer.

#### b. Enable API Protection
- Activate API protection settings as necessary.

#### c. Attach Origin Pool
- Connect the Origin Pool, specifying the API logger Public IP listening on port 3000.

### 5. Send Continuous Test Traffic Using Python Script

- Develop or utilize a Python script to generate sustained test traffic.
- Ensure the traffic volume is substantial for triggering comprehensive API discovery.

## Conclusion
Summarize the configuration steps and confirm the successful establishment of out-of-band F5 Distributed Cloud API Discovery.

## Additional Guidance
Include any extra tips, troubleshooting suggestions, or links to supplementary documentation for further assistance.

## Revision History
Maintain a detailed log of document revisions to track changes over time.




```bash
while true; do ./testbatch.sh "mike1@f5.com" "b6e81427-0a29-4d28-bb2a-70df44b66420.access.udf.f5.com" && sleep 10; done
```
