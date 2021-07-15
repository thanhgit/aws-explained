# WAF 

## Technical requirements
- Have experience AWS cloudfont, ALB, OSI model 

## WAF
- Its primary functionality is to prevent Webapps from intrusion by common attack patterns
- Against DDOS attacks at layer 3 and 4 of the OSI model

### Components of WAF
- There are 3 main components in how it configured to help protect webapps:
    - [Conditions](#conditions-waf)
    - [Rules](#rules-waf)
    - [Web access control list (WACL)](#wacl-waf)

#### <h4 id="conditions-waf">Conditions</h4>
- Define the first part of configuration and customization helping you to select the components of the web requests you want to monitor
- Six configurable conditions:
    - Cross-site scripting (XSS): stolen cookie info via embedded scripts within web apps
    - Geo match: This provides a method of restricting  requests based on a geographic location. Note that if you want to use this function in WAF you first have to disable any geo restriction
    - IP address: allow you restrict web requests based on a specific source IP address or a range of IP addresses
    - Size constraints: using mathematical operators to filter against a specific size of the request such as the header, URI or body
    - SQL injection attacks
    - String and regex matching: The conditions are based upon strings within the request

#### <h4 id="rules-waf">Rules</h4>
- The rules enable you to package the different conditions together
- There are 2 types of rules available:
    - Regular rule
    - Rate-based rule

#### <h4 id="wacl-waf">Web access control list<h4/>
- Is used to create rule sets and then associated to either CloudFont or ALB
- Define actions of <b>Allow</b>, <b>Block</b> and <b>Count</b>

### Monitoring (enable logging)
- Allow to store detailed information about all requests managed and handled by WACL
- The AWS WAF metrics supported by CloudWatch include:
    - AllowRequests
    - BlockedRequests
    - CountedRequests
    - PassedRequests

## AWS Shield
- Shield is used to protect webapp infrastructure from DDOS attacks
- Protect at layer 7 at OSI model
### Types of DDOS:
    - User datagram reflaction attack: using UDP to spoofed source IP adddress
    - SYN flood
    - DNS query flood: sending numerous DNS query against DNS server
    - HTTP flood/cache-busting attacks

## AWS Firewall manager
- Is a useful tool if you are using AWS Organizations and multiple AWS accounts
- AWS firewall manager has been designed to help you manage, control and implement WAF rules across 
- Before using AWS firewall manager. The account must either join an existing AWS Organization or create a new one

## AWS CloudFont secutiry features
- WACL can be associated to CloudFont distributions to help protect webapps from malicious activity. Common features:
    - Geo-restriction: restrict and permit traffic based on the source IP address
    - Origin access identity: the content via S3 bucket and CloudFont, this ensures that content can ONLY be services via CloudFont
    - HTTPS connectivity
    - Signed URLs and cookies
        - Signed URLs is generated manually or web app, including expire time to access content
        - Signed cookies restrict access based on multiple files, such as all pages available to your subscribed users
        - Both signed URLs and cookies involve a level of encryption using public/private key pairs