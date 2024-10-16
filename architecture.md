# AWS Architecture for Webtoon List Application

This document explains the AWS architecture for the Webtoon List application, outlining each component and its role in the system.


<img width="782" alt="Screenshot 2024-10-09 at 6 27 45 PM" src="https://github.com/user-attachments/assets/4376612a-f1a9-4d60-bcd1-6f7167a8210f">


## Architecture Components

### 1. Internet Gateway
- **Purpose**: The Internet Gateway allows communication between the resources in your VPC (Virtual Private Cloud) and the internet.
- **Role**: It facilitates public access to the EC2 instance where the web application is hosted.

### 2. EC2 Instance
- **Purpose**: The EC2 instance serves as the web server hosting the Webtoon List application.
- **Role**:
  - Runs the NGINX web server, which serves the static `index.html` file.
  - Processes incoming requests from users accessing the web application.

### 3. S3 Bucket
- **Purpose**: The S3 bucket is used to store images related to the manhwa titles.
- **Role**:
  - Hosts image files that are accessed by the web application.
  - Provides a scalable and durable storage solution for media files.

### 4. RDS (Relational Database Service)
- **Purpose**: RDS manages the database that stores information about manhwa titles, genres, and descriptions.
- **Role**:
  - Allows the web application to perform CRUD (Create, Read, Update, Delete) operations on the manhwa data.
  - Provides a managed database service, ensuring availability and durability of data.

### 5. Auto Scaling Group
- **Purpose**: The Auto Scaling Group automatically adjusts the number of EC2 instances based on traffic load.
- **Role**:
  - Ensures the application can handle varying amounts of traffic by adding or removing instances.
  - Maintains application performance during peak usage times.

### 6. CloudWatch
- **Purpose**: Amazon CloudWatch monitors the performance and health of AWS resources.
- **Role**:
  - Tracks metrics such as CPU utilization and network traffic for the EC2 instances.
  - Triggers alarms based on specified thresholds (e.g., CPU utilization exceeding 75%).

### 7. SNS (Simple Notification Service)
- **Purpose**: SNS is used to send notifications based on CloudWatch alarms.
- **Role**:
  - Sends alerts when the CPU usage of the EC2 instance exceeds 75%, notifying administrators to take action.
  - Can be configured to send notifications via various protocols, such as email or SMS.

## Architecture Overview

The architecture enables a scalable and efficient deployment of the Webtoon List application:

- Users access the application through the **Internet Gateway**, which routes traffic to the **EC2 instance** hosting the web server.
- The web server retrieves images from the **S3 bucket** and data from the **RDS** database as needed.
- **CloudWatch** monitors the application's performance and triggers alerts via **SNS** when certain thresholds are reached.
- The **Auto Scaling Group** ensures that the application remains responsive during traffic spikes by dynamically adjusting the number of running EC2 instances.

This architecture provides a robust foundation for hosting a cloud-based web application while ensuring scalability, security, and reliability.
