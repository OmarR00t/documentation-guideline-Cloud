**introduction:**
=================

This is a quick start guideline for how to implement deep security SaaS with NAT
gateway in AWS.

This document is divided into two sections:

-   **Setup environment:**

-   This section contains how to prepare your environment in AWS before
    installing deep security agent.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image001.jpg)

Bastion host is acting as a jump server to make connection from remote
authorized device to instances in private subnet (optional).

![](media/8ede43cca3a825262ea555b0e58868eb.jpg)

![](media/f7c519cc91092c03c18f214673681189.jpg)

-   **Cloud one workload security:**

-   This section contains a guideline for how to install deep security agent in
    cloud on workload security dashboard.

**Setup your enviroment**
=========================

1.  **Virtual private cloud (VPC):**

-   Create VPC and assign any private IP address ranges:

![](media/21fdcc960fd1cc8e7158d9d3e91c73f6.jpg)

subnet sizing for private IPv4 in AWS:

| **IP ranges**                   | **Prefix**          |
|---------------------------------|---------------------|
| 10.0. 0.0 - 10.255. 255.255     | (10/8 prefix)       |
| 172.16. 0.0 - 172.31. 255.255   | (172.16/12 prefix)  |
| 192.168. 0.0 - 192.168. 255.255 | (192.168/16 prefix) |

1.  **Internet gateway:**

-   Internet gateway is like a bridge connection between internet and VPC.

![](media/da9b3213a5da2dbdfbd42792bf6830f7.jpg)

After you create an internet gateway, you need to associate it with VPC.

1.  **Subnets:**

-   It needs to create two subnets which are:

-   **Public Subnet:**

-   This subnet will configure to access to internet through internet gateway.

-   This subnet will be used for Nat gateway & bastion host.

![](media/82352c03ddf00f55c1df78741df37209.jpg)

-   **Private Subnet:**

-   This subnet will be configured to access to internet through NAT Gateway.

![](media/1d1d50017598042a0e67ac24bc6ab08b.jpg)

This subnet will be used for Ec2 instances in private subnet.

1.  **Nat Gateway:**

-   The main purpose of Nat gateway is to translate private IP addresses in
    local network into one public IP address when EC2 instances want to connect
    to internet.

    ![C:\\Users\\omar_Almulhim\\Desktop\\guides\\10.JPG](media/4e48910624dee1587d85db6b52fdd500.jpg)

1.  **Route tables:**

-   You need to create two route tables which are:

-   **Private route table:**

![](media/98c9bf4306ebae5b28c83bebf818513d.jpg)

Once you create private route table, the default route will navigate traffic
through local network and it needs to add new route which navigate any traffic
to go through NAT gateway if the traffic is going to internet (you need to
associate this route within private subnet).

-   **Public route table:**

![](media/6ecfe64fc411d61176e6e8be59f09bec.jpg)

Once you create public route table, the default route will navigate traffic to
go through local network and it needs to add new route which navigate any
traffic to go through internet gateway if the traffic is going to internet (you
need to associate this route within public subnet).

1.  **Security Group:**

-   Aws security group let you limit and control inbound & outbound traffic in
    EC2 instances level.

-   **Security group for bastion host in public subnet**:

![](media/50a2724de4cc02ebfb7408929d321dab.jpg)

Inbound RDP port: Allowing RDP traffic to accept inbound traffic from authorized
external device to bastion host, (**Note:** specify your own public IP to
prevent un authorized connection to bastion host(optional)).

-   Outbound RDP: Allowing RDP traffic from bastion host to any ec2 instances in
    local network(optional).

    ![](media/251f1fa2c74dd8dced3ea474863b8d96.jpg)

-   **Security group in private subnet:**

![](media/3854357bd7f1bfa58f6ac7272b7c90f9.jpg)

Inbound RDP: allowing RDP inbound traffic from bastion host to any instances in
private subnet in local network(optional).

-   Outbound: HTTPS: allowing HTTPS outbound traffic and it is used by various
    Deep Security cloud services, you can restrict IP address in this URL in
    Deep security as service
    section:[https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html\#Deep2](https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html%23Deep2)

    ![](media/20159512946fd5aa6c8dbafeb57afa23.jpg)

-   **Note:** If you need to restrict the IP address that allowed in your
    environment, default ports, deep security URL, and etc. Kindly check this
    URL in section:
    [https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html\#Deep2](https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html%23Deep2)

**Install deep security agent**
===============================

1.  **Signup/Login Dashboard:**

![](media/ce730b0678d6ae21d5674bd0ec905d68.jpg)

You can sign up or login through this URL: <https://cloudone.trendmicro.com/>

![](media/9d3c61e607ca5f671fea6034b9cae6cd.jpg)

After successfully registering/login your account, you will see cloud one
dashboard click on workload security.

1.  **Add AWS account:**

-   Cloud one workload security dashboard will be opened click on computers tab
    then on left side right click on computers then choose add AWS account.

    ![](media/03ebc9225f1dd0ddbff3911eecdae5df.jpg)

1.  **Setup type:**

-   You can add AWS account by using quick & Advanced.

    ![](media/f48164668c9b24d47b76ee4244736507.jpg)

1.  **Deploy deep security agent on server:**

![](media/8d0d56aa6bf78a1f4edb3a09f4c434fa.jpg)

In cloud one workload dashboard, click on support and you will see two ways of
deployment.

-   Manual install Deep security agent:

-   If you want to download Agent package installer and install it manually.

-   Installation guide for manually install deep security
    agent:<https://help.deepsecurity.trendmicro.com/10_2/aws/Get-Started/Install/install-dsa.html>

    ![](media/c4a97d9edbadebb7dfa34de22f3349e2.jpg)

-   Deployment script:

-   This option will install agent by using script.

-   Installation guide for install deep security agent using deployment script:
    <https://help.deepsecurity.trendmicro.com/10_2/aws/Add-Computers/ug-add-dep-scripts.html>

    ![](media/af6b603f813dddf6b9ebac3543254822.jpg)

1.  **Check the state of deep security agent after deployment:**

-   Go to computer and see the status of instance.

![C:\\Users\\omar_Almulhim\\Desktop\\guides\\34.JPG](media/bf46be53910df28843c33bf20088d757.jpg)
