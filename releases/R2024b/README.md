# MATLAB on Amazon Web Services (Linux VM)

## Step 1. Launch the Template

Click the **Launch Stack** button to deploy a standalone MATLAB&reg; desktop client on AWS&reg;. This opens the CloudFormation Create Stack screen in your web browser.

| Region | Launch Link |
| --------------- | ----------- |
| **us-east-1** | [![alt text](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png "Start a MATLAB instance using the template")](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?templateURL=https://matlab-on-aws-stage.s3.amazonaws.com/413761/aws-matlab-template.json) |


**Note**: Creating a stack on AWS can take a few minutes.

## Step 2. Configure the Cloud Resources
After you click the Launch Stack button above, the “Create stack” page will open in your browser where you can configure the parameters. It is easier to complete the steps if you position these instructions and the AWS console window side by side.

1. Specify a stack name. This will be shown in the AWS CloudFormation console and must be unique within the AWS account.


2. Specify and check the defaults for these resource parameters:

| Parameter label | Description |
| --------------- | ----------- |
| **AWS EC2 Instance type** | AWS instance type to use for MATLAB. See https://aws.amazon.com/ec2/instance-types for a list of instance types. |
| **Instance Name** | Name for the MATLAB virtual machine |
| **Remote access protocol** | Access protocol to connect to this instance |
| **Enable browser access for MATLAB** | Option that enables access to MATLAB on your cloud instance within a browser. Opening MATLAB in a browser opens a separate MATLAB session to your Remote Desktop Protocol (RDP) session or NICE DCV session. |
| **Choose if a public IP address should be attached to the MATLAB EC2 instance** | Choose if a public IP address should be attached to the MATLAB EC2 instance. |
| **Keep public ip the same** | Flag indicating whether you want to keep the same public IP address for the instance |
| **Storage Size (GiB)** | Size in GB of the root volume |
| **Custom IAM Role (Optional)** | Name of a custom IAM Role to associate with this instance. If not specified, a predefined role is used. If specified, features requiring special permissions will be unavailable (NICE DCV, CloudWatch, IAM Policies). |
| **Additional IAM Policies (Optional)** | Semicolon-delimited list of IAM Policy ARNs to add to the predefined role. This option cannot be used with a custom IAM Role. |
| **VPC to deploy this stack to** | ID of an existing VPC in which to deploy this stack |
| **Subnet** | ID of an existing subnet. To access the instance from anywhere, ensure that your subnet auto-assigns public IP addresses and is connected to the internet. |
| **SSH Key Pair** | Name of an existing EC2 KeyPair to allow SSH access to all the instances. See https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html for details on creating these. |
| **ClientIPAddress** | Comma-separated list of IP address ranges that will be allowed to connect to this instance. Each IP CIDR should be formatted as \<ip_address>/\<mask>. The mask determines the number of IP addresses to include. A mask of 32 is a single IP address. Example of allowed values: 10.0.0.1/32 or 10.0.0.0/16,192.34.56.78/32. This calculator can be used to build a specific range: https://www.ipaddressguide.com/cidr. You may need to contact your IT administrator to determine which address is appropriate. |
| **Remote password** | Password for the "ubuntu" user. You also need to enter this as an authentication token to access MATLAB on your cloud instance within a browser. |
| **Confirm remote password** | Confirm Password |
| **License Manager for MATLAB connection string** | Optional License Manager for MATLAB, specified as a string in the form \<port>@\<hostname>. If not specified, use online licensing. If specified, the network license manager (NLM) must be accessible from the specified VPC and subnets. To use the private hostname of the NLM hub instead of the public hostname, specify the security group ID of the NLM hub in the AdditionalSecurityGroup parameter. For more information, see https://github.com/mathworks-ref-arch/license-manager-for-matlab-on-aws. |
| **Configure cloudwatch logging for the MATLAB instance** | Flag indicating whether cloudwatch logging for the MATLAB instance is to be enabled. For private instances, CloudWatch logs may not be updated unless a communication method like NAT Gateway or VPC endpoint is setup. |
| **AutoShutdown** | Choose whether you want to enable autoshutdown for your instance after a certain number of hours |
| **Additional security group to place instances in** | ID of an additional (optional) Security Group for the instances to be placed in. Often the License Manager for MATLAB's Security Group. |
| **Custom AMI ID (Optional)** | ID of a custom Amazon Machine Image (AMI) in the target region (optional). If the build has been customized then the resulting machine image may no longer be compatible with the provided CloudFormation template. Compatability can in some cases be restored by making corresponding modifications to the CloudFormation template. The ID should start with 'ami-'. |
| **Optional user inline command** | Provide an optional inline shell command to run on machine launch. For example, to set an environment variable CLOUD=AWS, use this command excluding the angle brackets: \<echo -e "export CLOUD=AWS" \| tee -a /etc/profile.d/setenvvar.sh && source /etc/profile>. To run an external script, use this command excluding the angle brackets: \<wget -O /tmp/my-script.sh "https://www.example.com/script.sh" && bash /tmp/my-script.sh>. Find the logs at '/var/log/mathworks/startup.log'. |


>**Note**: In the capabilities section, you must acknowledge that AWS Cloudformation might create IAM resources and autoexpand nested templates when creating the stack.

3. Click the **Create Stack** button.  The CloudFormation service will start creating the resources for the stack. <p>After clicking **Create** you will be taken to the *Stack Detail* page for your stack. Wait for the Status to reach **CREATE\_COMPLETE**. This may take up to 10 minutes.</p>

## Step 3. Connect to the Virtual Machine in the Cloud

If you chose RDP, then:
1. Expand the **Outputs** section in the the *Stack Detail* page.
1. Look for the key named `RDPConnection` and copy the corresponding public DNS name listed under value. *For example*: ec2-11-222-33-44.compute-1.amazonaws.com
1. Launch any remote desktop client, paste the public DNS name in the appropriate field, and connect. On the Windows Remote Desktop Client you need to paste the public DNS name in the **Computer** field and click **Connect**.
1. In the login screen that's displayed, use the username `ubuntu` and the password you specified while setting up the stack in [Step 2](#step-2-configure-the-stack).

If you chose NICE DCV, then:
1. Expand the **Outputs** section in the the *Stack Detail* page.
1. Look for the key named `NiceDCVConnection` and click on it
1. In the login screen that's displayed, use the username `ubuntu` and the password you specified while setting up the stack in [Step 2](#step-2-configure-the-stack).

If you chose to enable browser access for MATLAB, then:
1. Expand the **Outputs** section in the *Stack Details* page.
1. Look for the key named `BrowserConnection` and click on it
1. On the login screen, use the password you specified while deploying the stack in [Step 2](#step-2-configure-the-stack) as the 'auth token' to authenticate.
1. Browser access for MATLAB is enabled using `matlab-proxy`, a MathWorks&reg; developed Python&reg; package. For more information on `matlab-proxy`, refer to [matlab-proxy GitHub repository](https://github.com/mathworks/matlab-proxy).

## Step 4. Start MATLAB
Double-click the MATLAB icon on the virtual machine desktop to start MATLAB. The first time you start MATLAB, you need to enter your MathWorks Account credentials to license MATLAB. For other ways to license MATLAB, see [MATLAB Licensing in the Cloud](https://www.mathworks.com/help/install/license/licensing-for-mathworks-products-running-on-the-cloud.html).

>**Note**: It may take up to a minute for MATLAB to start the first time.


# Additional Information

## Private networking configuration
If you require a private networking configuration for the MATLAB EC2 instance, you can choose not to attach a public IP to the MATLAB EC2 instance by setting the `EnablePublicIPAddress` parameter as `No`. Listed below are some important considerations for this configuration:

> [!IMPORTANT]
> - **Client Access**: Ensure that the private IP addresses of the jumpbox or client(s) that will access the MATLAB EC2 instance are specified in the `ClientIPAddress` parameter. 
> - **Online Licensing**: If MATLAB needs to be licensed using online licensing, the MATLAB EC2 instance must be able to reach `*.mathworks.com`. This will require a method to allow outbound access to these domains to be setup in your VPC.
> - **Stack Deployment**: The MATLAB EC2 instance may get stuck in the `CREATE_IN_PROGRESS` state during stack deployment in the absence of a public IP. To avoid this, ensure that your VPC has a [VPC endpoint for the AWS CloudFormation service](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/vpc-interface-endpoints.html#vpc-endpoint-create) or NAT Gateway before deployment. Alternatively, you can remove the `CreationPolicy` attribute from the `MATLABEC2Instance` resource in the template before deployment but this can lead to AWS CloudFormation showing the instance as `CREATE_COMPLETE` before it is fully configured and ready for use.
> - **CloudWatch Logs**: If you want CloudWatch logs to be delivered from the instance, your VPC must have either a NAT Gateway or a [VPC endpoint for CloudWatch service](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html#create-VPC-endpoint-for-CloudWatchLogs).

## Delete Your Cloud Resources

Once you have finished using your stack, it is recommended that you delete all resources to avoid incurring further cost. To delete the stack, do the following:
1. Log in to the AWS Console.
1. Go to the AWS CloudFormation page and select the stack you created.
1. Click the **Actions** button and click **Delete Stack** from the menu that appears.

## Nested Stacks

This CloudFormation template uses nested stacks to reference templates used by multiple reference architectures. For details, see the [MathWorks Infrastructure as Code Building Blocks](https://github.com/mathworks-ref-arch/iac-building-blocks) repository.

## CloudWatch Logs
CloudWatch logs enables you to access logs from all the resources in your stack in a single place. To use CloudWatch logs, launch the stack with the feature "Configure cloudwatch logging for the MATLAB instance" enabled. Once the stack deployment is complete, you can access your logs in the "Outputs" of the stack by clicking the link next to "CloudWatchLogs". Note that if you delete the stack, the CloudWatch log group is also deleted. For more information, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html).

----

Copyright 2018-2025 The MathWorks, Inc.

----
