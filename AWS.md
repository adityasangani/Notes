# AWS
## AWS S3
### Buckets
- It is a container of objects that you want to store in a namespace.
- Bucket name has to be globally unique.
- It is basically like a general purpose file system.
### Objects
- The content that you are storing inside your buckets. Can include media content, json files, csv files, SDK files, etc.
- Object maximum size is 5TB.
### How to Access Content From S3
- By URL (can only be used if S3 bucket is publically exposed) : http://s3.amazonaws.com/<BUCKET_NAME>/<OBJECT_NAME>
- Programmatically

![image](https://github.com/user-attachments/assets/4ec5da52-8810-4cb2-893b-bd7af12e8ff5)

### S3 Storage Classes
- Allow you to reduce costs, but with certain sacrifices (higher latency, less availability etc.)
- Examples: Standard, Intelligent, Infrequent Access, Glacier.
- Standard Tier (Hot Data) -> Infrequent Access (after a month) -> Glacier (Cold Data) (when it becomes 6 month old). No need to do this manually; use Lifecycle Rules that automate the data movement process.

### S3 Security
- Public access is blocked by default.
- Data Protection: Highly durable and availability guarantees, encryption in transit and at rest.
- Access: Access and resourced based controls is managed with AWS IAM (Identity and Access Management).
- Auditing: AWS supports access logs, action based logs, alarms.
- Infrastructure security: Built on top of AWS Cloud Infrastructure.

## Amazon EC2
- We use AWS S3 for storing data (just like google drive). On the other hand, we use Amazon EC2 when we need to run applications or process data.
- Key point: EC2 is for computation — it’s where you run your software and services, process data, or host websites and apps that need to "do work."
- There are various family types in an instance. t2 is the general purpose instance.
- Key pair(login): to connect to your instance. If you forget it, you won't be able to connect to your server.

## AWS VPC (Virtual Private Cloud)
- It is like a data center.
![image](https://github.com/user-attachments/assets/0896a408-7805-48d0-9093-79e3e14c8044)
- Inside this VPC everything can be created (like EC2, Kubernetes, etc.)
- Private subnet's resources won't be accessible to the public, in constrast to the public subnet.
- Internet gateway is the gateway to accessing the internet.
- We must create an internet gateway in our VPC so that our subnet can communicate with the internet.
- Route table will be used to navigate through the internet gateway and the public and private subnets.

In the AWS console, we must do the following inside the VPC once it is created:
- create internet gateway
- create public and private subnets
- create public and private route tables
To give the public route table access to the internet, go inside it, put destination as 0.0.0.0/0 (0.0.0.0 means 'This network') and target as Internet Gateway.
Next, we must associate the route table to the subnets. For that, in your route table go to 'Subnet associations' >  'Edit subnet associations' > select your subnet.
Don't associate private subnet to the internet.

## Load Balancing
- We might have N servers, with servers getting requests which are supposed to be processed. These requests can also be called 'load'. The load balancer distributes these loads among the N servers.

## Proxy vs Reverse Proxy vs Load Balancer
1. Forward Proxy
   - Sits between clients and the public internet.
   - Forward Proxy = Client-Facing: It primarily serves the client by forwarding requests to a server.
2. Reverse Proxy
   - Proxy on the receiving end.
   - Manages the incoming requests and distributes them to the right servers.
   - Hence, reverse proxy sits on the server side.
   - Functionalities of a Reverse Proxy:
       - **Load Balancing**: Distributing incoming traffic across multiple backend servers to balance the load.
       - Acting as a shield to protect the servers.
         So, the reverse proxies will scan the requests, ensure SSL encryption is enabled (so the traffic is encrypted), and will check for any security threats.
       - Caching content to reduce load on backend servers.
       - Logging functionality for troubleshooting purposes.
  A popular reverse proxy is NGINX.
So basically, Load balancing is just one of the many functionalities of a reverse proxy.
![image](https://github.com/user-attachments/assets/3377c1f1-27ae-4ce0-ae23-e2d067fbbe2c)
So, we encapsulate proxy and backend servers into a private network. So, the AWS Load Balancer does load balancing to the reverse proxy.
**So why do we need to load balance twice?**
- Reverse proxy has more intelligent fine-grained load balancing which enables us to have more intelligent routing possible.
- So while Cloud load balancer distribute traffic based on simple algorithms, we can use more advanced balancing in reverse proxy.

### Bonus
- Node.js can act as a web server. You can build a reverse light-weight proxy and can do load balancing with built-in cluster module.
  ![image](https://github.com/user-attachments/assets/f04e856a-38e1-4bab-9fd2-ee53804c6efc)
 
## Shared Responsibility Model
