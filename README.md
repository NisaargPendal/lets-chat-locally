# Let's Chat Docker Deployment

A simple, self-hosted chat application where users can join a room by simply entering a name—no authentication required. This deployment uses Docker Compose to set up two services:

**MongoDB**: For storing chat data.
**Let's Chat**: The chat application itself.


## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Customization](#customization)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

**Let's Chat** is a lightweight chat application designed for small teams or personal projects. The Docker-based deployment allows you to have a working chat room quickly without complex authentication or configuration steps.

## Features

- **Simple UI**: Just enter your name and start chatting.
- **Self-Hosted**: Maintain control over your data.
- **Dockerized Setup**: Easy installation using Docker Compose.
- **Persistent Data**: MongoDB stores chat history in a mapped volume.

## Requirements

- Docker Engine (v18.09+ recommended)
- Docker Compose (v1.25+ recommended)
- Basic command-line knowledge

## Installation & Setup

1. **Clone or Download the Repository**

   Clone the repository to your local machine or download the source files.

   ```bash
   git clone https://github.com/NisaargPendal/lets-chat-locally.git
   cd lets-chat-locally
   ```

2. **Prepare the Environment**

   Create a directory for MongoDB data if it doesn’t already exist:

   ```bash
   sudo su
   mkdir -p ./mongo-data
   ```

3. **Docker Compose File**

   The provided `docker-compose.yml` sets up two services:

   ```yaml
   version: '3.8'

   services:
     mongo:
       image: mongo:4.0
       container_name: letschat-mongo
       restart: unless-stopped
       volumes:
         - ./mongo-data:/data/db

     letschat:
       image: sdelements/lets-chat:latest
       container_name: letschat
       restart: unless-stopped
       environment:
         - MONGO_URL=mongodb://mongo:27017/letschat
         - PORT=5000
       ports:
         - "5000:5000"
       depends_on:
         - mongo
   ```

4. **Start the Stack**

   Launch the services with Docker Compose:

   ```bash
   docker-compose up -d
   ```

5. **Verify Deployment**

   Check that the containers are running:

   ```bash
   docker-compose ps
   ```

## Usage

1. **Access the Chat Application**

   Open your browser and navigate to:

   ```
   http://localhost:5000
   ```

2. **Start Chatting**

   Enter your name in the chat interface and join the chat room. No further setup or authentication is required.

## Customization

- **Environment Variables**

  You can adjust the following environment variables in the `docker-compose.yml`:

  - `MONGO_URL`: Change if you wish to point to a different MongoDB instance.
  - `PORT`: Change the port on which Let's Chat will run.

- **Volumes**

  - MongoDB data is stored in `./mongo-data`. To preserve chat history across container restarts, do not remove this folder.

- **Advanced Configuration**

  For additional customization, refer to the [Let's Chat documentation](https://github.com/sdelements/lets-chat) on GitHub.

## Troubleshooting

- **Containers Not Running**

  - Check logs for the MongoDB container:
    ```bash
    docker-compose logs mongo
    ```
  - Check logs for the Let's Chat container:
    ```bash
    docker-compose logs letschat
    ```

- **Port Conflicts**

  Ensure port `5000` is free or adjust the port mapping in the `docker-compose.yml`.

- **Data Persistence Issues**

  Make sure the `./mongo-data` directory has the appropriate permissions:
  
  ```bash
  sudo chown -R $USER:$USER ./mongo-data
  ```

## License

This project is licensed under the [MIT License](LICENSE). See the LICENSE file for details.

---
