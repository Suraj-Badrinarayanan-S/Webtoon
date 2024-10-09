# Deployment Steps for Webtoon List Web Application

## 1. EC2 Setup
### Purpose
To host the web application.

### Steps
1. **Launch an EC2 Instance**:
   - Log in to the AWS Management Console.
   - Navigate to the **EC2** dashboard and click on **Launch Instance**.
   - Choose **Amazon Linux 2** or **Ubuntu** as the operating system.
   - Select **t2.micro** as the instance type (free tier eligible).
   - Click **Next** to configure instance details, keeping defaults or adjusting based on your requirements.
   - **Add Storage**: Adjust storage settings as needed, then click **Next**.
   - **Configure Security Group**: Create a new security group allowing:
     - **HTTP (Port 80)**: To serve the web application.
     - **SSH (Port 22)**: To allow SSH access from your IP.
   - Review your configuration and click **Launch**. Choose or create a new key pair and download the `.pem` file.

2. **Install a Web Server (NGINX)**:
   - Connect to your EC2 instance using SSH:
     ```bash
     ssh -i "your-key.pem" ec2-user@<EC2-Public-IP>
     ```
   - Update the instance and install NGINX:
     ```bash
     sudo yum update -y
     sudo yum install nginx -y
     sudo systemctl start nginx
     sudo systemctl enable nginx
     ```

3. **Upload the Application Code**:
   - Use `scp` to transfer the `index.html` file to the EC2 instance:
     ```bash
     scp -i "your-key.pem" index.html ec2-user@<EC2-Public-IP>:/tmp/
     ```
   - Move the file to the NGINX directory:
     ```bash
     sudo mv /tmp/index.html /usr/share/nginx/html/index.html
     ```

4. **Test Your Application**:
   - Access the application through the public IP of your EC2 instance by entering the public IP into your web browser.

## 2. S3 Setup (for Image Storage)
### Purpose
To store images related to the manhwa titles.

### Steps
1. **Create an S3 Bucket**:
   - In the AWS Management Console, navigate to S3 and create a new bucket.
   - Set the bucket name (e.g., `manhwa-images`) and ensure public access is enabled.

2. **Upload Images**:
   - Upload images related to the manhwa titles to the bucket.
   - Make sure to set permissions to allow public access.

3. **Reference Images in Your Application**:
   - Update the `index.html` file to use the public URLs of the images stored in S3.

## 3. RDS Setup (for Database)
### Purpose
To manage and store data about the manhwa titles.

### Steps
1. **Launch an RDS Instance**:
   - Go to the RDS service in the AWS Management Console and click on **Create database**.
   - Choose a database engine (e.g., MySQL or PostgreSQL).
   - Configure the instance with the db.t2.micro type for free tier eligibility.
   - Follow the wizard to set up the database.

2. **Create a Database and Table**:
   - Connect to your RDS instance using a MySQL or PostgreSQL client.
   - Create a `manhwa` table with the following schema:
     ```sql
     CREATE TABLE manhwa (
       id INT AUTO_INCREMENT PRIMARY KEY,
       title VARCHAR(255),
       genre VARCHAR(255),
       description TEXT
     );
     ```

3. **Insert Mock Data**:
   - Insert sample data into the database to be displayed in the web application.

## 4. Auto Scaling Configuration
### Purpose
To automatically adjust the number of EC2 instances based on the load.

### Steps
1. **Create a Launch Template**:
   - In the EC2 dashboard, go to **Launch Templates** and create a new template based on your EC2 instance settings.

2. **Create an Auto Scaling Group**:
   - Go to **Auto Scaling Groups** and create a new group based on the launch template.
   - Set the minimum and maximum number of instances, and configure scaling policies based on CPU utilization (e.g., add instances when CPU > 75%).

## 5. Security Configuration
### Purpose
To secure the web application.

### Steps
1. **Configure Security Groups**:
   - Ensure the EC2 security group allows:
     - HTTP (Port 80) for web traffic.
     - SSH (Port 22) limited to your IP for secure access.

2. **Set Up HTTPS Using AWS Certificate Manager**:
   - Request a public SSL certificate from ACM.
   - Attach the certificate to the EC2 instance or load balancer to enable HTTPS.
