TravelMemory Application
Full Deployment on AWS EC2 — Assignment Submission
MERN Stack • 3-Instance Load Balanced Architecture • Cloudflare DNS

GitHub Repository
The complete source code and deployment documentation is available at:
Repository URL: https://github.com/risingali-new/TravelMemory-AWS-Deployment
Note: Replace YOUR_USERNAME with your actual GitHub username before submission.

Live Deployment URLs
Custom Domain: https://mytravelmemory.in
AWS ALB URL: http://travelmemory-alb-1494153123.eu-north-1.elb.amazonaws.com
AWS Region: eu-north-1 (Stockholm)

Phase Completion Summary
Phase	Title	Status	Key Details
Phase 1	EC2 Launch & Configuration	✅ Done	Ubuntu 22.04, Node.js 18, Nginx, PM2, Git — eu-north-1a
Phase 2	Backend Deployment	✅ Done	Node.js API on port 3001, MongoDB Atlas connected, PM2 managing process
Phase 3	Frontend Deployment	✅ Done	React app built, served via Nginx on port 80
Phase 4	Load Balancer Setup	✅ Done	3x EC2 t3.micro behind ALB, all targets Healthy
Phase 5	Custom Domain via Cloudflare	✅ Done	mytravelmemory.in live with SSL, GoDaddy NS → Cloudflare

Infrastructure Details
EC2 Instances
Instance Name	Public IP	Type	Availability Zone
TravelMemory-Server	51.21.127.223	t3.micro	eu-north-1a
TravelMemory-Server-2	51.20.78.237	t3.micro	eu-north-1a
TravelMemory-Server-3	16.171.114.47	t3.micro	eu-north-1a

Load Balancer
Name: TravelMemory-ALB
Type: Application Load Balancer (ALB)
Scheme: Internet-facing
Listener: HTTP:80 → Target Group: TravelMemory-TG
Target health: 3/3 Healthy

DNS & SSL
Domain registrar: GoDaddy
DNS provider: Cloudflare (Free plan)
Nameservers: maeve.ns.cloudflare.com / ruben.ns.cloudflare.com
A record: mytravelmemory.in → 51.21.127.223 (Proxied)
CNAME record: www → TravelMemory-ALB DNS (Proxied)
SSL mode: Flexible — Cloudflare terminates HTTPS, HTTP to origin

Database
Provider: MongoDB Atlas
Tier: M0 Free Shared Cluster
Connection: MONGO_URI via .env (not committed to Git)
Network access: 0.0.0.0/0 (Allow from anywhere)

Technology Stack
Layer	Technology	Version / Details
Frontend	React.js	Production build served by Nginx
Backend	Node.js + Express	v18.x • Port 3001 • Managed by PM2
Database	MongoDB Atlas	M0 Free tier • Cloud hosted
Web server	Nginx	Reverse proxy + static file server
Process mgr	PM2	Auto-restart + startup on boot
Cloud	AWS EC2	t3.micro • Ubuntu 22.04 LTS
Load balancer	AWS ALB	Application LB • HTTP:80
DNS / CDN	Cloudflare	Free plan • Proxied • Flexible SSL


