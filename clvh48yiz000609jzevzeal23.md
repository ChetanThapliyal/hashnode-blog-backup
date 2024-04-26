---
title: "Optimizing Docker Images with Multistage Builds: A Hugo Portfolio Example"
seoTitle: "distroless docker build with hugo"
seoDescription: "Optimizing Docker Images with Multistage Builds: A Hugo Portfolio Example"
datePublished: Fri Apr 26 2024 20:19:19 GMT+0000 (Coordinated Universal Time)
cuid: clvh48yiz000609jzevzeal23
slug: optimizing-docker-images-with-multistage-builds-a-hugo-portfolio-example
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714155257973/f3bb0b2f-96d7-4988-bec6-b630dfa4cebc.png
tags: docker, hugo, distroless, dockerprojects, multistage-docker-build

---

Docker has revolutionized the way we build, package, and deploy applications. However, as projects grow in complexity, the size of Docker images can quickly balloon, leading to slower build times, increased storage requirements, and reduced performance. This is where multistage builds come into play.

Docker images are templates that house the instructions for the associated container. These images are built in read-only layers, and each layer represents a step in the build process. Optimizing these layers is crucial for creating lean and efficient Docker images.

One powerful technique for optimizing Docker images is the use of multistage builds. Multistage builds allow you to separate the build process from the final image, resulting in a smaller and more streamlined container.

Let's explore this concept in detail and use a real-world example of a portfolio website bui[l](https://medium.com/@navneetskahlon/optimizing-docker-images-best-practices-and...)t with Hugo to illustrate the process.

## **Understanding Docker Images and Layers**

Before diving into multistage builds, let's briefly discuss what Docker image is and how Docker images work.

### What is Docker Image?

A Docker [i](https://medium.com/@navneetskahlon/optimizing-docker-images-best-practices-and...)mage is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712061549886/fa4e7f71-0d1d-484e-b7c6-bbae61b71a6b.png align="center")

A Docker image is built using a series of instructions defined in a `Dockerfile`. Each instruction creates a new layer in the image, and these layers are stacked on top of each other. Images contain the code or binary, runtimes, dependencies, and other filesystem objects to run an application. The image relies on the host operating system (OS) kernel.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712061291623/cdc5c538-ce2a-4d90-abbe-81b2a5eb4ac6.png align="center")

Docker images are designed to be immutable, meaning that once an image is built, it cannot be changed. Any updates or modifications to the software or environment must be made by creating a new image, rather than modifying the existing one. This makes it easy to roll back to a previous version of the image if necessary, and to deploy the same image consistently across different environments.

When you make changes to your application and rebuild the image, only the modified layers are rebuilt, while the unchanged layers are cached. This layered approach allows for efficient image building and sharing.

There are three specific types of commands that can be used to build and run Dockerfiles:

1. **RUN** - To run this command you will need a separate new layer and this command is mainly used to build images and install packages and applications.
    
2. **CMD** - The CMD describes the default container parameters or commands. The user can easily override the default command when you use this.
    
3. **ENTRYPOINT** - A container with an ENTRYPOINT is preferred when you want to define an executable. You can only override it if you use the --entrypoint flag.
    

## **The Problem with Monolithic Dockerfiles**

Traditionally, Dockerfiles are written as a single, monolithic set of instructions. This approach works well for simple projects, but as the complexity grows, it can lead to several issues:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1712310721590/e91b61ea-efbf-43af-880f-65878fd90ba5.png align="center")

### Why **the image is so Large?**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714155961952/a03eeeec-c79d-4c27-9658-cfca99b6f1a5.png align="center")

1. **Large Image Sizes:** Including all the dependencies, build tools, and artifacts in a single image result in a large image size.
    
2. **Slow Build Times:** Rebuilding the entire image from scratch every time a change is made can be time-consuming.
    
3. **Security Risks:** Unnecessary tools and libraries included in the final image can introduce potential security vulnerabilities.
    

The solution that Docker has for this problem is a multi-stage image build.

## **Introducing Multistage Builds (Distroless)**

The key to minimizing the size of Docker image is to reduce the number of layers. Multistage builds enable us to achieve this by separating the build process from the final image.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714155734992/deca91ce-54bd-458b-aa22-95959aee4852.png align="center")

Here's how it works:

1. **Build Stage**: In the first stage, you build your application, including all the necessary dependencies and tooling. This stage can be quite large, as it includes the entire build process.
    
2. **Final Stage**: In the second stage, you copy only the necessary artifacts from the build stage and create a smaller, more streamlined image.
    

By separating these stages, we can ensure that final Docker image only contains the essential components required to run your application, leaving behind the bulky build dependencies.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714156351048/e54505fe-d544-4a35-b797-1e55df7a4df6.png align="center")

It involves downloading a new image after initial build is complete, and copying all application files, assets, and library dependencies.

