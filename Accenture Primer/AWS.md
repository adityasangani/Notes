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
