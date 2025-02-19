# EX188 Red Hat Certified Specialist In Containers

> [! tip]
> **Max Mark : 300**
> **Clearing / Passing Mark : 210**
> **Duration** : **$2 \frac{1}{2} \text{Hrs.}$**

## Initial Instructions

### I. General 
1. In the exam they will provide you the initial credentials for logging into any device (Eg: flectrag1)
2. We have to use those credentials throughout the exam session unless otherwise it is specifically mentioned to do so to change.
3. Most activities are carried in exam base machine in the specified directory.
4. podman and podman-compose will be available in the exam.

## Question Outline

4. Running Simple Containers
5. Interacting with the Containers
6. Injecting Variables into Containers
7. Building Custom Container Image
8. Multi-container Deployment
9. Troubleshooting Multi-Container Stack

## Detailed Questions

### 1. Running Simple Containers:

**ACME** cooperation wants to demonstrate micro service for their software to use containers for their applications.

As an initial demonstration, run the container should use **nginx**

#### Task
1. Use the image **podman.io/library/nginx
2. The container must be named "**acme-demo-html**"
3. The container must run in **detached** mode
4. The container's port **80** must be exposed on local port **8001**
5. The container should map local file **/home/student/workspace/acme-nginx-html/index.html**

> [! important]
> **Note:** The Container serve at **/usr/share/nginx/html/index.html**

#### Testing your work

Once Container deployed it should be available on the following url : **http://localhost:8001**
Initial response should be : "Hello World NginX"

> [! warning]
> **"The locally mapped file system should reflect in container without need to redeploy it."**

#### Solution

```bash
podman run -d \
  --name acme-demo-html \
  -p 8001:80 \
  -v /home/student/workspace/acme-nginx-html/index.html:/usr/share/nginx/html/index.html:Z \
  podman.io/library/nginx

curl http://localhost:8001
```

---

### 2. Interacting with Containers

**ACME** cooperation want to know more about the container technology, how they make changes in live container running.

#### Task
1. Use the image **podman.io/library/nginx**
2. The container must be named as **acme-demo-nginx**
3. The container must run in **detached** from the command line
4. The container's port **80** must be exposed on local port **8002**
5. Once Container deployed and running it should copy the directory from the path **/home/student/workspace/acme-nginx-web/html** into the container at the path **/usr/share/nginx/html**
6. The container should utilize the configuration file available at the path **/home/student/workspace/acme-nginx-web/conf/nginx.conf**, config file for the container is located at **/etc/nginx/conf.d/default.conf**
7. After the container runs, execute **nginx -s reload** commands to reload the nginx

> [! success]
> **Note: ** Your Container should persist to run after configuration

#### Testing your work

Once the container is deployed it should be available on the following **URL**: **http:localhost:8002**

Response from the container should reflect the content of the **index.html** copied into it as **"Welcome to ACME"**

#### Solution

```bash
podman run -d \
  --name acme-demo-nginx \
  -p 8002:80 \
  -v /home/student/workspace/acme-nginx-web/html:/usr/share/nginx/html:Z \
  -v /home/student/workspace/acme-nginx-web/conf/nginx.conf:/etc/nginx/conf.d/default.conf:Z \
  podman.io/library/nginx

podman exec acme-demo-nginx nginx -s reload

curl http://localhost:8002
```

---

### 3. Injecting Variables into Containers

#### Task

1. Uses the image **quay.io/myacme/welcome**
2. There should be two containers named **acme_nginx_container_1** and acme_nginx_container_2
3. The **acme_nginx_container_1** should have environment variable as **RESPONSE**="**Welcome_ACME_Nginx_Container_1**"
4. The acme_nginx_container_2 should have environment variable as **RESPONSE**="**Welcome_ACME_Nginx_Container_2**"
5. The containers should run in the detached mode 
6. The container's port **8080** must be exposed on local port **8003**

> [!tip]
> **Both containers are meant to be co-exists but not running at the same time**

#### Testing your work

Once containers are deployed it should be available on the following URL: **http://localhost:8003**

When the **first** container is up, you should see the response as : **"Welcome_ACME_Nginx_Container_1"**

