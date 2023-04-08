# AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY

The objective of this project is to create a secure infrastructure within the AWS VPC (Virtual Private Cloud) network for a company that utilizes WordPress CMS for their main business website and a Tooling Website for their DevOps team. To enhance security and performance, the company has opted to employ a reverse proxy technology provided by NGINX. The resulting infrastructure will resemble the diagram below.

![tooling_project_15](https://user-images.githubusercontent.com/94229949/230491127-a51fbecb-9c65-44c2-a122-e7b4dba7c36c.png)


Create a Virtual Private Cloud (VPC) with the name "B-VPC". & enable DNS hostnames.

![b-vpc4](https://user-images.githubusercontent.com/94229949/230495663-f623adbe-660e-4be7-befa-460661b8bc26.png)

Create an Internet gateway and attach to the VPC.

![b-vpc1](https://user-images.githubusercontent.com/94229949/230495955-26fe7033-ae33-49c5-84be-28d0c5693694.png)


![b-vpc2](https://user-images.githubusercontent.com/94229949/230495978-8c48f65c-fc63-4b3e-a77e-ac6414b56013.png)


![b-vpc3](https://user-images.githubusercontent.com/94229949/230495991-a8760c24-b346-473f-ac84-336ecfa06ffe.png)


The next step is to create a public and private subnet for each availability zone. There should be two subnets in availability zone A and B, respectively, for the public subnet, and for the private subnet, four subnets should be created, with two subnets in each availability zone A and B, resulting in a total of four subnets. These steps align with the provided diagram above.

![b-vpc5](https://user-images.githubusercontent.com/94229949/230499704-3f2cb4b7-671c-4880-9b32-680a2e9e45a5.png)


![b-vpc6](https://user-images.githubusercontent.com/94229949/230499740-00e68a66-84d6-46cf-89a7-d19f391ec042.png)


![b-vpc7](https://user-images.githubusercontent.com/94229949/230499759-b3385ce2-2079-4447-a875-f8231f36e1c3.png)


Next step is to create two route tables, one for public and the other for private . Once created, navigate to each route table and select "Subnet Association" followed by "Edit Subnet Association". Choose the two public subnets for the public route table, and the four private subnets for the private route table.

![b-vpc8](https://user-images.githubusercontent.com/94229949/230501646-6782fd98-011c-4e93-a646-fe31a33a4dcb.png)


![b-vpc9](https://user-images.githubusercontent.com/94229949/230501667-54c90090-748a-4c9c-b970-15e066a6c502.png)

![b-vpc10](https://user-images.githubusercontent.com/94229949/230501685-a2215d3b-575d-4e40-b027-4aa2d35c1abe.png)

![b-vpc11](https://user-images.githubusercontent.com/94229949/230501714-74d36de7-a1fc-4348-866d-2b0bc5b281d8.png)


Edit a route in public route table, and associating it with the Internet Gateway, this action enables a public subnet to be accessible from the internet.

![b-vpc12](https://user-images.githubusercontent.com/94229949/230502261-47263124-83ce-4cd1-b9dc-8b8e6ef30abe.png)

Create the Nat-Gateway,  and attached the Elastic IP created.

![b-vpc13](https://user-images.githubusercontent.com/94229949/230502758-ccdd54f7-5536-4f9f-acf6-0eea28bb9384.png)

![b-vpc14](https://user-images.githubusercontent.com/94229949/230502790-63431738-2f8d-49ce-88dc-ea1d0cb89017.png)

Edit routes in the private route table and associate it with the Nat-Gateway

![b-vpc15](https://user-images.githubusercontent.com/94229949/230504111-697686f4-0c79-4b73-9798-1a673710b0f7.png)

![b-vpc16](https://user-images.githubusercontent.com/94229949/230504134-35e416b3-e655-4c7b-abc4-bd68ff1fb7fb.png)

## Create security group for:

-External Load balancer - Traffic from the internet (http, https).


-Bastion - SSH access from your computer.


-Nginx (Reverse Proxy Server) - Source of traffic only from the external load balancer and SSH from Bastion.


-Internal Load balancer - traffic only form Nginx.



-Webserver - traffic source from Internal Load Balancer and SSH from Bastion only.



-Datalayer - traffic from Bastion (Mysql), Webserver (NFS and Mysql).


![b-vpc17](https://user-images.githubusercontent.com/94229949/230512364-09585a02-e1ef-4261-ba02-cdc845eec06f.png)


![b-vpc18](https://user-images.githubusercontent.com/94229949/230512384-959724af-d7f0-432e-9d28-ac64e63612dd.png)


![b-vpc19](https://user-images.githubusercontent.com/94229949/230512389-bf9d404e-2be9-4846-ae2a-8829f023d9c1.png)


![b-vpc19a](https://user-images.githubusercontent.com/94229949/230512418-a9e43a6c-3bb5-4afd-8a1e-724c196c7266.png)


![b-vpc20](https://user-images.githubusercontent.com/94229949/230512430-a5170dcf-1830-42ce-912d-2f9600335aef.png)


![b-vpc21](https://user-images.githubusercontent.com/94229949/230512460-f447f5f7-59bf-4bb3-9602-4e56d1682a8c.png)


it is important to create a certificate,this is because when setting up an application load balancer, a certificate needs to be selected, as instances behind the load balancer listen to traffic from port 443. Once the certificate is created, it is necessary to create a record on Route 53 and attach the certificate to all load balancers. If DNS validation is selected, the Route 53 record will be automatically updated.


![b-vpc22](https://user-images.githubusercontent.com/94229949/230513009-5cbbb828-8d6f-41b9-bbd6-228ebf835b39.png)


To make the Amazon EFS available in a specific subnet, you must add a mount target to the filesystem and specify the subnet. In this case, it is advisable to specify private subnet 1 and 2 so that all resources in these subnets can mount the file system. To ensure the security of the data layer, the security group settings for the mount target should be set to the data layer security group.


![b-vpc23](https://user-images.githubusercontent.com/94229949/230514585-63d28d7c-713e-4844-9bef-33910d3e7da9.png)


![b-vpc24](https://user-images.githubusercontent.com/94229949/230514602-2b95b15f-0ef0-4890-9391-b732d517a120.png)


The next step is to create an access point that specifies the location where the webservers will mount. This will create two mount points, one for the Tooling server and another for the Wordpress server.


![b-vpc25](https://user-images.githubusercontent.com/94229949/230515141-9149768b-e5b7-444b-8094-3ee9f9ac5cb7.png)


Before creating an Amazon RDS instance, it is necessary to create a KMS key that will be used to encrypt the RDS instance. 


![b-vpc26](https://user-images.githubusercontent.com/94229949/230516291-b37163b7-f101-4a20-8a12-0128c87d0807.png)


![b-vpc27](https://user-images.githubusercontent.com/94229949/230516312-70b99ba8-dcab-44a5-b21a-03e8b215706a.png)


create a Database and a subnet group in RDS using the data layer security group.


![b-vpc28](https://user-images.githubusercontent.com/94229949/230516571-35f28142-b0f0-48fc-b634-3a65ab101912.png)


![b-vpc29](https://user-images.githubusercontent.com/94229949/230516582-5c27cac4-6051-4cd6-86d9-ed8a605ecf00.png)

## Before creating the Autoscaling group, you need to perform the following steps:


Create three instances using Red Hat as the operating system.




Assign the security group to each instance, with names Bastion, Nginx, and Webserver.



Launch each instance and carry out the necessary installation and configuration on them.


For Bastion, Nginx and Webserver.


yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm


yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm


yum install wget vim python3 telnet htop git mysql net-tools chrony -y


systemctl start chronyd


systemctl enable chronyd



Dependencies for Nginx and Webserver (needed so that our servers can function properly on all the redhat instance)


setsebool -P httpd_can_network_connect=1


setsebool -P httpd_can_network_connect_db=1


setsebool -P httpd_execmem=1


setsebool -P httpd_use_nfs 1


To mount targets on the Elastic File System, you need to install the Amazon EFS utils on the Nginx and Webserver instances.


git clone https://github.com/aws/efs-utils


cd efs-utils


yum install -y make


yum install -y rpm-build


make rpm 


yum install -y  ./build/amazon-efs-utils*rpm


In order to secure the connection between the load balancer and the webserver via port 443, a self-signed certificate needs to be installed on the Nginx instance. This is necessary because the load balancer will be sending traffic to the webserver on port 443 and listening on port 443 as well.


sudo mkdir /etc/ssl/private


sudo chmod 700 /etc/ssl/private


openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ACS.key -out /etc/ssl/certs/ACS.crt


sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048


![b-vpc30](https://user-images.githubusercontent.com/94229949/230569319-dde91665-576d-4988-a9e5-ec4c46764663.png)



![b-vpc31](https://user-images.githubusercontent.com/94229949/230569333-ea888f20-3984-4dcf-847d-19da2e1c2bb4.png)



![b-vpc32](https://user-images.githubusercontent.com/94229949/230569391-beed85a8-c137-4880-8aea-2753855fdba3.png)

After successfully generating the certificate, you can enter the private IPv4 DNS of the Nginx or Webserver instance as the common name. Note that the load balancer does not validate the certificates, but it still needs to be installed.


To use the certificate, you need to specify the path to the ACS.crt and ACS.key files in the Nginx configuration.


For each of the instance created, we create AMI on each.


![b-vpc34](https://user-images.githubusercontent.com/94229949/230568949-7635f581-82c5-49d1-bb82-3c57264da781.png)


Since Nginx, WordPress, and Tooling are all behind a load balancer, you need to create target groups for each of them. The instances launched by the Auto-Scaling group will be added to these target groups.


![b-vpc-1](https://user-images.githubusercontent.com/94229949/230579476-d445d260-dcdc-430d-b848-7b3090f1cc9d.png)


![b-vpc-2](https://user-images.githubusercontent.com/94229949/230579502-a3b800ab-d32f-4519-894a-abc31d3a4337.png)



![b-vpc-3](https://user-images.githubusercontent.com/94229949/230579514-66bb9184-f74a-475d-8245-23f688c66584.png)


You will need to create two  load balancers: an external load balancer and an internet-facing load balancer. Keep in mind that for a load balancer to work, it needs to have at least two availability zones.


![b-vpc36](https://user-images.githubusercontent.com/94229949/230582745-9b29fc87-0326-4a8d-b622-f426ed91cce8.png)


You can configure a rule to cache tooling requests and forward them to the tooling target. To do this, select the internal load balancer and go to the listeners section.


![b-vpc35](https://user-images.githubusercontent.com/94229949/230583731-6ea5a878-a47c-4a1d-9a6c-51481ca4390b.png)


create launch templates for Bastion, Nginx, Wordpress, and Tooling. Wordpress and Tooling will use the Webserver AMI.Input a bash script in the user data section.


![b-vpc41](https://user-images.githubusercontent.com/94229949/230686743-f796c2bd-5aa2-4ca1-a577-5523a5f56cb4.png)


![b-vpc37](https://user-images.githubusercontent.com/94229949/230687149-e3cd578c-3f1a-427e-99bd-be67380e4990.png)


![b-vpc39](https://user-images.githubusercontent.com/94229949/230687211-6b96a1c4-026f-4d59-84cd-196be58cf394.png)


![b-vpc40](https://user-images.githubusercontent.com/94229949/230687217-154eb515-9e96-4cd3-9fff-a767d0329887.png)


![b-vpc42](https://user-images.githubusercontent.com/94229949/230687229-b56101a4-dfd9-4f04-b4ac-a85825ba7579.png)


## configuring auto-scaling group


Selecting the right launch template


Selecting the VPC


Selecting both public subnets


Enabling Application Load Balancer for the AutoScalingGroup (ASG)


Selecting the target group you created before


Ensuring health checks for both EC2 and ALB


Setting the desired capacity, Minimum capacity and Maximum capacity to 2


Setting the scale out option if CPU utilization reaches 90%


Activating SNS topic to send scaling notifications

![b-vpc47](https://user-images.githubusercontent.com/94229949/230690213-878cf107-aeb3-45dd-b006-33b5d7fd6c3a.png)


![d-14](https://user-images.githubusercontent.com/94229949/230690246-891d800a-e038-4ee4-946a-f77013c6d3b2.png)


![d-15](https://user-images.githubusercontent.com/94229949/230690254-7014acc1-175d-4fa1-8632-195ce02a1403.png)


Create the tooling and wordpress database on mysql.


![b-vpc60](https://user-images.githubusercontent.com/94229949/230707025-47a75261-1d62-47cf-b368-44330dc06367.png)


Create record for our load balancer (route 53).


![b-vpc46](https://user-images.githubusercontent.com/94229949/230707584-50e919c2-edcf-4bd6-8393-0126ada5dd40.png)


Check if All target groups are healthy.


![b-vpc43](https://user-images.githubusercontent.com/94229949/230707683-f8c1381b-2a36-428c-b655-8b6486d15649.png)


![b-vpc44](https://user-images.githubusercontent.com/94229949/230707685-2adf6986-9008-458d-82ec-a4372a0c640c.png)



![b-vpc45](https://user-images.githubusercontent.com/94229949/230707689-4bd1d31b-5df6-4b4c-9a6e-670d54c97978.png)


## Result


![b-vpc51](https://user-images.githubusercontent.com/94229949/230709564-8d8bf34b-23b3-4b5b-931d-1159913977b5.png)




![b-vpc52](https://user-images.githubusercontent.com/94229949/230709576-b28c4e99-84c6-4e9d-94c4-0c5b9d31206d.png)



![b-vpc53](https://user-images.githubusercontent.com/94229949/230709585-1cb6973a-d02d-42d0-b65d-2ee49355c3eb.png)
