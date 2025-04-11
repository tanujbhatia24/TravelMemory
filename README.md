# Travel Memory Application

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

   # Install git and clone the repo
   sudo yum install git -y
   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to backend folder and create .env file and install the dependencies of the application.
   ```bash
   # Go to backend directory
   cd ~/TravelMemory/backend

   # Create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements)
   MONGO_URI=your_mongo_uri
   PORT=3000

   # Install & Build the dependencies
   sudo npm install

   # Start the application
   sudo node index.js
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
   sudo nginx -t #to validate nginx config file
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

1. Launch separate EC2 instances (Linux) for frontend.<br>
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

   # Install git and clone the repo
   sudo yum install git -y
   git clone https://github.com/UnpredictablePrashant/TravelMemory
   ```
4. Navigate to frontend folder and create .env file and install the dependencies of the application.
   ```bash
   # Go to frontend directory
   cd ~/TravelMemory/frontend

   # create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://backend_server_ip #eg. - http://3.110.217.207

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
   sudo nginx -t #to validate nginx config file
   ```
   Start the application
   ```bash
   npm start
   ```
8. Now we can access the application at port 80 because of reverse proxy.<br>
   <img width="450" alt="image" src="https://github.com/user-attachments/assets/fae03525-5226-4720-927a-3c1973561a0e" />
---

### Load Balancing & Scaling

1. Create AMIs for both frontend and backend instances.<br>
   <img width="823" alt="image" src="https://github.com/user-attachments/assets/75af8ffc-001c-427d-997f-d1d09e5a2846" /><br>
2. Launch backend & frontend instances using these AMIs.<br>
   <img width="957" alt="image" src="https://github.com/user-attachments/assets/959d9791-9771-4fa9-aac4-3b26bb42a1af" /><br>
3. For Load Balancing, create Target Groups for frontend and backend.<br>
   <img width="429" alt="image" src="https://github.com/user-attachments/assets/dd9356cd-e1c2-42fc-9d43-3b13991fb37f" /><br>
   <img width="398" alt="image" src="https://github.com/user-attachments/assets/fd619d7f-ceb8-4fec-ad6e-020c45ca1c2b" /><br>
4. Create Application Load Balancers and assign target groups.<br>
   NOTE : Make sure the Load balancer is mapped to multi AZs
   <img width="959" alt="image" src="https://github.com/user-attachments/assets/d737c3c2-d550-48b0-8821-13bf94450d4a" /><br>
   ![image](https://github.com/user-attachments/assets/b5a9b575-d518-423c-aa21-425d820da886)<br>
---

### Custom Domain Setup Using Cloudflare

1. Connect your domain & sudomain to the application using Cloudflare.<br> 
   e.g. - Domain: tanujbhatia.site<br>
   e.g. - Subdomain: api.tanujbhatia.site<br>
   
2. Create a CNAME record pointing to the load balancer endpoint.<br>
   <img width="446" alt="image" src="https://github.com/user-attachments/assets/36cab0cd-c2c6-4c7a-b60d-f65bb64bca30" />

3. Update .env & urls.js in frontend to use your new domain/subdomain.
   ```bash
   # Go to frontend directory
   cd ~/TravelMemory/frontend

   # Create .env file
   sudo nano .env

   # Put the following content (remember to change it based on your requirements): 
   REACT_APP_BACKEND_URL=http://backend_api_url #e.g. - http://api.tanujbhatia.site

   # Build the dependencies
   sudo npm run build

   # Start the application
   sudo npm start
   ```
4. Now we can access the application using domain name because of DNS settings.<br>
   ![image](https://github.com/user-attachments/assets/f12ae51c-e6ed-4e85-bb9f-41fa3911672d)<br>

   Click on "Add Experience" and enter new data through UI.<br>
   <img width="956" alt="image" src="https://github.com/user-attachments/assets/4d37bc15-44a2-4c4a-af48-a2d4630c58f6" /><br>
   
   The data get stored in the MongoDB database.<br>
   ![image](https://github.com/user-attachments/assets/35a49355-3ec9-4dd5-8c9a-8f520830fc55)
---

### Deployment Status

1. Backend on EC2 with Nginx (port 80).
2. Frontend on EC2 with Nginx (port 80).
3. MongoDB connected via Atlas.
4. Load balancing enabled (multi-AZ support).
5. Domain/subdomain setup complete.

