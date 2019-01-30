# Migrate data from AWS S3 to Alibaba Cloud OSS by using IPSec VPN and Express Connect {#concept_rms_whd_2gb .concept}

This topic describes how to migrate data from a AWS S3 bucket to a Alibaba Cloud OSS bucket by using IPSec VPN tunnel and ExpressConnect.

## Background information {#section_ngt_zmd_2gb .section}

When customers migrate data from AWS S3 to Alibaba Cloud OSS, especially cross countries, there are usually two network architectures for them to choose from:

-   Transfer data based on the Internet.
-   Transfer data based on private network.

This topic focuses on the second network architecture. The IPSEC VPN tunnel and the Alibaba Cloud ExpressConnect product portfolio are combined to meet the customer's needs of transmitting data through internal networks and increasing the security of data transmission.

The following figure shows the overall network architecture.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80669/154882987738273_en-US.png)

The advantage of this network architecture is that the data in the S3 bucket is first moved to the Alibaba Cloud Network VPC in the same region as the S3 bucket, and then the data is retransmitted to the OSS bucket of the destination region by means of the Alibaba Cloud ExpressConnnect cross-region high-speed network. This network architecture accelerates the transmission speed of cross-country data migration.

## Preparation {#section_qtg_yhr_pgb .section}

The following table describes the information about the network entities involved in the migration process.

|Provider|Network entity|Value|
|--------|--------------|-----|
|AWS|VPC in Tokyo|172.16.0.0/16|
|S3 bucket Endpoint|s3.ap-northeast-1.amazonaws.com|
|S3 bucket name|eric-s3-tokyo|
|OssImport IP address in the EC2 instance|Internal IP address: 172.16.1.183 Public IP address: 3.112.29.59|
|Customer Gateway with Strongswan|Public IP address: 3.112.29.59|
|Alibaba Cloud|VPN Gateway|Public IP address: 47.74.46.62|
|VPC in Tokyo Japan|172.24.0.0/16|
|Subnet in VPC in Tokyo Japan|172.24.0.0/20|
|VPC in Shanghai China|172.19.0.0/16|
|Subnet in VPC in Shanghai China|172.19.48.0/20|
|OSS bucket endpoint|http://oss-cn-shanghai-internal.aliyuncs.com|
|OSS bucket name|eric-oss-datastore-shanghai|

## Step 1: Install and configure Strongswan. {#section_ygt_zmd_2gb .section}

Perform the following operations in AWS to install Strongswan.

1.  Prepare a VPC and related resources.
    1.  Create a VPC with the following settings.

        ![](images/38330_en-US.png)

    2.  Create a subnet with the following settings.

        ![](images/38331_en-US.png)

    3.  Create an Internet gateway with the following settings.

        ![](images/38332_en-US.png)

        **Note:** If you want to access EC2 via Internet, such as SSH client, then this gateway is necessary, attach this Internet gateway to the VPC that you just created.

    4.  Create a security group and allow the traffic.

        ![](images/38333_en-US.png)

2.  Create an EC2 instance for Strongswan and OssImport.
    1.  Launch an EC2 instance in the VPC, subnet and security group.

        ![](images/38334_en-US.png)

        **Note:** If you want to access the EC2 instance with SSH client, you need to save the \*.pem file to local compute devices.

    2.  Test to access EC2 with SSH client.

        Run the following command to log on the the EC2 instance via SSH:

        `ssh -i 3k3j***M.pem ec2-user@ec2-3-112-29-59.ap-northeast-1.compute.amazonaws.com`

        ![](images/38335_en-US.png)

        If you cannot access the EC2 instance via SSH client, it may because that you need to add a route entry.

3.  Install Strongswan.

    Run the following code to install Strongswan and verify the version of Strongswan:

    ```
    $ wget http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/s/strongswan-5.7.1-1.el7.x86_64.rpm
    $ sudo yum install gcc
    $ sudo yum install trousers
    $ sudo rpm -ivh strongswan-5.7.1-1.el7.x86_64.rpm
    $ strongswan version
    Linux strongSwan U5.7.1/K3.10.0-957.el7.x86_64
    University of Applied Sciences Rapperswil, Switzerland
    See 'strongswan --copyright' for copyright information.
    ```

