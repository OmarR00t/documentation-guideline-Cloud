## introduction:
=================

This is a quick start guideline for how to implement deep security SaaS with NAT
gateway in AWS.

This document is divided into two sections:

-   ### Setup environment:

 This section contains how to prepare your environment in AWS before
    installing deep security agent.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image001.jpg)

Bastion host is acting as a jump server to make connection from remote
authorized device to instances in private subnet (optional).

![](media/8ede43cca3a825262ea555b0e58868eb.jpg)

![](media/f7c519cc91092c03c18f214673681189.jpg)

-    ### Install deep security agent:

 This section contains a guideline for how to install deep security agent in
    cloud on workload security dashboard.

### Setup your enviroment
=========================

1.  #### Virtual private cloud (VPC):

-   Create VPC and assign any private IP address ranges:

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image002.jpg)

subnet sizing for private IPv4 in AWS:

| **IP ranges**                   | **Prefix**          |
|---------------------------------|---------------------|
| 10.0. 0.0 - 10.255. 255.255     | (10/8 prefix)       |
| 172.16. 0.0 - 172.31. 255.255   | (172.16/12 prefix)  |
| 192.168. 0.0 - 192.168. 255.255 | (192.168/16 prefix) |

2.  #### Internet gateway:

-   Internet gateway is like a bridge connection between internet and VPC.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image003.jpg)

After you create an internet gateway, you need to associate it with VPC.

3.  #### Subnets:

-   It needs to create two subnets which are:

    **Public Subnet:**

 This subnet will configure to access to internet through internet gateway,also This subnet will be used for Nat gateway & bastion host.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image004.jpg)

   **Private Subnet:**

 This subnet will be configured to access to internet through NAT Gateway &it will be used for Ec2 instances in private subnet.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image005.jpg)


4.  #### Nat Gateway:

-   The main purpose of Nat gateway is to translate private IP addresses in
    local network into one public IP address when EC2 instances want to connect
    to internet.

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image006.jpg)

5.  #### Route tables:

-   You need to create two route tables which are:

   **Private route table:**

Once you create private route table, the default route will navigate traffic
through local network and it needs to add new route which navigate any traffic
to go through NAT gateway if the traffic is going to internet (you need to
associate this route within private subnet).

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image007.jpg)


   **Public route table:**

Once you create public route table, the default route will navigate traffic to
go through local network and it needs to add new route which navigate any
traffic to go through internet gateway if the traffic is going to internet (you
need to associate this route within public subnet).

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image008.jpg)


6.  #### Security Group:

-   Aws security group let you limit and control inbound & outbound traffic in
    EC2 instances level.

   **Security group for bastion host in public subnet**:

Inbound RDP: Allowing RDP traffic to accept inbound traffic from authorized
external device to bastion host, (**Note:** specify your own public IP to
prevent un authorized connection to bastion host(optional)).

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image009.jpg)


Outbound RDP: Allowing RDP traffic from bastion host to any ec2 instances in
local network(optional).

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image010.jpg)

   **Security group in private subnet:**

Inbound RDP: allowing RDP inbound traffic from bastion host to any instances in
private subnet in local network(optional).

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image011.jpg)


Outbound HTTPS: allowing HTTPS outbound traffic and it is used by various
Deep Security cloud services, you can restrict IP address in this URL in
Deep security as service.
section:[https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html\#Deep2](https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html%23Deep2)

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image012.jpg)

**Note:** If you need to restrict the IP address that allowed in your
environment, default ports, deep security URL, and etc. Kindly check this
URL in section:[https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html\#Deep2](https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html%23Deep2)

### Install deep security agent
===============================

1.  #### Signup/Login Dashboard:

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image013.jpg)

You can sign up or login through this URL: <https://cloudone.trendmicro.com/>

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image014.jpg)

After successfully registering/login your account, you will see cloud one
dashboard click on workload security.

2.  #### Add AWS account:

   Cloud one workload security dashboard will be opened click on computers tab
    then on left side right click on computers then choose add AWS account.

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image015.jpg)

3.  #### Setup type:

   You can add AWS account by using quick & Advanced.

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image016.jpg)

4.  #### Deploy deep security agent on server:

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image017.jpg)

In cloud one workload dashboard, click on support and you will see two ways of
deployment.

   **Manual install Deep security agent:**

If you want to download Agent package installer and install it manually.

Installation guide for manually install deep security
    agent:<https://help.deepsecurity.trendmicro.com/10_2/aws/Get-Started/Install/install-dsa.html>

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image018.jpg)

   **Deployment script:**

   This option will install agent by using script.

   Installation guide for install deep security agent using deployment script:
    <https://help.deepsecurity.trendmicro.com/10_2/aws/Add-Computers/ug-add-dep-scripts.html>

    ![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image019.jpg)

5. #### Check the state of deep security agent after deployment:

   Go to computer and see the status of instance.

![](https://github.com/OmarR00t/documentation-beta-/blob/master/Guides%20NAT%20Gateway_files/image020.jpg)
