# Identity and Access Management for VPC Endpoints and VPC Endpoint Services<a name="vpc-endpoints-iam"></a>

Use IAM to manage access to VPC endpoints and VPC endpoints services\.

**Controlling the Use of VPC Endpoints**  
By default, IAM users do not have permission to work with endpoints\. You can create an IAM user policy that grants users the permissions to create, modify, describe, and delete endpoints\. The following is an example\.

```
{
    "Version": "2012-10-17",
    "Statement":[{
    "Effect":"Allow",
    "Action":"ec2:*VpcEndpoint*",
    "Resource":"*"
    }
  ]
}
```

For information about controlling access to services using VPC endpoints, see [Controlling Access to Services with VPC Endpoints](vpc-endpoints-access.md)\.

**Controlling the Creation of VPC Endpoints Based on the Service Owner**  
You can use the `ec2:VpceServiceOwner` condition key to control what VPC Endpoint can be created based on who owns the service\. In the following example, you can only create VPC endpoints when the service owner is "amazon"\. To use this example, substitute the account ID, the service owner, and the Region \(unless you are in the `us-east-1` Region\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:CreateVpcEndpoint",
            "Resource": [
                "arn:aws:ec2:us-east-1:accountId:vpc-endpoint/*"
            ],
            "Condition": {
                "StringEquals": {
                    "ec2:VpceServiceOwner": [
                        "amazon"
                    ]
                }
            }
        }
    ]
}
```

**Controlling the Creation or Modification of VPC Endpoint Services Based on the Private DNS Name**  
You can use the `ec2:VpceServicePrivateDnsName` condition key to control what VPC endpoint service can be modified or created based on the Private DNS name associated with the VPC endpoint service\. In the following example, you can only create or VPC endpoint service when the when the Private DNS name is "example\.com"\. To use this example, substitute the account ID, the Private DNS name, and the Region \(unless you are in the `us-east-1` Region\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:ModifyVpcEndpointServiceConfiguration",
                "ec2:CreateVpcEndpointServiceConfiguration"
            ],
            "Resource": [
                "arn:aws:ec2:us-east-1:accountId:vpc-endpoint-service/*"
            ],
            "Condition": {
                "StringEquals": {
                    "ec2:VpceServicePrivateDnsName": [
                        "example.com"
                    ]
                }
            }
        }
    ]
}
```

**Controlling the Creation of VPC Endpoints Based on the Service Name**  
You can use the `ec2:VpceServiceName` condition key to control what VPC endpoint can be created based on the service name\. In the following example, you can only create or VPC endpoint when the when the service name is "com\.amazonaws\.us\-east\-1\.s3"\. To use this example, substitute the account ID, the service name, and the Region \(unless you are in the `us-east-1` Region\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:CreateVpcEndpoint",
            "Resource": [
                "arn:aws:ec2:us-east-1:accountId:vpc-endpoint/*"
            ],
            "Condition": {
                "StringEquals": {
                    "ec2:VpceServiceName": [
                        "com.amazonaws.us-east-1.s3"
                    ]
                }
            }
        }
    ]
}
```