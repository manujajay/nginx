# Nginx Configuration Guide

This README provides detailed instructions for setting up and configuring Nginx, including various scenarios like serving static content, using Nginx as a reverse proxy, and implementing load balancing.

## 1. Basic Setup

### Installation

On Ubuntu, you can install Nginx using the package manager:

```bash
sudo apt update
sudo apt install nginx
```

### Starting and Stopping Nginx

To start Nginx:

```bash
sudo systemctl start nginx
```

To stop Nginx:

```bash
sudo systemctl stop nginx
```

## 2. Serving Static Content

Here is a basic configuration for serving static content from `/var/www/html`.

### Sample Nginx Configuration

Create or edit the file `/etc/nginx/sites-available/default`:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        root /var/www/html;
        index index.html index.htm;
    }
}
```

## 3. Reverse Proxy Configuration

To set up Nginx as a reverse proxy for a Node.js application running on localhost:3000:

### Sample Nginx Configuration

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## 4. Load Balancing

Configure Nginx as a load balancer to distribute traffic among multiple backend servers:

### Sample Nginx Configuration

```nginx
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
    }
}
```

## 5. SSL/TLS Configuration

Setting up HTTPS in Nginx using SSL certificates:

### Sample Nginx Configuration

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/example.com.crt;
    ssl_certificate_key /etc/nginx/ssl/example.com.key;

    location / {
        root /var/www/html;
        index index.html index.htm;
    }
}
```

Ensure that you have your SSL certificates ready and adjust the paths in the configuration accordingly.

## Conclusion

This guide provides a basic overview of common tasks with Nginx, from simple web serving to more advanced configurations like reverse proxies and load balancing. Customize your setup based on your specific needs.
