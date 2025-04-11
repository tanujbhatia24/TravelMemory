# Travel Memory Application

[![Deployment](https://img.shields.io/badge/Deployed-EC2-green.svg)](#deployment-status)
[![Made With](https://img.shields.io/badge/Made%20With-MERN-blueviolet)](#tech-stack)

A full-stack travel memory journal web app built using the **MERN stack**, deployed on **AWS EC2**, and load-balanced with custom domain management via **Cloudflare**.

---

## Repository

🔗 [GitHub: Travel Memory App](https://github.com/UnpredictablePrashant/TravelMemory)

---

## Features

- Node.js backend & React frontend deployment.
- Load balancing via AWS Application Load Balancer.
- Multi-instance scaling with EC2 & AMI snapshots.
- MongoDB Atlas database.
- Nginx reverse proxy for port management.
- Domain setup with Cloudflare (e.g. `tanujbhatia.site`).

---

## Tech Stack

- **Frontend**: React
- **Backend**: Node.js, Express
- **Database**: MongoDB Atlas
- **Cloud**: AWS EC2, Load Balancer, AMI, VPC
- **DNS**: Cloudflare
- **Web Server**: Nginx

---

### Prerequisites
1. Ensure backend IP is whitelisted in MongoDB Atlas.
2. All EC2 instances used were launched in ap-south-1 (Mumbai) region.
3. Snapshots (AMIs) can be reused for creating instances & scailing.

---

## Quick Deployment Guide

### Backend Setup

1. Launch EC2 Instance (Linux) for backend setup.<br>
   Public IP - 3.110.217.207<br>
   Private IP - 172.31.4.142
   ![image](https://github.com/user-attachments/assets/e6477b84-4ff0-4e42-8031-db698d79636d)

3. Install Node.js and clone repo.
   ```bash
   # Download and install fnm:
   curl -o- https://fnm.vercel.app/install | bash
   
   # Download and install Node.js:
   fnm install 22
   
   # Verify the Node.js version:
   node -v # Should print "v22.14.0".
   
   # Verify npm version:
   npm -v # Should print "10.9.2".

   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to backend folder and create .env file and install the dependencies of the application.
   ```bash
   # Go to backend directory
   cd ~/TravelMemory/backend

   # create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements)
   MONGO_URI=your_mongo_uri
   PORT=3000

   # install the dependencies
   sudo npm install
   ```
5. Start the backend application at port 3000 and validate.<br>
   <img width="511" alt="image" src="https://github.com/user-attachments/assets/3e964302-bc01-4b36-9e89-5398ec45de4d" /><br>
   <img width="532" alt="image" src="https://github.com/user-attachments/assets/026f1b94-4cc4-4a2d-819b-2bfc3768df02" /><br>
    
6. Setup Nginx reverse proxy on port 80.
   ```bash
   sudo yum install nginx -y
   sudo systemctl enable nginx
   sudo nano /etc/nginx/nginx.conf
   ```
   Example nginx config.
   ```bash
   server {
    listen 80;
    location / {
        proxy_pass http://localhost:3000;
     }
   }
   ```
   Restart & Reload nginx new config.
   ```bash
   sudo systemctl reload nginx
   sudo systemctl restart nginx
   sudo systemctl nginx -t #to validate nginx config file
   ```
   Start the nodejs application.
   ```bash
   node index.js
   ```
7. Now we can access the application at port 80 because of reverse proxy.<br>
   <img width="514" alt="image" src="https://github.com/user-attachments/assets/def014a6-099b-42ab-8064-3b572bb4c862" /><br>
   <img width="527" alt="image" src="https://github.com/user-attachments/assets/232d9e0e-2f04-406d-846b-abb3c3fa7b3f" />
---

### Frontend Setup

1. Launch separate EC2 instances for frontend.<br>
   Public IP - 43.205.91.11<br>
   Private IP - 172.31.25.183
   ![image](https://github.com/user-attachments/assets/b02f329a-7981-400f-8872-1b44fc39dbd8)

3. Install Node.js, clone the same repository.
   ```bash
   # Download and install fnm:
   curl -o- https://fnm.vercel.app/install | bash
   
   # Download and install Node.js:
   fnm install 22
   
   # Verify the Node.js version:
   node -v # Should print "v22.14.0".
   
   # Verify npm version:
   npm -v # Should print "10.9.2".

   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to frontend folder and create .env file and install the dependencies of the application. Create .env file.
   ```bash
   # Go to frontend directory
   cd ~/TravelMemory/frontend

   # create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://backend_server_ip

   # install the dependencies
   sudo npm install
   ```
5. Update urls.js with backend IP or domain.
   ```bash
   # Go to frontend/src directory
   cd ~/TravelMemory/frontend/src
   
   # Put the following content (remember to change it based on your requirements): 
   export const baseUrl = process.env.REACT_APP_BACKEND_URL #Calling the value from .env file directy using variable "REACT_APP_BACKEND_URL"
   ```
6. Start the frontend application at port 3000 and validate.
   ```bash
   npm start
   ```
   <img width="516" alt="image" src="https://github.com/user-attachments/assets/9540d27f-93b5-449d-be90-ab350d3883da" />

7. Setup Nginx reverse proxy on port 80 for frontend just like backend.
   ```bash
   sudo yum install nginx -y
   sudo systemctl enable nginx
   sudo nano /etc/nginx/nginx.conf
   ```
   Example nginx config.
   ```bash
   server {
    listen 80;
    location / {
        proxy_pass http://localhost:3000;
     }
   }
   ```
   Restart & Reload nginx new config.
   ```bash
   sudo systemctl reload nginx
   sudo systemctl restart nginx
   sudo systemctl nginx -t #to validate nginx config file
   ```
   Start the application
   ```bash
   npm start
   ```
8. Now we can access the application at port 80 because of reverse proxy.<br>
   <img width="450" alt="image" src="https://github.com/user-attachments/assets/fae03525-5226-4720-927a-3c1973561a0e" />
---

### Load Balancing & Scaling

1. Create AMIs for both frontend and backend instances.
2. Launch multiple instances using those AMIs.
3. Create Target Groups in EC2 for frontend and backend.
4. Create Application Load Balancers and assign target groups.
5. Verify traffic distribution and availability.

---

### Custom Domain Setup Using Cloudflare

1. Domain: tanujbhatia.site.
2. Subdomain: api.tanujbhatia.site.
3. Add:
    1. CNAME record pointing to Load Balancer DNS.
    2. A record (if necessary) pointing to frontend instance IP

Update .env & urls.js in frontend to use your new domain/subdomain.

---

### Deployment Status

1. Backend on EC2 with Nginx (port 80).
2. Frontend on EC2 with Nginx (port 80).
3. MongoDB connected via Atlas.
4. Load balancing enabled (multi-AZ support).
5. Domain/subdomain setup complete.

