---
title: "AWS EC2 Serial Console"
weight: 5
---

{{% notice note %}}
#### EC2 Instance Connect
In many of these workshop scenarios, you'll need to connect to EC2 instances via Instance Connect.  

From the AWS console, follow these directions below to connect to the specific instance for the given task instruction.
{{% /notice %}}

  - In the **EC2 Console** go to the **Instances page** then find and select the **TASK_SPECIFIC_INSTANCE**.
  - Click **Connect > EC2 serial console**.
    - **Copy the instance ID** as this will be the username and **click Connect**. 

    {{% expand title="**Expand for Screenshots**" %}}
{{% notice note %}}
The instances in the screenshot are just an example. Reference the workshop guides instructions on which instance to select
{{% /notice %}}
	
![](image-ec2conn-1.png)
&nbsp;
![](image-ec2conn-2.png)
&nbsp;
![](image-ec2conn-3.png)
    {{% /expand %}}

  - Login to the EC2 instance:
    - You may need to hit **ENTER** to get a login prompt
        - username: <<copied Instance ID from above>>
        - Password: **`FORTInet123!`** 

    {{% expand title="**Expand for Screenshot**" %}}
![](image-ec2conn-4.png)
    {{% /expand %}}
