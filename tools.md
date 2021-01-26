# Tools

## Online network tools
https://centralops.net/co/

## Removing every resource associated with an AWS account
```
AWS Daleks tool
https://github.com/faermanj/aws-daleks/tree/master/awsdaleks
```

## Using jq
```
aws route53 list-hosted-zones
---
{ "HostedZones": [ {
"ResourceRecordSetCount": 9, "CallerReference":
"A036EFFA-E0CA-2AA4-813B-46565D601BAB", "Config": {}, "Id":
"/hostedzone/Z1Q7O2Q6MTR3M8", "Name": "epitech.nl." }, {
"ResourceRecordSetCount": 4, "CallerReference":
"7456C4D0-DC03-28FE-8C4D-F85FA9E28A91", "Config": {}, "Id":
"/hostedzone/ZAY3AQSDINMTR", "Name": "awssystemadministration.com." } ]
}
```
```
aws route53 list-hosted-zones | jq '.HostedZones[].Name'
---
"epitech.nl."
"awssystemadministration.com."
```

```
aws route53 list-hosted-zones | jq \
'.HostedZones[] | select(.Name=="epitech.nl.").ResourceRecordSetCount'
---
9
```
