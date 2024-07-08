Frontend - ReactJS with ChakraUI
This directory contains the frontend of the application built with ReactJS and ChakraUI.

Prerequisites
Node.js (version 14.x or higher)
npm (version 6.x or higher)
Docker (for Docker-based deployment)
Setup Instructions
Manual Deployment
Clone the Repository:

bash
Copy code
git clone https://github.com/hngprojects/devops-stage-2.git
cd devops-stage-2/frontend
Install Dependencies:
Install the necessary packages using npm:

bash
Copy code
npm install
Configure API URL:
Create a .env file in the frontend directory with the following content:

plaintext
Copy code
REACT_APP_API_URL=http://your-backend-url
Run the Development Server:
Start the development server:

bash
Copy code
npm run dev
This will start the server on http://localhost:3000. Visit this URL in your browser to view the application.

Build for Production:
Create an optimized production build:

bash
Copy code
npm run build
This will generate the build files in the build directory.

Serve the Production Build:
You can serve the production build using a static file server. Install serve globally if you haven’t already:

bash
Copy code
npm install -g serve
Then serve the build directory:

bash
Copy code
serve -s build
By default, serve will run on http://localhost:5000.

Deployment Using Docker
Ensure Docker is Installed:
Install Docker by following the official installation guide.

Create a Dockerfile:
Create a Dockerfile in the frontend directory with the following content:

Dockerfile
Copy code
# Stage 1: Build the React app
FROM node:14-alpine as build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Stage 2: Serve the app with Nginx
FROM nginx:alpine

COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
Build the Docker Image:
Build the Docker image for the frontend application:

bash
Copy code
docker build -t frontend:latest .
Run the Docker Container:
Run the Docker container, mapping port 80 on the host to port 80 in the container:

bash
Copy code
docker run -d -p 80:80 frontend:latest
The application will now be accessible at http://<your-ec2-public-ip>.

Verify Deployment:
Open your browser and navigate to the public IP address of your EC2 instance to verify that the application is running.

Additional Notes
Ensure that the backend API is up and running and accessible at the URL specified in the .env file.
If using an EC2 instance, make sure that security groups are configured to allow traffic on the required ports (e.g., port 80 for HTTP).
