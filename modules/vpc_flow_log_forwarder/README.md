# AWS Datadog VPC Flow Log Monitoring Terraform module

Terraform module which process a VPC flow log monitoring DATA_MESSAGE, coming from CloudWatch logs.

## Terraform versions

Terraform 0.12 and above are supported.

## Usage

```hcl
# Note: you will need to create this secret manually prior to running
# This avoids having to pass the key to Terraform in plaintext
data "aws_secretsmanager_secret" "datadog_api_key" {
  name = "datadog/api_key"
}

module "datadog_vpc_flow_log_forwarder" {
  source  = "clowdhaus/datadog-forwarders/aws//modules/vpc_flow_log_forwarder"

  kms_alias             = "alias/datadog" # KMS key will need to be created outside of module
  dd_api_key_secret_arn = data.aws_secretsmanager_secret.datadog_api_key.arn

  tags = {
    Terraform   = "true"
    Environment = "dev"
  }
}
```

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.12.26 |
| aws | >= 3.0 |

## Providers

| Name | Version |
|------|---------|
| aws | >= 3.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| create | Controls whether the forwarder resources should be created | `bool` | `true` | no |
| dd\_api\_key\_secret\_arn | The ARN of the Secrets Manager secret storing the Datadog API key, if you already have it stored in Secrets Manager | `string` | `""` | no |
| dd\_app\_key | The Datadog application key associated with the user account that created it, which can be found from the APIs page | `string` | `""` | no |
| dd\_site | Define your Datadog Site to send data to. For the Datadog EU site, set to datadoghq.eu | `string` | `"datadoghq.com"` | no |
| environment\_variables | A map of environment variables for the forwarder lambda function | `map(string)` | `{}` | no |
| forwarder\_version | VPC flow log monitoring version - see https://github.com/DataDog/datadog-serverless-functions/releases | `string` | `"3.27.0"` | no |
| kms\_alias | Alias of KMS key used to encrypt the Datadog API keys - must start with `alias/` | `string` | n/a | yes |
| kms\_key\_arn | KMS key that is used to encrypt environment variables. If this configuration is not provided when environment variables are in use, AWS Lambda uses a default service key | `string` | `null` | no |
| lambda\_tags | A map of tags to apply to the forwarder lambda function | `map(string)` | `{}` | no |
| layers | List of Lambda Layer Version ARNs (maximum of 5) to attach to the forwarder lambda | `list(string)` | `[]` | no |
| log\_retention\_days | Forwarder CloudWatch log group retention in days | `number` | `7` | no |
| memory\_size | Memory size for the forwarder lambda function | `number` | `256` | no |
| name | Forwarder lambda name | `string` | `"datadog-vpc-flow-log-forwarder"` | no |
| policy\_arn | IAM policy arn for forwarder lambda function to utilize | `string` | `""` | no |
| policy\_name | Forwarder policy name | `string` | `""` | no |
| policy\_path | Forwarder policy path | `string` | `null` | no |
| publish | Whether to publish creation/change as a new Lambda Function Version | `bool` | `false` | no |
| read\_cloudwatch\_logs | Whether the forwarder will read CloudWatch log groups for VPC flow logs | `bool` | `false` | no |
| reserved\_concurrent\_executions | The amount of reserved concurrent executions for the forwarder lambda function | `number` | `10` | no |
| role\_arn | IAM role arn for forwarder lambda function to utilize | `string` | `""` | no |
| role\_max\_session\_duration | The maximum session duration (in seconds) that you want to set for the specified role. If you do not specify a value for this setting, the default maximum of one hour is applied. This setting can have a value from 1 hour to 12 hours. | `number` | `null` | no |
| role\_name | Forwarder role name | `string` | `""` | no |
| role\_path | Forwarder role path | `string` | `null` | no |
| role\_permissions\_boundary | The ARN of the policy that is used to set the permissions boundary for the forwarder role. | `string` | `null` | no |
| role\_tags | A map of tags to apply to the forwarder role | `map(string)` | `{}` | no |
| runtime | Lambda function runtime | `string` | `"python2.7"` | no |
| s3\_log\_bucket\_arns | S3 log buckets for forwarder to read and forward VPC flow logs to Datadog | `list(string)` | `[]` | no |
| security\_group\_ids | List of security group ids when Lambda Function should run in the VPC. | `list(string)` | `null` | no |
| subnet\_ids | List of subnet ids when Lambda Function should run in the VPC. Usually private or intra subnets. | `list(string)` | `null` | no |
| tags | A map of tags to use on all resources | `map(string)` | `{}` | no |
| timeout | The amount of time the forwarder lambda has to execute in seconds | `number` | `10` | no |
| use\_policy\_name\_prefix | Whether to use unique name beginning with the specified `policy_name` for the forwarder policy | `bool` | `false` | no |
| use\_role\_name\_prefix | Whether to use unique name beginning with the specified `role_name` for the forwarder role | `bool` | `false` | no |

## Outputs

| Name | Description |
|------|-------------|
| cloudwatch\_log\_group\_arn | The ARN of the forwarder lambda function CloudWatch log group |
| lambda\_arn | The ARN of the forwarder lambda function |
| lambda\_kms\_key\_arn | (Optional) The ARN for the KMS encryption key for the forwarder lambda function |
| lambda\_qualified\_arn | The ARN of the forwarder lambda function (if versioning is enabled via publish = true) |
| lambda\_source\_code\_hash | Base64-encoded representation of raw SHA-256 sum of the zip file, provided either via filename or s3\_\* parameters |
| lambda\_version | Latest published version of the forwarder lambda function |
| role\_arn | The forwarder lambda role arn |
| role\_id | The forwarder lambda role id |
| role\_name | The forwarder lambda role name |
| role\_policy\_arn | The ARN of the forwarder lambda role policy |
| role\_policy\_id | The ID of the forwarder lambda role policy |
| role\_policy\_name | The name of the forwarder lambda role policy |
| role\_unique\_id | The stable and unique string identifying the forwarder lambda role. |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

Apache-2.0 Licensed. See [LICENSE](../../LICENSE).
