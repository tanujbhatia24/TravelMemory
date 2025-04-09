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
3. Snapshots (AMIs) can be reused for scaling.

---

## Quick Deployment Guide

### Backend Setup

1. Launch EC2 Instances.
2. Install Node.js and clone repo.

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
3. Create .env file.
   ```bash
   # Got to backend directory
   cd TravelMemory/backend

   # Put the following content (remember to change it based on your requirements): 
   MONGO_URI=your_mongo_uri
   PORT=3000
   ```
4. Install and start.
   ```bash
   npm install
   node index.js
   ```
5. Setup Nginx reverse proxy on port 80.
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

---

### Frontend Setup

1. Launch separate EC2 instances for frontend.
2. Clone the same repository and navigate to frontend folder.
3. Create .env file.
   ```bash
   # Got to frontend directory
   cd TravelMemory/frontend

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://api.tanujbhatia.site
   ```
4. Update urls.js with backend IP or domain.
5. Run:
```bash
npm install
npm start
```
6. Set up Nginx for frontend just like backend.
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

