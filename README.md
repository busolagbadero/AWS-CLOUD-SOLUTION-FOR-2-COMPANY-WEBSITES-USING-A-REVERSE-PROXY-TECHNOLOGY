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

Create security group for:

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


