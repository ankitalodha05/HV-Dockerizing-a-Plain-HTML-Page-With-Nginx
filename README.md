# Assignment: Dockerizing a Simple HTML Page with Nginx

## Objective

The objective of this assignment is to learn Docker and containerization by Dockerizing a basic HTML page using Nginx as the web server.

---

## Requirements

1. **HTML Page**:
   - Create an `index.html` page with specific content.
2. **Nginx Configuration**:
   - Create an Nginx configuration file named `nginx.conf`.
   - Configure Nginx to serve the HTML page on port 80.
3. **Dockerfile**:
   - Use the official Nginx base image.
   - Copy the `index.html` and `nginx.conf` into appropriate locations in the Docker container.
4. **Build and Test**:
   - Build a Docker image and test it locally.
5. **Push to Amazon ECR**:
   - Create a public repository on Amazon Elastic Container Registry (ECR) and push the Docker image.

---

## Step-by-Step Instructions

### Step 1: Create `index.html`

Create an HTML file named `index.html` with the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>ANKITA LODHA</title>
    <style>
        body {
            font-family: mono cursiva, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #ff5349;
        }
        header {
            background-color: #4CAF51;
            padding: 10px;
            text-align: center;
            color: white;
        }
        main {
            padding: 20px;
            margin: 20px;
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        footer {
            background-color: #4CAF51;
            color: white;
            text-align: center;
            padding: 10px;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>
</head>
<body>

<header>
    <h1> "Hello Docker" </h1>
</header>

<main>
    <h2>About This Page</h2>
    <p>This is a simple HTML page created as a demonstration. You can add more content, images, and links to make it more interactive.</p>
    <p>HTML (HyperText Markup Language) is the standard markup language used to create web pages. CSS (Cascading Style Sheets) is used to style HTML elements, making the page visually appealing.</p>
</main>

<footer>
    <p>Created by Ankita Lodha &copy; 2024</p>
</footer>

</body>
</html>
```

---

### Step 2: Create `nginx.conf`

Create a configuration file named `nginx.conf` with the following content:

```nginx
server {
    listen 80 default_server;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Security headers
    add_header X-Content-Type-Options nosniff;
    add_header X-Frame-Options SAMEORIGIN;
    add_header X-XSS-Protection "1; mode=block";

    # Logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
}
```

---

### Step 3: Create the `Dockerfile`

Create a file named `Dockerfile` with the following content:

```dockerfile
# Use an official Nginx base image
FROM nginx:1.25.2

# Copy the HTML file to the web root directory
COPY index.html /usr/share/nginx/html/index.html

# Copy the custom Nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80
EXPOSE 80

# Start the Nginx server
CMD ["nginx", "-g", "daemon off;"]
```

---

### Step 4: Build the Docker Image

1. Open a terminal in your project directory.
2. Run the following command to build the Docker image:
   ```bash
   docker build -t nginx-html-docker .
   ```
   - The `-t` flag tags the image with the name `my-nginx-app`.
3. Verify the image:
   ```bash
   docker images
   ```
   - Ensure `my-nginx-app` is listed in the output.

---

### Step 5: Test Locally

1. Run the container:
   ```bash
   docker run -d -p 80:80 my-nginx-app
   ```
   - The `-d` flag runs the container in detached mode.
   - The `-p` flag maps port 80 of the container to port 80 on your machine.
2. Open a web browser and navigate to `http://localhost` to view the page.
3. Stop the container (if needed):
   ```bash
   docker ps
   docker stop <container_id>
   ```

---

### Step 6: Push the Image to Amazon ECR

1. **Create an ECR Repository:**
   - Log in to the AWS Management Console.
   - Navigate to **Amazon ECR** > **Public Repositories**.
   - Create a repository named `my-nginx-app`.
2. **Authenticate Docker to ECR:**
   ```bash
   aws ecr-public get-login-password --region <your-region> | docker login --username AWS --password-stdin public.ecr.aws/<your-account-id>
   ```
3. **Tag the Image:**
   ```bash
   docker tag my-nginx-app:latest public.ecr.aws/<your-account-id>/nginx-html-docker:latest
   ```
4. **Push the Image:**
   ```bash
   docker push public.ecr.aws/<your-account-id>/nginx-html-docker:latest
   ```

---

### Step 7: Verify the Image on ECR

1. Log in to the AWS Management Console.
2. Navigate to **Amazon ECR** > **Public Repositories**.
3. Verify the `my-nginx-app` image is listed.
4. Share the repository URL, e.g., `public.ecr.aws/<your-account-id>/my-nginx-app:latest`.

---

## Conclusion

You have successfully Dockerized a simple HTML page using Nginx, tested it locally, and pushed the Docker image to Amazon ECR. This demonstrates your understanding of Docker, Nginx configuration, and containerization basics.

