---
title: AWS Advanced Threat Intelligence
date: 2024-04-07 12:34:27 -500
categories: [AWS Security]
tags: [detection, aws]
---

## Dynamic WAF Rule Generation
```

def lambda_handler(event, context):
guardduty_findings = event['detail']['findings']
waf = boto3.client('wafv2')

    for finding in guardduty_findings:
        if finding['Type'] == 'UnauthorizedAccess:EC2/SSHBruteForce':
            ip_set = waf.get_ip_set(
                Name='blocked_ips',
                Scope='REGIONAL'
            )
            new_ips = ip_set['IPSet']['Addresses'] + [finding['Resource']['InstanceDetails']['IpAddress']]
            
            waf.update_ip_set(
                Name='blocked_ips',
                Addresses=new_ips,
                LockToken=ip_set['LockToken']
            )
    ```

## Security Hub Automation Framework
```

Resources:
SecurityAutomationRole:
Type: AWS::IAM::Role
Properties:
AssumeRolePolicyDocument:
Version: '2012-10-17'
Statement:
- Effect: Allow
Principal:
Service: [lambda.amazonaws.com]
Action: ['sts:AssumeRole']
ManagedPolicyArns:
- arn:aws:iam::aws:policy/AWSWAFFullAccess

```

---