4.  Configure Strongswan.

    Run the following code to configure Strongswan:

    ```
    $ sudo vi /etc/strongswan/ipsec.conf
    Paste following setting into file:
    conn %default
    authby=psk
    type=tunnel
    keyexchange=ikev2
    auto=start
    ike=aes-sha1-modp1024
    ikelifetime=86400s
    esp=aes-sha1-modp1024
    lifetime=86400s
    conn abc_shanghai_oss
    left=172.16.1.183 //// Local IP address of EC2(Strongswan installed )
    leftsubnet=172.16.1.0/24 //// AWS Tokyo VPC CIDR.
    leftid=3.112.29.59 //// Public IP address as ID.
    right=47.74.46.62 //// Public IP address of Alibaba Cloud VPN Gateway
    rightid=47.74.46.62 //// Public IP address as ID.
    rightsubnet=100.118.102.0/24 //// Note(1)
    
    ```

    **Note:** When you ping the intranet endpoint of the OSS bucket in some ECS instances in the Alibaba Cloud Shanghai VPC, you can get the IP address which is belonged to this CIDR block. Therefore this CIDR block is used as the right subnet.

    ![](images/38336_en-US.png)

5.  Start Strongswan.
    1.  Run the following commands:

        ```
        $ sudo su – root
        # echo 1 > /proc/sys/net/ipv4/ip_forward
        
        ```

        **Note:** Then you need to add `net.ipv4.ip_forward=1` into /etc/sysctl.conf.

    2.  Run the following commands to start Strongswan:

        ```
        # systemctl enable strongswan
        # systemctl start strongswan
        # systemctl status strongswan
        
        ```


## Step 2: Create VPN gateways and IPSec connections. {#section_aft_dqq_pgb .section}

Follow these steps to create VPN gateways and IPSec connections.

1.  Create a VPN gateway and a customer gateway.
    1.  Create a VPN gateway.

        ![](images/38337_en-US.png)

    2.  Create a customer gateway.

        ![](images/38338_en-US.png)

        **Note:** In this practice, Strongswan@AWS EC2 is used as the customer gateway.

2.  Create an IPSec connection.
    1.  Make basic settings.

        ![](images/38339_en-US.png)

    2.  Make advanced settings.

        ![](images/38340_en-US.png)

        ![](images/38341_en-US.png)

3.  Check the IPSec connection status.
    1.  Check the IPSec connection status on the Alibaba Cloud console.

        ![](images/38342_en-US.png)

    2.  Check the IPSec connection status on Strongswan by running the following command:

        ```
        # systemctl status strongswan
        ```

        ![](images/38343_en-US.png)


## Step 3: Create VPC Peering Connection. {#section_n3t_zmd_2gb .section}

