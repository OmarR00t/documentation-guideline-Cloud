**introduction:**
=================

This is a quick start guideline to how to implement deep security agent SaaS
with Squid proxy in AWS.

This document is divided into two sections:

-   **Setup your environment:**

-   This section contains how to setup your environment and walkthrough in every
    step using AWS.

    ![](media/76b7b84c1f3abcbf697e7b2a4561b152.jpg)

    ![C:\\Users\\omar_Almulhim\\Downloads\\Copy of AWS (2019) horizontal framework squid proxy .jpeg](media/31f7b57e8fa0714d73f8302d6abfc75c.jpg)

-   **Cloud one workload security:**

-   This section contains a guideline for how to install deep security agent in
    cloud on workload security dashboard.

**Setup your enviroment**
=========================

1.  **Virtual private cloud (VPC):**

-   Create VPC and assign any private IP address ranges:

    ![](media/21fdcc960fd1cc8e7158d9d3e91c73f6.jpg)

    Subnet sizing for private IPv4 in AWS:

| **IP ranges**                   | **Prefix**          |
|---------------------------------|---------------------|
| 10.0. 0.0 - 10.255. 255.255     | (10/8 prefix)       |
| 172.16. 0.0 - 172.31. 255.255   | (172.16/12 prefix)  |
| 192.168. 0.0 - 192.168. 255.255 | (192.168/16 prefix) |

1.  **Internet gateway:**

-   Internet gateway is acting as bridge connection between internet and VPC.

![](media/da9b3213a5da2dbdfbd42792bf6830f7.jpg)

After you create internet gateway, you need to associate it with VPC.

1.  **Subnets:**

-   you need to create two subnets which are:

-   **Public Subnet:**

    It will configure to access the internet through the internet gateway if the
    traffic is going to the internet & this subnet will be used for Squid proxy
    instance.

![C:\\Users\\omar_Almulhim\\Desktop\\guides\\screenshots2\\squid proxy\\omar public.JPG](media/82352c03ddf00f55c1df78741df37209.jpg)

-   **Private Subnet:**

![](media/3fa63be6a21ce251ccbbf24ac5615628.jpg)

It will configure to access to internet through Squid proxy if the traffic is
going to the internet.

1.  **Squid proxy:**

-   You need to connect to proxy server ec2-user running on Linux OS by using
    putty:

    Open putty On session section –\<Write (username \@Public IP address) and
    choose on connection type SSH.

    ![](media/357d4ab7ea507c80aab88c3049b79b34.jpg)

    Open Puttygen program then choose load then select key pair file that you
    assign it with this ec2 instance then save private key.

    ![C:\\Users\\omar_Almulhim\\Desktop\\guides\\screenshots2\\squid proxy\\putty generator.JPG](media/62b16ad86207c1c60b9163411da4bc54.jpg)

    On putty interface choose SSH section then Auth browse your private key file
    that you saved from Puttygen then click open to connect ec2-instance.

    ![](media/3afca5dc63d5e165d1ed9961de004b0f.jpg)

    write sudo yum install -y squid –\< to install squid proxy

    ![](media/81bcbe3bccfbd82000ac1a6c68d5b313.jpg)

    sudo service squid start

    sudo chkconfig squid on –\< to start squid proxy

    sudo vim /etc/squid/squid.conf –\< Edit the SQUID Configurations file to
    allow TrendMicro and AWS domains only

    add the following commands in squid.conf:

    **\#\# Restrict TrendMicro Access**

    **acl GOOD dstdomain .trendmicro.com .amazonaws.com**

    **http_access allow GOOD**

    **http_access deny all**

    ![](media/542dcf305ebcf5b25f18573fbacc9c56.jpg)

    After you add these commands Press Esc key and type: wq to save changes to
    a file and exit from vim.

    sudo service squid restart–\< restart Squid proxy.

1.  **Route tables:**

-   You need to create two route tables which are:

-   **Private route table:**

    Once you create private route table, the default route will navigate local
    traffic  
    through local network and it needs to add new route which navigate any
    traffic to go through Squid proxy if the traffic is going to internet (you
    need to associate this route within private subnet).

    ![](media/3fa63be6a21ce251ccbbf24ac5615628.jpg)

