# 🚀 MEAN Stack DevOps Deployment Assignment

## 👤 Candidate Details
**Name:** Deepesh Suthar  
**Email:** deepeshsuthar23@gmail.com  

Detail Report: https://drive.google.com/file/d/1Gjg1MhVriTjOOoj9XyQfEY5YQP0K3SdG/view?usp=sharing

---

# 📌 Project Overview
This project demonstrates containerization, CI/CD, and cloud deployment of a full-stack MEAN application using:

- MongoDB
- Express.js
- Angular
- Node.js
- Docker
- Docker Compose
- AWS EC2
- Nginx Reverse Proxy
- GitHub Actions (CI/CD)

---

# 🧰 Tools Used
- Git & GitHub
- Docker & Docker Hub
- AWS EC2 (Ubuntu 22.04)
- Nginx
- GitHub Actions

---

# ⚙️ Step 1 — Repository Setup
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/deepesh-suthar/mean-devops-assignment.git
git push -u origin main

🐳 Step 2 — Dockerization
Backend Dockerfile

FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["npm", "start"]

Frontend Dockerfile

FROM node:18 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

🐳 Step 3 — Docker Compose

version: "3"
services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"

  backend:
    build: ./backend
    container_name: backend
    ports:
      - "8080:8080"
    depends_on:
      - mongodb

  frontend:
    build: ./frontend
    container_name: frontend
    ports:
      - "80:80"
    depends_on:
      - backend

🐳 Step 4 — Push Images to Docker Hub

docker tag backend deepeshsuthar/mean-backend
docker tag frontend deepeshsuthar/mean-frontend

docker push deepeshsuthar/mean-backend
docker push deepeshsuthar/mean-frontend


☁️ Step 5 — AWS EC2 Deployment

Install Docker:
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu

Run containers:
docker run -d -p 27017:27017 --name mongodb mongo
docker run -d -p 8080:8080 --name backend --network bridge deepeshsuthar/mean-backend
docker run -d -p 80:80 --name frontend deepeshsuthar/mean-frontend

🌐 Step 6 — Nginx Reverse Proxy
sudo apt install nginx -y
sudo nano /etc/nginx/sites-available/default

server {
    listen 80;

    location / {
        proxy_pass http://localhost:80;
    }

    location /api/ {
        proxy_pass http://localhost:8080/;
    }
}

Resart:
sudo nginx -t
sudo systemctl restart nginx

🔄 Step 7 — CI/CD Pipeline (GitHub Actions)

Secrets added:

DOCKER_USERNAME

DOCKER_PASSWORD

EC2_HOST

EC2_USER

EC2_KEY

Workflow:

name: CI-CD Pipeline

on:
  push:
    branches: ["main"]

jobs:
  build-deploy:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Docker login
      run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

    - name: Build images
      run: |
        docker build -t deepeshsuthar/mean-backend ./backend
        docker build -t deepeshsuthar/mean-frontend ./frontend

    - name: Push images
      run: |
        docker push deepeshsuthar/mean-backend
        docker push deepeshsuthar/mean-frontend

    - name: Deploy to EC2
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.EC2_KEY }}
        script: |
          docker pull deepeshsuthar/mean-backend
          docker pull deepeshsuthar/mean-frontend
          docker rm -f backend frontend || true
          docker run -d --name backend -p 8080:8080 deepeshsuthar/mean-backend
          docker run -d --name frontend -p 80:80 deepeshsuthar/mean-frontend


🌍 Application Access

Frontend:
👉 http://35.154.53.12

Backend:
👉 http://35.154.53.12/api/tutorials

📸 Screenshots (To Add)

Add screenshots of:

GitHub Actions success
<img width="940" height="450" alt="image" src="https://github.com/user-attachments/assets/adbff3e6-41b6-488f-a26d-fbe667eccfc0" />
<img width="940" height="475" alt="image" src="https://github.com/user-attachments/assets/17020b29-a223-499b-ad82-496a917d8630" />
<img width="940" height="461" alt="image" src="https://github.com/user-attachments/assets/fdfde0c9-7f8e-4709-bd90-2cecaa8027e7" />
<img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/bab5f3e9-3bcc-46de-b25a-b41dedcecb01" />




Docker Hub images
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/1b0895c0-a5c5-4531-badc-e0df05cfe3f4" />
<img width="940" height="491" alt="image" src="https://github.com/user-attachments/assets/e6c111a4-2a55-4b01-8257-7a560ed88808" />



EC2 docker ps
<img width="940" height="473" alt="image" src="https://github.com/user-attachments/assets/e9affcd2-37ee-49ac-bcd7-aef6622e38d0" />




Frontend UI

<img width="1919" height="756" alt="image" src="https://github.com/user-attachments/assets/79318354-6093-439d-a1fd-6658e9fbad9a" />



Backend API

<img width="1919" height="618" alt="image" src="https://github.com/user-attachments/assets/997448af-5f77-44e6-b1fd-6ce2f34faaf2" />



Nginx config
<img width="940" height="354" alt="image" src="https://github.com/user-attachments/assets/ee80ca76-dda9-4d10-b452-6bf249aad641" />

<img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/4630bc9e-f0b5-4179-89c4-9d9a507092ff" />

<img width="940" height="390" alt="image" src="https://github.com/user-attachments/assets/09204f49-b38c-41da-a5cb-8582854141d5" />




EC2 instance page
<img width="940" height="463" alt="image" src="https://github.com/user-attachments/assets/c29907a9-f87f-449a-b529-ac40be8a095b" />
<img width="940" height="470" alt="image" src="https://github.com/user-attachments/assets/2c1b7c65-5b73-4254-84b1-a503df082ef0" />
<img width="940" height="500" alt="image" src="https://github.com/user-attachments/assets/ffcdd97e-f036-45b7-aa85-54da98a66daa" />




✅ Status

✔ Dockerized
✔ Deployed on AWS
✔ MongoDB connected
✔ Nginx reverse proxy working
✔ CI/CD automated
