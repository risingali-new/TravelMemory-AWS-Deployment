✈️ TravelMemory — AWS Deployment
![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws) ![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white) ![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green?logo=mongodb) ![Cloudflare](https://img.shields.io/badge/Cloudflare-DNS-F38020?logo=cloudflare&logoColor=white) ![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?logo=node.js&logoColor=white) ![React](https://img.shields.io/badge/React-Frontend-61DAFB?logo=react&logoColor=black)
> A full-stack MERN travel memory application deployed on AWS with a 3-instance load-balanced architecture, Cloudflare DNS, and custom domain SSL.
---
🌐 Live URLs
	URL
Custom Domain	https://mytravelmemory.in
AWS ALB	http://travelmemory-alb-1494153123.eu-north-1.elb.amazonaws.com
---
📖 Full Deployment Guide
> For detailed step-by-step instructions for all 5 phases, refer to:
> ### 👉 [TravelMemory-Deployment-Guide.docx](https://github.com/risingali-new/TravelMemory-AWS-Deployment/blob/main/TravelMemory-Deployment-Guide.docx)
---
🏗️ Architecture Overview
```
User Browser (HTTPS)
        │
        ▼
┌─────────────────────┐
│     Cloudflare      │  DNS + CDN + Flexible SSL
│  mytravelmemory.in  │
└─────────────────────┘
        │ HTTP:80
        ▼
┌─────────────────────┐
│    AWS ALB          │  Application Load Balancer
│  TravelMemory-ALB   │  Internet-facing · eu-north-1
└─────────────────────┘
        │
   ┌────┴────┬──────────┐
   ▼         ▼          ▼
┌──────┐  ┌──────┐  ┌──────┐
│ EC2  │  │ EC2  │  │ EC2  │   t3.micro · Ubuntu 22.04
│  #1  │  │  #2  │  │  #3  │   Nginx + Node.js + React
└──────┘  └──────┘  └──────┘
        │
        ▼
┌─────────────────────┐
│   MongoDB Atlas     │  M0 Free Tier · Cloud DB
└─────────────────────┘
```
☁️ Infrastructure Details

EC2 Instances
<img width="3201" height="297" alt="image" src="https://github.com/user-attachments/assets/8ed8c741-19fc-4c99-8e97-f1d043f26a9b" />

Load Balancer
<img width="3187" height="434" alt="image" src="https://github.com/user-attachments/assets/a8030576-7c0c-49c4-9d4b-962a9815c5c0" />

DNS & SSL
<img width="3182" height="433" alt="image" src="https://github.com/user-attachments/assets/325d8964-428f-4cd1-9af8-3e82268b3c94" />

🛠️ Tech Stack
<img width="3176" height="550" alt="image" src="https://github.com/user-attachments/assets/94b97bba-abc0-49ec-a223-1655de2e0323" />

📋 Deployment Phases
<img width="3183" height="371" alt="image" src="https://github.com/user-attachments/assets/a30ec332-d6f6-47ec-a272-54baf44d6d1a" />

---
⚙️ Nginx Configuration
Each EC2 instance runs this Nginx config:
```nginx
server {
    listen 80;
    server_name mytravelmemory.in www.mytravelmemory.in;

    location / {
        root /home/ubuntu/TravelMemory/frontend/build;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://localhost:3001/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
---
🔒 Security Group Ports
<img width="3213" height="316" alt="image" src="https://github.com/user-attachments/assets/478914a3-d323-48f0-81c7-0f5ea1640b50" />

🚀 Quick Setup (Per EC2 Instance)
```bash
# 1. Install dependencies
sudo apt update
sudo apt install -y nodejs npm nginx
sudo npm install -g pm2

# 2. Clone repo
git clone https://github.com/risingali-new/TravelMemory-AWS-Deployment.git
cd TravelMemory-AWS-Deployment

# 3. Backend setup
cd backend
npm install
pm2 start index.js --name travelmemory-backend
pm2 startup && pm2 save

# 4. Frontend setup
cd ../frontend
echo "REACT_APP_BACKEND_URL=https://mytravelmemory.in/api" > .env
npm install && npm run build

# 5. Restart Nginx
sudo systemctl restart nginx
```
> 📄 For the complete step-by-step guide → [TravelMemory-Deployment-Guide.docx](https://github.com/risingali-new/TravelMemory-AWS-Deployment/blob/main/TravelMemory-Deployment-Guide.docx)
---
📁 Repository Structure
```
TravelMemory-AWS-Deployment/
├── backend/                  # Node.js + Express API
│   ├── index.js
│   ├── models/
│   └── routes/
├── frontend/                 # React application
│   ├── src/
│   ├── public/
│   └── build/                # Production build (generated)
├── TravelMemory-Deployment-Guide.docx   # Full deployment guide
└── README.md
```
---
👤 Author
risingali-new · GitHub
---
TravelMemory Deployment Assignment · AWS EC2 · 2026
