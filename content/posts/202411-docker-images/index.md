---
date: '2025-11-26T16:12:08+05:00'
title: 'Creating Your First Docker Image: A Beginner-Friendly Guide'
draft: false
categories: ["Docker", "Container"]
tags: [ "Images"]
---
Docker has become one of the most essential tools for developers. It
allows you to package applications with all their dependencies into
portable containers. In this blog post, you will build your first Docker
image using the simplest possible example.

## What Is a Docker Image?

A Docker image is a read-only template that contains everything needed
to run an application: - Source code - Runtime - Dependencies - System
tools

When you run an image, Docker creates a container, which is a small
isolated environment.

## Step 1 - Create a Project Folder

Start by creating a folder for your project:

``` bash
mkdir myapp
cd myapp
```

## Step 2 - Add a Simple Python Script

Inside your myapp folder, create a file named app.py:

``` python
print("Hello from my Docker image!")
```

This small script will be what your container runs.

## Step 3 - Create Your Dockerfile

The Dockerfile tells Docker how to build your image. Create a file named
Dockerfile:

``` dockerfile
# Use a small official Python image
FROM python:3.10-slim

# Copy script into image
COPY app.py /app.py

# Run script on container start
CMD ["python", "app.py"]
```

## Step 4 - Build the Docker Image

Run this from inside your project folder:

``` bash
docker build -t my-first-image .
```

Docker will: 1. Download the base Python image 2. Add app.py into the
image 3. Set the startup command

If everything works correctly, you will have your first image.

## Step 5 - Run Your Docker Image

Start a container by running:

``` bash
docker run my-first-image
```

You should see:

    Hello from my Docker image!

Congratulations. You have built and run your first Docker image.

## What's Next?

Now that you understand the basic process, you can try: - Building
images for web servers - Using docker-compose - Optimizing images with
multi-stage builds - Publishing your image to Docker Hub

Docker is a powerful tool, and this is your starting point.