## The Project: Portfolio Website with Hugo

Hugo is a fast and modern static site generator written in Go, and it's designed to make website creation simple and efficient. Static site generators like Hugo are an excellent match for Docker, as they produce a set of static files that are easy to serve with a lightweight HTTP server.

## The Challenge: Image Size Reduction

The challenge presented was to optimize a Docker image for a portfolio website created with Hugo. The initial Docker image size was `434MB`, which is quite large for a simple portfolio website. The goal is to reduce the image size without sacrificing functionality.

To tackle this challenge, two Dockerfiles were created: one for development (`dev`) and one for production (`prod`). The production Dockerfile, on the other hand, was a Multistage build designed for image optimization.

#### Development Dockerfile

The development Dockerfile is a standard build, containing all the necessary tools and dependencies for a development environment. The development Dockerfile included everything needed to build the Hugo site, including the full Hugo binary, a web server for testing, and various other dependencies.

```dockerfile
FROM golang:alpine

# Set the working directory
WORKDIR /app

# Copy your Hugo project files
COPY . .

# Install Hugo and compatibility libraries
RUN apk add --no-cache hugo libc6-compat libgcc libstdc++

# Install Node.js and npm
RUN apk add --no-cache nodejs npm

# Install PostCSS for Hugo
RUN npm install -g postcss-cli

# Generate your website's static content
RUN hugo

# Install NGINX
RUN apk add --no-cache nginx

# Optional: Copy your custom Nginx configuration if needed
# COPY nginx.conf /etc/nginx/nginx.conf

# Remove Hugo and any build dependencies (optional, for size reduction)
RUN apk del --no-cache hugo

# Expose the port NGINX uses
EXPOSE 80

# Command to start NGINX when the container runs
CMD ["nginx", "-g", "daemon off;"]
```

#### Explanation:

1. **Base Image**: The Dockerfile starts by using the `golang:alpine` base image, which is a minimal and lightweight version of the Go programming language runtime environment based on the Alpine Linux distribution.
    
2. **Working Directory**: The `WORKDIR /app` command sets the working directory inside the container to `/app`, which is where the project files will be copied.
    
3. **Copying Project Files**: The `COPY . .` command copies all the files and directories from the current context (your local machine) to the `/app` directory inside the container.
    
4. **Installing Hugo and Dependencies**: The `RUN apk add --no-cache hugo libc6-compat libgcc libstdc++` command installs the Hugo static site generator and its required compatibility libraries on the Alpine Linux system.
    
5. **Installing Node.js and npm**: The `RUN apk add --no-cache nodejs npm` command installs Node.js and npm, which are required for the PostCSS installation in the next step.
    
6. **Installing PostCSS**: The `RUN npm install -g postcss-cli` command installs the PostCSS command-line interface globally, which is used for processing CSS files.
    
7. **Generating Static Content**: The `RUN hugo` command generates the static content of the Hugo website.
    
8. **Installing NGINX**: The `RUN apk add --no-cache nginx` command installs the NGINX web server, which will be used to serve the generated static content.
    
9. **Removing Hugo and Build Dependencies**: The `RUN apk del --no-cache hugo` command removes the Hugo package and any build dependencies to reduce the overall size of the final Docker image.
    
10. **Exposing Port**: The `EXPOSE 80` command exposes port 80 on the container, which is the default port for the NGINX web server.
    
11. **Starting NGINX**: The `CMD ["nginx", "-g", "daemon off;"]` command specifies the command to be executed when the container starts, which is to run the NGINX web server in the foreground.
    

This Dockerfile sets up a complete environment for building and serving a Hugo-based website using the Go programming language and the Alpine Linux distribution. It installs the necessary dependencies, generates the static content, and sets up NGINX to serve the website.

* To build image run:
    
    ```bash
    docker build -f Dockerfile.dev -t my-hugo-site:dev .
    docker images
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714160456092/7560b7a4-3eab-4cac-ae81-b205d1c213b5.png align="center")

### The Solution: Implementing Multistage Dockerfiles

#### Production Dockerfile: Multistage Build

The production Dockerfile was where the magic happened. It was divided into multiple stages:

1. **Build Stage**: This stage installed Hugo and built the static website.
    
2. **Optimization Stage**: Here, unnecessary files and dependencies were removed.
    
3. **Final Stage**: The last stage copied only the necessary artifacts from the previous stages, resulting in a much lighter image.
    

```dockerfile
# Stage 1: Build Stage
FROM golang:alpine AS build

# Set the working directory within the container
WORKDIR /app

# Copy your Hugo project files
COPY . . 

# Install Hugo
RUN apk add --no-cache hugo

# Install Node.js and npm (npm includes npx)
RUN apk add --no-cache nodejs npm

# Install PostCSS
RUN npm install -g postcss-cli