When the **second** container is up, you should see the response as : **"Welcome_ACME_Nginx_Container_2"**

#### Solution

```bash
podman create -d \
  --name acme_nginx_container_1 \
  -p 8003:8080 \
  -e RESPONSE="Welcome_ACME_Nginx_Container_1" \
  quay.io/myacme/welcome

podman create -d \
  --name acme_nginx_container_2 \
  -p 8003:8080 \
  -e RESPONSE="Welcome_ACME_Nginx_Container_2" \
  quay.io/myacme/welcome

podman start acme_nginx_container_1
curl http://localhost:8003  # Expected: "Welcome_ACME_Nginx_Container_1"
podman stop acme_nginx_container_1

podman start acme_nginx_container_2
curl http://localhost:8003  # Expected: "Welcome_ACME_Nginx_Container_2"
podman stop acme_nginx_container_2

--------------------------------------

podman pod create --name acme_pod -p 8003:8080

podman run -d --pod acme_pod --name acme_nginx_container_1 -e RESPONSE="Welcome_ACME_Nginx_Container_1" quay.io/myacme/welcome

podman run -d --pod acme_pod --name acme_nginx_container_2 -e RESPONSE="Welcome_ACME_Nginx_Container_2" quay.io/myacme/welcome
```

---

### 4. Building Custom Container Image

#### Task

##### acme-db

7. Build a custom image utilize the initial file available at the reference path **/home/student/workspace/acme-mariadb-containerfile**
8. Source for the container image is from **mariadb**
9. The container file should have build argument as below
		**ACME_MARIADB_DATABASE**
		**ACME_MARIADB_PASSWORD**
10. The container file should have environment variables as below
		**MARIADB_DATABASE**
		**MARIADB_ROOT_PASSWORD**
11. The environment variables should use the build arguments as below
		**MARIADB_DATABASE** uses **ACME_MARIADB_DATABASE**
		**MARIADB_ROOT_PASSWORD** uses **ACME_MARIADB_PASSWORD**
12. Use the build argument should fetch it from user at build
		**ACME_MARIADB_DATABASE/acme**
		**ACME_MARIADB_PASSWORD/acme**
13. The image must be locally available and tag as **acme:5000/acme-mariadb:latest**
14. Push the image to registry as **acme:5000/acme-mariadb:latest**

##### acme-db-exporter:

15. Build a custom image as per the requirement below, utilize the initial file available at the reference path **/home/student/workspace/acme-mariadb-db/acme-db-export-containerfile**
16. Source for the Container image is from **mariadb**
17. Copy the file from **scripts/export.sh** in to **/scripts** in container
18. Working directory is **/scripts**
19. Run **export.sh** script
20. The image must be locally available and tag as **acme-mariadb-export:latest**.
21. Push the image to registry as **acme:5000/acme-mariadb-export:latest**

#### Testing your work

**Run a test container named acme-mariadb-test using the container image available at acme:5000/acme-mariadb:latest**

**Test container should be in running state once initialized and persist to run**.

Run a test container named **acme-mariadb-exporter** using the container image available at **acme:5000/acme-mariadb-export:latest** using the local map from path **/home/student/workspace/acme-mariadb-db/export** to **/home** in container

Once container execution is done, you should be able to read the **mysql dump file** at the following location **/home/student/workspace/acme-mariadb-db/export/acmeMpsql.sql**. You can be able to read the **mysql dump with any reader**.
#### Solution

```dockerfile
#### acme-db
# Use official MariaDB as base image
FROM mariadb:latest

# Define build arguments
ARG ACME_MARIADB_DATABASE
ARG ACME_MARIADB_PASSWORD

# Set environment variables using build arguments
ENV MARIADB_DATABASE=$ACME_MARIADB_DATABASE
ENV MARIADB_ROOT_PASSWORD=$ACME_MARIADB_PASSWORD

# Expose default MySQL port
EXPOSE 3306

# Default command to run MariaDB
CMD ["mariadbd"]
----------------------------------------
#### acme-db-exporter
# Use official MariaDB as base image
FROM mariadb:latest

# Set working directory
WORKDIR /scripts

# Copy export script into the container
COPY scripts/export.sh /scripts/

# Grant execution permission to the script
RUN chmod +x /scripts/export.sh

# Run export script on container startup
CMD ["sh", "/scripts/export.sh"]
```

