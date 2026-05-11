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
---
☁️ Infrastructure Details
EC2 Instances
Instance	Public IP	Type	Zone	Status
TravelMemory-Server	51.21.127.223	t3.micro	eu-north-1a	✅ Running
TravelMemory-Server-2	51.20.78.237	t3.micro	eu-north-1a	✅ Running
TravelMemory-Server-3	16.171.114.47	t3.micro	eu-north-1a	✅ Running
Load Balancer
Property	Value
Name	TravelMemory-ALB
Type	Application Load Balancer
Scheme	Internet-facing
Listener	HTTP:80
Target Group	TravelMemory-TG
Target Health	3/3 Healthy ✅
DNS & SSL
Property	Value
Domain Registrar	GoDaddy
DNS Provider	Cloudflare (Free)
Nameservers	maeve.ns.cloudflare.com / ruben.ns.cloudflare.com
SSL Mode	Flexible
A Record	mytravelmemory.in → Proxied
CNAME	www → ALB DNS → Proxied
---
🛠️ Tech Stack
Layer	Technology	Details
Frontend	React.js	Production build served by Nginx
Backend	Node.js + Express	Port 3001 · Managed by PM2
Database	MongoDB Atlas	M0 Free Tier
Web Server	Nginx	Reverse proxy + static files
Process Manager	PM2	Auto-restart + boot startup
Cloud	AWS EC2	t3.micro · Ubuntu 22.04 LTS
Load Balancer	AWS ALB	Application LB · HTTP:80
DNS / CDN	Cloudflare	Free plan · Flexible SSL
---
📋 Deployment Phases
Phase	Title	Status
Phase 1	EC2 Launch & Server Configuration	✅ Complete
Phase 2	Backend Deployment (Node.js + MongoDB)	✅ Complete
Phase 3	Frontend Deployment (React + Nginx)	✅ Complete
Phase 4	Load Balancer Setup (AWS ALB)	✅ Complete
Phase 5	Custom Domain via Cloudflare	✅ Complete
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
Port	Protocol	Purpose
22	TCP	SSH access
80	TCP	HTTP (Nginx)
443	TCP	HTTPS
3001	TCP	Node.js Backend API
---
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
