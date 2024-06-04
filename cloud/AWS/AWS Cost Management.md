Ways to keep AWS costs low:
- Use the AWS instance scheduler CloudFormation template ([https://aws.amazon.com/solutions/implementations/instance-scheduler-on-aws/](https://aws.amazon.com/solutions/implementations/instance-scheduler-on-aws/)) to turn off VMs automatically outside working hours.
- Automate scaling EKS development/test clusters to zero nodes outside working hours, leaving only the cost of the control plane.
- Cloud Custodian ([https://cloudcustodian.io/docs/index.html](https://cloudcustodian.io/docs/index.html)) can be used to control and monitor cloud resources more closely but we haven’t used this tool before.
- You can create billing alerts to email if actual or predicted costs rise above expected thresholds.
- Unnecessary regions can be disabled.
- Be sure to enable two-factor authentication for each human user and restrict access with IAM policies for machine users.