```bash
# export.sh
#!/bin/sh
# MySQL Export Script

echo "Starting MySQL export..."
mysqldump -u root -p"$MARIADB_ROOT_PASSWORD" "$MARIADB_DATABASE" > /home/acmeMpsql.sql

echo "Export completed. File saved at /home/acmeMpsql.sql"
```

```bash
#Create `acme-db` Image
podman build \
  --file /home/student/workspace/acme-mariadb-containerfile \
  --build-arg ACME_MARIADB_DATABASE=acme \
  --build-arg ACME_MARIADB_PASSWORD=acme \
  -t acme-mariadb:latest

#Push `acme-db` Image to Registry
podman tag acme-mariadb:latest acme:5000/acme-mariadb:latest
podman push acme:5000/acme-mariadb:latest

-----------------

#Create `acme-db-exporter` Image
podman build \
  --file /home/student/workspace/acme-mariadb-db/acme-db-export-containerfile \
  --build-arg ACME_MARIADB_DATABASE=acme \
  --build-arg ACME_MARIADB_PASSWORD=acme \
  -t acme-mariadb-export:latest

#Push `acme-db-exporter` Image to Registry
podman tag acme-mariadb-export:latest acme:5000/acme-mariadb-export:latest
podman push acme:5000/acme-mariadb-export:latest

podman run -d \
  --name acme-mariadb-test \
  acme:5000/acme-mariadb:latest

----------------------

#Ensure the container is **running persistently
podman ps -a | grep acme-mariadb-test

#Test `acme-db-exporter` Image
podman run --rm \
  --name acme-mariadb-exporter \
  -v /home/student/workspace/acme-mariadb-db/export:/home \
  acme:5000/acme-mariadb-export:latest

#Check the exported SQL dump file
ls -lh /home/student/workspace/acme-mariadb-db/export/acmeMpsql.sql
cat /home/student/workspace/acme-mariadb-db/export/acmeMpsql.sql
```

---

### 5. Multi-Container Stack

Deploy a multi-container stack for **WordPress** with the following specification

22. Create a network called **acme-wp-net**
23. Create a volume named **acme-wp-backend**
24. Create another volume named **acme-wp-app**
25. Create another volume named **acme_wordpress_data**

###### acme-wp-db

1. Uses the image **quay.io/myacme/mariadb:latest**
2. The container must be named **mariadb**
3. The container should be part of the **acme-wp-net** network
4. Map the volume **acme-wp-backend** to the container at the /bitnami/mariadb
5. Use environment variables as **MARIADB_USER=acme_wordpress MARIADB_PASSWORD=acme MARIADB_DATABASE=acme_wordpress MARIADB_ROOT_PASSWORD=acme**
6. **Display the container to run persistently**
###### acme-nginx

1. Uses the image **quay.io/myacme/nginx**
2. The container must be named **acme-wp-app**
3. Add the container to **acme-wp-app** to **/etc/nginx** inside the container
4. The container's port **8080** must be export on local port **8080**

##### acme-wp-nginx

1. Uses the image **quay.io/myacme/wordpress:latest**
2. The container must be named **acme-wordpress**
3. Add the container to **acme-wp-net** network
4. Map the volume **acme_wordpress_data** to the container at the **/bitnami/wordpress**
5. The container's port **8080** must be exposed on local port **8004**
6. The container's port **8443** must be exposed on local port **8443**
7. Use Environment variables as **WORDPRESS_DATABASE_USER=acme_wordpress WORDPRESS_DATABASE_PASSWORD=acme WORDPRESS_DATABASE_NAME=acme_wordpress**
8. **Deploy the container to run persistently.**

> [! important]
> **Note: ** Your container should persist to run after configuration.

#### Testing your network

Once container deployed it should be available on the following URL: **http://localhost:8443**
#### Solution

