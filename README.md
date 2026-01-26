# fullstack_developer_capstone

Project Overview
This is the capstone project for the IBM Cloud Native Full Stack Development course on Coursera. It is a full-stack, cloud-native dealership management application that allows users to browse car dealerships, view reviews, and submit their own reviews with sentiment analysis.

The application consists of multiple interconnected services deployed using modern cloud-native practices, including Docker containers, Kubernetes orchestration, and IBM Cloud Code Engine.

Solution Architecture
The solution implements a microservices architecture with the following components:

text
┌─────────────────┐     ┌─────────────────────────────────────────┐
│                 │     │        Django Application                │
│     Web         ├─────►  (Dealerships Website - Main UI)        │
│    Browser      │     │                                         │
│                 │     │  • get_cars/                            │
│                 │     │  • get_dealers/                         │
└─────────────────┘     │  • get_dealers/:state                   │
                        │  • dealer/:id                           │
                        │  • review/dealer/:id                    │
                        │  • add_review/                          │
                        └────────────────┬────────────────────────┘
                                         │
                  ┌──────────────────────┼──────────────────────┐
                  │                      │                      │
        ┌─────────▼─────────┐  ┌────────▼────────┐  ┌──────────▼──────────┐
        │ Django Proxy       │  │ SQLite Database │  │ Django Proxy        │
        │ Service            │  │ (Car Make/Model)│  │ Service             │
        └─────────┬─────────┘  └─────────────────┘  └──────────┬──────────┘
                  │                                              │
        ┌─────────▼─────────┐                          ┌────────▼──────────┐
        │ Dealerships &     │                          │ Sentiment         │
        │ Reviews Service   │                          │ Analyzer Service  │
        │ (Express + MongoDB│                          │ (IBM Cloud        │
        │ Docker Container) │                          │ Code Engine)      │
        │                   │                          │                   │
        │ • /fetchDealers   │                          │ • /analyze/:text  │
        │ • /fetchDealer/:id│                          └───────────────────┘
        │ • /fetchReviews   │
        │ • /insertReview   │
        └───────────────────┘
Core Components:
Dealerships Website (Django Application) - Main web application with user interface

Django Proxy Services - Bridges between Django and external services

Car Database (SQLite) - Stores Car Make and Car Model data

Dealerships & Reviews Service (Express + MongoDB) - Manages dealer and review data in a Docker container

Sentiment Analyzer Service - Deployed on IBM Cloud Code Engine for sentiment analysis of reviews

Features
User Management: Django authentication system with React frontend

Dealership Browsing: View dealerships by state or individually

Review Management: Read and submit reviews for dealerships

Sentiment Analysis: Automatic sentiment detection for reviews (positive/negative/neutral)

Dynamic Pages: Django templates for interactive user experience

Cloud-Native Deployment: Containerized services with Kubernetes orchestration

CI/CD Pipeline: Automated linting and deployment processes

Project Structure
text
fullstack_developer_capstone/
│
├── django-app/                 # Main Django application
│   ├── dealership/             # Dealership management app
│   ├── djangapp/               # Main project configuration
│   ├── static/                 # Static files (CSS, JS, images)
│   ├── templates/              # Django HTML templates
│   ├── manage.py
│   └── requirements.txt
│
├── react-frontend/             # React frontend for user management
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── README.md
│
├── node-backend/               # Express.js + MongoDB service
│   ├── models/                 # MongoDB models
│   ├── routes/                 # API routes
│   ├── server.js
│   ├── package.json
│   └── Dockerfile
│
├── sentiment-analyzer/         # Sentiment analysis service
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── kubernetes/                 # Kubernetes deployment files
│   ├── django-deployment.yaml
│   ├── node-backend-deployment.yaml
│   ├── mongodb-deployment.yaml
│   └── services.yaml
│
├── .github/workflows/          # CI/CD pipeline definitions
│   └── ci-cd.yml
│
├── docker-compose.yml          # Local development setup
├── .gitignore
└── README.md

Getting Started
Prerequisites
Python 3.8+

Node.js 14+

Docker & Docker Compose

IBM Cloud Account

Git

Installation & Local Development
Fork and Clone the Repository

bash
# Fork the repository on GitHub
# Then clone your fork locally
git clone https://github.com/your-username/fullstack_developer_capstone.git
cd fullstack_developer_capstone
Set Up Django Application

bash
cd django-app
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
Set Up React Frontend

bash
cd ../react-frontend
npm install
npm start
Set Up Node.js Backend with MongoDB

bash
cd ../node-backend
npm install

# Start MongoDB (requires Docker)
docker run -d -p 27017:27017 --name mongodb mongo:latest

# Start the Node.js service
npm start
Run with Docker Compose (All Services)

bash
# From project root
docker-compose up --build
API Endpoints
Django Application Endpoints:
GET /get_cars/ - Get list of cars

GET /get_dealers/ - Get list of all dealers

GET /get_dealers/:state - Get dealers by state

GET /dealer/:id - Get dealer by ID

GET /review/dealer/:id - Get reviews for a specific dealer

POST /add_review/ - Add a new review for a dealer

Dealerships & Reviews Service:
GET /fetchDealers - Fetch all dealers

GET /fetchDealer/:id - Fetch dealer by ID

GET /fetchReviews - Fetch all reviews

GET /fetchReview/dealer/:id - Fetch reviews for a dealer

POST /insertReview - Insert a new review

Sentiment Analyzer Service:
GET /analyze/:text - Analyze sentiment of text (returns positive/negative/neutral)

Deployment
IBM Cloud Code Engine (Sentiment Analyzer)
bash
# Build and push Docker image
docker build -t sentiment-analyzer .
docker tag sentiment-analyzer us.icr.io/your-namespace/sentiment-analyzer:latest
docker push us.icr.io/your-namespace/sentiment-analyzer:latest

# Deploy to Code Engine
ibmcloud ce project select --name your-project
ibmcloud ce application create --name sentiment-analyzer \
  --image us.icr.io/your-namespace/sentiment-analyzer:latest \
  --port 8080
Kubernetes Deployment
bash
# Apply Kubernetes configurations
kubectl apply -f kubernetes/mongodb-deployment.yaml
kubectl apply -f kubernetes/node-backend-deployment.yaml
kubectl apply -f kubernetes/django-deployment.yaml
kubectl apply -f kubernetes/services.yam

CI/CD Pipeline
The project includes GitHub Actions workflows for:

Code Linting: Automatic Python and JavaScript linting

Testing: Unit and integration tests

Container Building: Automated Docker image builds

Deployment: Staging and production deployments

 esting
bash
# Test Django application
cd django-app
python manage.py test

# Test Node.js backend
cd ../node-backend
npm test

# Run end-to-end tests
cd ..
pytest e2e_tests/

🛠️ Built With
Frontend: React, Django Templates

Backend: Django, Node.js/Express

Database: SQLite, MongoDB

Containerization: Docker, Docker Compose

Orchestration: Kubernetes

Cloud Services: IBM Cloud Code Engine

CI/CD: GitHub Actions

License
This project was created for educational purposes as part of the IBM Cloud Native Full Stack Development capstone project on Coursera.

👥 Acknowledgments
IBM Skills Network for course materials

Coursera for the learning platform

Django, React, and Node.js open source communities


