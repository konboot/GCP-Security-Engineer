### What is VPC Network in Google Cloud?

A **Virtual Private Cloud (VPC) Network** in Google Cloud is a global, private, and isolated virtual network that allows you to securely connect and manage your Google Cloud resources. It provides networking functionality for your Google Cloud projects, enabling you to define IP ranges, create subnets, and configure firewalls, routes, and more.

### Key Features of VPC Network

1. **Global Scope**: VPC networks are global, spanning all regions within Google Cloud.
2. **Subnets**: You can create subnets in different regions, and each subnet can have its own IP range.
3. **Private IP Addresses**: Resources within a VPC can communicate using private IP addresses.
4. **Firewalls**: Control traffic to and from instances with firewall rules.
5. **Routes**: Define how traffic flows within the network and to the internet.
6. **Peering and VPN**: Connect VPCs to other networks, including on-premises networks.

### Real-Life Example: Setting Up a Web Application with a VPC Network

Imagine you're setting up a web application for a retail business. Your infrastructure consists of front-end web servers, back-end application servers, and a database. You want to ensure that your resources are secure and can communicate with each other efficiently.

#### Step-by-Step Implementation

1. **Create a VPC Network**:
   - Name your VPC network `retail-vpc`.
   - Define the IP range for the VPC network (e.g., `10.0.0.0/16`).

2. **Create Subnets**:
   - **Front-end Subnet**: For your web servers, create a subnet called `frontend-subnet` in the `us-central1` region with IP range `10.0.1.0/24`.
   - **Back-end Subnet**: For your application servers, create a subnet called `backend-subnet` in the `us-central1` region with IP range `10.0.2.0/24`.
   - **Database Subnet**: For your database, create a subnet called `database-subnet` in the `us-central1` region with IP range `10.0.3.0/24`.

3. **Deploy Instances**:
   - Deploy web server instances in the `frontend-subnet`.
   - Deploy application server instances in the `backend-subnet`.
   - Deploy database instances in the `database-subnet`.

4. **Configure Firewall Rules**:
   - Allow incoming HTTP/HTTPS traffic to the `frontend-subnet`.
     ```shell
     gcloud compute firewall-rules create allow-http-https \
         --network retail-vpc \
         --allow tcp:80,tcp:443 \
         --target-tags web-server \
         --source-ranges 0.0.0.0/0
     ```
   - Allow internal communication between subnets for application and database communication.
     ```shell
     gcloud compute firewall-rules create allow-internal \
         --network retail-vpc \
         --allow icmp,tcp,udp \
         --source-ranges 10.0.0.0/16
     ```

5. **Set Up Routes**:
   - Default route to the internet for the `frontend-subnet` to allow web servers to access the internet.
     ```shell
     gcloud compute routes create default-internet-route \
         --network retail-vpc \
         --destination-range 0.0.0.0/0 \
         --next-hop-gateway default-internet-gateway
     ```

6. **Peering and VPN**:
   - If you have an on-premises data center, set up a VPN or VPC peering to connect your on-premises network to your Google Cloud VPC.
     - VPC Peering: Allows internal IP communication across VPC networks.
     - VPN: Secures communication between on-premises network and Google Cloud VPC.

### Summary

In this example, the VPC network (`retail-vpc`) provides an isolated, secure, and efficient environment for your web application. Subnets segregate different components (web servers, application servers, database) for better management and security. Firewall rules control access, while routes define how traffic flows within the network and to the internet. Peering and VPN extend the network to on-premises infrastructure, ensuring seamless connectivity.
