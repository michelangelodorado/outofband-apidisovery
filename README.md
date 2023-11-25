# outofband-apidisovery

# Configuring Out-of-Band F5 Distributed Cloud API Discovery using BIG-IP LTM

## Objective
Guide users through the step-by-step process of configuring out-of-band API discovery using F5 Distributed Cloud (XC) and BIG-IP LTM.

![image](https://github.com/michelangelodorado/outofband-apidisovery/assets/102953584/b996fc11-0a54-44b7-a6ce-e4eb95d8e0b2)

## Prerequisites
Ensure the following prerequisites are in place before proceeding:
- Backend API Server (Juiceshop for this demo)
- F5 Distributed Cloud (XC) account
- Test FQDN for F5 XC Load Balancer

## Step-by-Step Configuration

### 1. Configure BIG-IP with Two Virtual Servers

#### a. API Application (SSL Offloading)
- Configure the first virtual server for the API application (In my example, it is named /Common/api-gw)
- Implement SSL offloading, attach http profile if there isn't so we can use the iRule.

#### b. Internal VS for Encrypted HSL
- Set up the second virtual server to covert unencrypted HSL to use TLS (https://techdocs.f5.com/en-us/bigip-15-0-0/external-monitoring-of-big-ip-systems-implementations/setting-up-secure-remote-logging.html)

### 2. Attach iRule to API Application VS

- Attach the iRule **sidebandLogger.tcl** to the API Application VS 
- Configure the iRule to capture HTTP request and response data and forward it to the logger container.

### 3. Set Up Apilogger

#### a. Prepare Linux Host and Install Docker
- Set up a linux host for the logger and install Docker

#### b. Modify apilogger.js
- Customize the apilogger.js file by setting the FQDN to match the configuration in F5 XC.
- Change **webTargetIP** value, in my example it is set to **external-logger.mikelabs.online**

#### c. Modify viplist.csv
- Update viplist.csv to synchronize F5 VS (API Application) names with the designated FQDN in F5 XC.
In the example:
**/Common/api-gw** is the Virtual Server name
**external-logger.mikelabs.online** is the FQDN of the HTTP Load Balancer in F5 XC

### 4. Configure F5 XC HTTP Load Balancer

#### a. Set Up FQDN
- Define the FQDN for the F5 XC Load Balancer.

#### b. Enable API Protection
- Activate API protection settings as necessary.

#### c. Attach Origin Pool
- Connect the Origin Pool, specifying the API logger Public IP listening on port 3000.

### 5. Send Continuous Test Traffic Using Python Script

- On a client test machine, use Python script under logger/testbatch.sh to generate sustained test traffic.
- Below is a bash command to loop the script every 10 seconds to generate traffic volume that is substantial for triggering API discovery.

```bash
while true; do ./testbatch.sh "mike1@f5.com" "<original FQDN of the Application>" && sleep 10; done
```

## Conclusion

## Additional Guidance