# Generate your website's static content 
RUN hugo

# Stage 2: Web Server Stage
FROM nginx:alpine

# Set NGINX's default serving directory 
WORKDIR /usr/share/nginx/html

# Copy the generated static website from the build stage
COPY --from=build /app/public /usr/share/nginx/html

# Optional: Copy your custom Nginx configuration if needed
# COPY nginx.conf /etc/nginx/nginx.conf

# Expose the port NGINX uses
EXPOSE 8080

# Command to start NGINX when the container runs
CMD ["nginx", "-g", "daemon off;"] 

#env var
ENV NODE_ENV=production \
HOSTNAME="0.0.0.0" \
NEXT_TELEMETRY_DISABLED=1
```

#### Explanation:

This Prod Dockerfile uses a multi-stage build approach to create a Docker image for a Hugo-based website. Let's break it down step by step:

**Stage 1: Build Stage**

1. **Base Image**: The first stage uses the `golang:alpine` base image, which is a minimal and lightweight version of the Go programming language runtime environment based on the Alpine Linux distribution.
    
2. **Working Directory**: The `WORKDIR /app` command sets the working directory inside the container to `/app`, where the project files will be copied.
    
3. **Copying Project Files**: The `COPY . .` command copies all the files and directories from the current context (your local machine) to the `/app` directory inside the container.
    
4. **Installing Hugo**: The `RUN apk add --no-cache hugo` command installs the Hugo static site generator on the Alpine Linux system.
    
5. **Installing Node.js and npm**: The `RUN apk add --no-cache nodejs npm` command installs Node.js and npm, which are required for the PostCSS installation in the next step.
    
6. **Installing PostCSS**: The `RUN npm install -g postcss-cli` command installs the PostCSS command-line interface, which is used for processing CSS files.
    
7. **Generating Static Content**: The `RUN hugo` command generates the static content of the Hugo website.
    

**Stage 2: Web Server Stage**

1. **Base Image**: The second stage uses the `nginx:alpine` base image, which is a minimal and lightweight version of the NGINX web server based on the Alpine Linux distribution.
    
2. **Working Directory**: The `WORKDIR /usr/share/nginx/html` command sets the working directory inside the container to the default NGINX serving directory.
    
3. **Copying Static Content**: The `COPY --from=build /app/public /usr/share/nginx/html` command copies the generated static content from the previous build stage to the NGINX serving directory.
    
4. **Exposing Port**: The `EXPOSE 8080` command exposes port 8080 on the container, which is the default port for the NGINX web server.
    
5. **Starting NGINX**: The `CMD ["nginx", "-g", "daemon off;"]` command specifies the command to be executed when the container starts, which is to run the NGINX web server in the foreground.
    
6. **Environment Variables**: The `ENV` command sets three environment variables:
    
    * `NODE_ENV=production`: Sets the Node.js environment to production mode.
        
    * `HOSTNAME="0.0.0.0"`: Sets the hostname to "0.0.0.0".
        
    * `NEXT_TELEMETRY_DISABLED=1`: Disables Next.js telemetry.
        

This Dockerfile uses a multi-stage build approach to create a Docker image for a Hugo-based website. The first stage builds the static content using Go, Hugo, and PostCSS, while the second stage sets up the NGINX web server to serve the generated content. The resulting image is a lightweight and production-ready container for the Hugo website.

* To build image run:
    
    ```bash
    docker build -f Dockerfile.dev -t my-hugo-site:dev .
    docker images
    ```
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714161910427/d26cf370-0cb6-43cf-9d8a-62814e2ab0a9.png align="center")

### The Results: A Dramatic Reduction

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1714162068962/ecafac7e-32a7-470f-94da-cae1926d1ecb.png align="center")

By implementing Multistage builds, the final production image size was reduced from `434MB` to an impressive `61MB`. This not only makes the image easier to handle and quicker to deploy but also reduces the attack surface for potential security vulnerabilities.

### How You Can Achieve Similar Results

To achieve similar results, consider the following steps:

1. **Analyze Your Current Dockerfile**: Understand what is necessary for your application to run.
    
2. **Implement Multistage Builds**: Use separate stages for building and optimizing your Docker image.
    
3. **Optimize Your Artifacts**: Be selective about what you copy into your final image.
    
4. **Iterate and Test**: Continuously test your Docker images to ensure functionality while optimizing.
    

### Conclusion

Docker image optimization is both an art and a science. By leveraging Multistage builds and being mindful of the artifacts included in the final image, significant efficiencies can be realized. This case study with a Hugo-based portfolio website demonstrates the potential for optimization, reducing the image size by nearly 90% without compromising on functionality.

Remember, the key to optimization is understanding your application's requirements and continuously iterating on your Dockerfile to find the perfect balance between functionality and efficiency. Happy Dockering!