1.  Follow the steps in [Interconnect two VPCs under the same account](../../../../../reseller.en-US/Getting Started (New Console)/Interconnect two VPCs under the same account.md#) to create a VPC peering connection on the Alibaba Cloud Express Connect console.

    ![](images/38344_en-US.png)

2.  Add the following routing entries in the route settings both on the AWS VPC and Alibaba Cloud VPC.
    -   AWS VPC in Tokyo Japan

        ![](images/38345_en-US.png)

        ![](images/38346_en-US.png)

    -   Alibaba Cloud VPC in Tokyo Japan

        ![](images/38347_en-US.png)

        The subnets in the preceding figure are described as follows:

        -   100.118.102.0/24: VPC endpoint of OSS bucket in the China Shanghai region

        -   172.16.1.0/24: AWS VPC in Tokyo Japan

        -   172.19.48.0/20: Alibaba Cloud VPC in Shanghai China

    -   Alibaba Cloud VPC in Shanghai China

        ![](images/38348_en-US.png)

        The subnets in the preceding figure are described as follows:

        -   172.16.1.0/24: AWS VPC in Tokyo Japan

        -   172.24.0.0/20: Alibaba Cloud VPC in Tokyo Japan

3.  Test the connection between AWS VPC in Tokyo Japan and the Alibaba Cloud OSS bucket in Shanghai.

    ![](images/38349_en-US.png)

    The test results in the preceding figure show that the connection between the AWS EC2 instance and the Alibaba Cloud OSS bucket is connected. You can deploy OssImport and migrate objects from the AWS S3 bucket to the Alibaba Cloud OSS bucket.


## Step 4: Create a VPC endpoint for AWS S3 bucket. {#section_y3t_zmd_2gb .section}

In this practice, OssImport deployed on the EC2 instance in VPC is used to move data from the AWS S3 bucket to the OSS bucket. We recommend you create a VPC endpoint for the AWS S3 bucket and associate it with the route table so that OssImport can visit the AWS S3 bucket within the VPC instead of visiting the Internet IP address of the AWS S3 bucket.

1.  Create a VPC endpoint for the AWS S3 bucket.

    ![](images/38350_en-US.png)

2.  Check the VPC route table.

    ![](images/38351_en-US.png)

3.  Verify the connection between the AWS S3 VPC endpoint and the EC2 instance.

    ![](images/38352_en-US.png)


## Step 5: Migrate Data from the AWS S3 bucket to the OSS bucket by using OssImport. {#section_cjt_zmd_2gb .section}

Follow the steps in [Use OssImport to migrate data](reseller.en-US/Best Practices/Migrate data to OSS/Use OssImport to migrate data.md#) to deploy and configure OssImport.

1.  Configure the local\_job.cfg file as follows:

    ```
    srcType=s3
    srcAccessKey=AK************A
    srcSecretKey=+RW************iM3
    srcDomain=s3.ap-northeast-1.amazonaws.com
    srcBucket=eric-s3-tokyo
    srcPrefix=source_folder/
    destAccessKey=L************ic
    destSecretKey=nEtM************iDx
    destDomain=http://oss-cn-shanghai-internal.aliyuncs.com
    destBucket=eric-oss-datastore-shanghai
    destPrefix=destination_folder/
    
    ```

2.  Execute the import.sh script and check the migration status as follows:

    ```
    [root@ip-172-16-1-183 ~]# cd /home/ec2-user/ossimport
    [root@ip-172-16-1-183 ossimport]# ./import.sh
    Clean the previous job, Yes or No: yes
    Stop import service completed.
    delete jobs dir:./master/jobs/local_test/
    Clean job:local_test completed.
    submit job:/home/ec2-user/ossimport/conf/local_job.cfg
    submit job:local_test success!
    Start import service completed.
    ……
    -------------- job stats ---------------
    ---------------- job stat ------------------
    JobName:local_test
    JobState:Running
    PendingTasks:0
    DispatchedTasks:1
    RunningTasks:1
    SucceedTasks:0
    FailedTasks:0
    ScanFinished:true
    RunningTasks Progress:
    8528637A126676A4FD0D2F981ED5E0EF_1544176618182:0/191048 1/42
    ----------------------------------------
    -------------- job stats ---------------
    ---------------- job stat ------------------
    JobName:local_test
    JobState:Running
    PendingTasks:0
    DispatchedTasks:1
    RunningTasks:1
    SucceedTasks:0
    FailedTasks:0
    ScanFinished:true
    RunningTasks Progress:
    8528637A126676A4FD0D2F981ED5E0EF_1544176618182:191048/191048 42/42
    [root@ip-172-16-1-183 ossimport]#
    
    ```

    **Note:** You can find detail logs in the ossimport/logs folder.

3.  Compare the files in the AWS S3 bucket and those in the OSS bucket.

    ![](images/38353_en-US.png)

    ![](images/38354_en-US.png)


## References {#section_tlk_zyq_pgb .section}

-   [Data migration](../../../../../reseller.en-US/Tools/ossimport/Data migration.md#)

-   [Migration Technical Guide](https://www.alibabacloud.com/help/doc-detail/73912.htm?spm=a2c63.p38356.b99.9.4e023602Mor3NO)