-   **Public route table:**

    ![](media/6ecfe64fc411d61176e6e8be59f09bec.jpg)

    Once you create public route table, the default route will navigate local
    traffic to go through local network and it needs to add new route which
    navigate any traffic to go through internet gateway if the traffic is going
    to internet (you need to associate this route within public subnet).

1.  **Security Group:**

-   Aws security group let you limit and control inbound & outbound traffic in
    EC2 instances level.

-   **Security group Squid proxy in public subnet**:

    Inbound SSH: Allowing SSH traffic to accept inbound traffic from authorized
    external device to squid proxy, (**Note:** specify your own public IP to
    prevent an authorized connection to squid proxy (optional)).

    ![](media/c7582805f8e810ca6b8cb31b48f7fc5e.jpg)

    Inbound squid proxy: allowing 3128 port traffic from internet to squid
    proxy.

    ![](media/d48e47a3323e30d965dda8e503d5c62e.jpg)

    Outbound HTTPS:allowing HTTPS outbound traffic and it is used for various
    Deep Security cloud services, you can restrict IP addresses in HTTPS port
    ,for more info please check this
    URL:[https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html\#Deep2](https://help.deepsecurity.trendmicro.com/10/0/Manage-Components/ports.html%23Deep2)

-   **Security group in private subnet:**

-   Inbound squid proxy: allowing 3128 port traffic from squid proxy to Deep
    security agent that has been installed in ec2 instance.

![C:\\Users\\omar_Almulhim\\Desktop\\guides\\screenshots2\\squid proxy\\private SG subent inbound.JPG](media/acfcccf7f3908cd0356d055f2f7d9b8d.jpg)

![](media/18a477b1149651120194c6c94d217991.jpg)

Outbound squid proxy: allowing 3128 port traffic from ec2 instance that has been
installed Deep security agent to squid proxy server.

-   **Note:** you need to assign private IP squid proxy in inbound/outbound ec2
    instances in private subnet.

**Install deep security agent**
===============================

1.  **Signup/Login Dashboard:**

![](media/ce730b0678d6ae21d5674bd0ec905d68.jpg)

You can sign up or login by using this URL: <https://cloudone.trendmicro.com/>

![](media/9d3c61e607ca5f671fea6034b9cae6cd.jpg)

After successfully registering, login your account, you will see cloud one
dashboard click on workload security.

1.  **Add AWS account:**

-   Cloud one workload security dashboard will be opened click on computers tab
    then on left side right click on computers then choose add AWS account.

    ![](media/03ebc9225f1dd0ddbff3911eecdae5df.jpg)

1.  **Setup type:**

-   You can add AWS account by using quick & Advanced.

    ![](media/f48164668c9b24d47b76ee4244736507.jpg)

1.  **Add squid proxy server in workload security:**

![](media/e9211e8777595971201e215317b135ec.jpg)

Go to administration then click system setting, click new to add new proxy.

-   In new proxy properties interface, write the private IP squid proxy with
    port.

    ![](media/31b9774ee5df234fd54d00d3109a3ae2.jpg)

1.  **Deploy deep security agent on server:**

![](media/8d0d56aa6bf78a1f4edb3a09f4c434fa.jpg)

In cloud one workload dashboard, click on support and you will see two ways of
deployment.

-   **Download agents:**

    If you want to download Agent package installer and install it manually.

    **Note**: installation guide for manually install deep security
    agent:<https://help.deepsecurity.trendmicro.com/10_2/aws/Get-Started/Install/install-dsa.html>

    ![](media/c4a97d9edbadebb7dfa34de22f3349e2.jpg)

-   **Deployment script:**

    This option will install agent by using script.

    Installation guide for install deep security agent using deployment script:
    <https://help.deepsecurity.trendmicro.com/10_2/aws/Add-Computers/ug-add-dep-scripts.html>

-   In proxy to contact deep security manager & proxy to contact relay options:

    ![](media/ee469bb1a52574669c26ec22dec8c7e2.jpg)

    you need to select squid proxy that you already added in previous step.

1.  **Check the state of deep security agent after deployment:**

-   Go to computer and see the status of instance.

![](media/999c5a6cd4c32caa91676fb92ad7a80a.jpg)