```bash
#Create a Network
podman network create acme-wp-net

#Create Volumes
podman volume create acme-wp-backend
podman volume create acme-wp-app
podman volume create acme_wordpress_data

#Deploy MariaDB Container
podman run -d \
  --name mariadb \
  --network acme-wp-net \
  -v acme-wp-backend:/bitnami/mariadb \
  -e MARIADB_USER=acme_wordpress \
  -e MARIADB_PASSWORD=acme \
  -e MARIADB_DATABASE=acme_wordpress \
  -e MARIADB_ROOT_PASSWORD=acme \
  quay.io/myacme/mariadb:latest

#Deploy Nginx Container
podman run -d \
  --name acme-wp-app \
  --network acme-wp-net \
  -v acme-wp-app:/etc/nginx \
  -p 8080:8080 \
  quay.io/myacme/nginx

#Deploy WordPress Container
podman run -d \
  --name acme-wordpress \
  --network acme-wp-net \
  -v acme_wordpress_data:/bitnami/wordpress \
  -e WORDPRESS_DATABASE_USER=acme_wordpress \
  -e WORDPRESS_DATABASE_PASSWORD=acme \
  -e WORDPRESS_DATABASE_NAME=acme_wordpress \
  -p 8004:8080 \
  -p 8443:8443 \
  quay.io/myacme/wordpress:latest

#Check if the containers are running
podman ps

#Test crawling
curl http://localhost:8004
curl https://localhost:8443

#Check logs for a specific container
podman logs mariadb
podman logs acme-wp-app
podman logs acme-wordpress

#Restart a container if needed
podman restart mariadb acme-wp-app acme-wordpress

#Check the created network
podman network inspect acme-wp-net
```

---

### 6. Troubleshooting Multi-Container Stack

#### **Configuration**

### **Network and Volume Setup**

- Create a network called **`acme-troubleshoot`**.
- Create a volume named **`acme-wp-backend-ts`**.

### **Database Container: `acme-wp-db-ts`**

- Use the image: `quay.io/myacme/mariadb:latest`.
- Container name: **`mariadb-ts`**.
- Attach the container to the **`acme-troubleshoot`** network.
- **(Ignore for now, but required in the exam)** Map the volume `acme-wp-backend-ts` to `/usr/share/mysql` inside the container.

### **Nginx Container: `acme-nginx-ts`**

- Use the image: `quay.io/myacme/nginx`.
- Container name: **`acme-nginx-ts`**.
- Attach the container to the **`acme-troubleshoot`** network.

### **WordPress Container: `acme-wp-nginx`**

- Container name: **`acme-wordpress-ts`**.
- Use the image: `quay.io/myacme/wordpress:latest`.
- Attach the container to the **`acme-troubleshoot`** network.
- Deploy the container to **run persistently**.

## **Testing Your Work**

- Once deployed, ensure the containers are accessible and running.
- Verify the setup using:
    
    ```bash
    podman ps
    podman network inspect acme-troubleshoot
    podman logs mariadb-ts
    podman logs acme-nginx-ts
    podman logs acme-wordpress-ts
    ```

#### Solution

```bash
podman network create acme-troubleshoot

podman volume create acme-wp-backend-ts

podman run -d \
  --name mariadb-ts \
  --network acme-troubleshoot \
  -e MARIADB_DATABASE=acme_wordpress \
  -e MARIADB_USER=acme_wordpress \
  -e MARIADB_PASSWORD=acme \
  -e MARIADB_ROOT_PASSWORD=acme \
  quay.io/myacme/mariadb:latest

podman run -d \
  --name acme-nginx-ts \
  --network acme-troubleshoot \
  quay.io/myacme/nginx

podman run -d \
  --name acme-wordpress-ts \
  --network acme-troubleshoot \
  -e WORDPRESS_DATABASE_USER=acme_wordpress \
  -e WORDPRESS_DATABASE_PASSWORD=acme \
  -e WORDPRESS_DATABASE_NAME=acme_wordpress \
  quay.io/myacme/wordpress:latest

podman ps

podman network inspect acme-troubleshoot

podman logs mariadb-ts
podman logs acme-nginx-ts
podman logs acme-wordpress-ts

podman restart <container_name>
```