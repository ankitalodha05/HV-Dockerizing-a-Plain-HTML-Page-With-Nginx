# Assignment: Dockerizing a Simple HTML Page with Nginx

## Objective
The goal of this assignment is to understand Docker and containerization by Dockerizing a basic HTML page using Nginx as the web server.

---

## Requirements
1. **Basic HTML Page:**
   - Create a plain HTML page named `index.html` with some content (e.g., "Hello, Docker!").

2. **Nginx Configuration:**
   - Create an Nginx configuration file named `nginx.conf` that serves the `index.html` page.
   - Configure Nginx to listen on port 80.

3. **Dockerfile:**
   - Use an official Nginx base image.
   - Copy the `index.html` and `nginx.conf` files into the appropriate locations in the container.
   - Ensure that the Nginx server starts when the container is run.

4. **Build and Run:**
   - Build the Docker image.
   - Test the container locally.

5. **Push to ECR:**
   - Create a public repository on Amazon Elastic Container Registry (ECR).
   - Push the Docker image to the ECR repository.

---

## Step-by-Step Process

### Step 1: Create `index.html`
1. Create a file named `index.html` in your project directory.
2. Add the following content:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Dockerized Nginx</title>
   </head>
   <body>
       <h1>Hello, Docker!</h1>
       <p>This is a simple HTML page served by a Dockerized Nginx server.</p>
   </body>
   </html>
   ```

---

### Step 2: Create `nginx.conf`
1. Create a file named `nginx.conf` in the same directory.
2. Add the following content:
   ```nginx
   server {
       listen 80 default_server;
       server_name localhost;

       root /usr/share/nginx/html;
       index index.html;

       location / {
           try_files $uri $uri/ =404;
       }

       # Add security headers
       add_header X-Content-Type-Options nosniff;
       add_header X-Frame-Options SAMEORIGIN;
       add_header X-XSS-Protection "1; mode=block";

       # Logging (optional for debugging)
       access_log /var/log/nginx/access.log;
       error_log /var/log/nginx/error.log;
   }
   ```

---

### Step 3: Create the `Dockerfile`
1. Create a file named `Dockerfile` in the same directory.
2. Add the following content:
   ```dockerfile
   # Use a specific version of the Nginx base image
   FROM nginx:1.25.2

   # Copy the HTML file and Nginx configuration
   COPY index.html /usr/share/nginx/html/index.html
   COPY nginx.conf /etc/nginx/nginx.conf

   # Expose port 80
   EXPOSE 80

   # Start the Nginx server
   CMD ["nginx", "-g", "daemon off;"]
   ```

---

### Step 4: Build the Docker Image
1. Open a terminal in the project directory.
2. Run the following command to build the Docker image:
   ```bash
   docker build -t my-nginx-app .
   ```
   - The `-t` flag tags the image as `my-nginx-app`.

3. Verify the image:
   ```bash
   docker images
   ```
   - Ensure `my-nginx-app` is listed in the output.

---

### Step 5: Test Locally
1. Run the container locally:
   ```bash
   docker run -d -p 80:80 my-nginx-app
   ```
   - The `-d` flag runs the container in detached mode.
   - The `-p` flag maps port `80` of the container to port `80` on your machine.

2. Access the application:
   - Open a web browser and navigate to `http://localhost`.
   - Verify that the "Hello, Docker!" page is displayed.

3. Stop the container (optional):
   ```bash
   docker ps
   docker stop <container_id>
   ```

---

### Step 6: Push the Image to Amazon ECR
1. **Create an ECR Public Repository:**
   - Log in to the AWS Management Console.
   - Navigate to **Amazon ECR** > **Public Repositories**.
   - Create a public repository named `my-nginx-app`.

2. **Authenticate Docker to ECR:**
   ```bash
   aws ecr-public get-login-password --region <your-region> | docker login --username AWS --password-stdin public.ecr.aws/<your-account-id>
   ```

3. **Tag the Image:**
   ```bash
   docker tag my-nginx-app:latest public.ecr.aws/<your-account-id>/my-nginx-app:latest
   ```

4. **Push the Image:**
   ```bash
   docker push public.ecr.aws/<your-account-id>/my-nginx-app:latest
   ```

---

### Step 7: Verify the Image on ECR
1. **Check the Image in the AWS Console:**
   - Go to **Amazon ECR** > **Public Repositories**.
   - Verify that the `my-nginx-app` image is listed.

2. **Share the Repository URL:**
   - The URL will look like: `public.ecr.aws/<your-account-id>/my-nginx-app:latest`.
   - Share or use this URL to pull the image.

---

## Conclusion
You have successfully Dockerized a simple HTML page using Nginx, tested it locally, and pushed the Docker image to Amazon ECR. This process demonstrates your understanding of Docker, Nginx configuration, and containerization basics.


