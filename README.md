# deploy-war-readme
## Guide: Hosting and Running .war File on Windows with NGINX Reverse Proxy

This guide provides detailed instructions on how to host and run a `.war` file on a Windows machine using NGINX as a reverse proxy to route traffic from a specific domain to your application running on `localhost:8081`. The guide assumes you are using Java version 11 and MySQL Server (free community edition or paid version) as prerequisites.

## Prerequisites

- Windows operating system
- Java Development Kit (JDK) 11 installed
- MySQL Server (free community edition or paid version) installed
- NGINX installed

## Step 1: Installing Java Development Kit (JDK) 11

1. Visit the [Oracle JDK 11 download page](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) (or an alternative source) and download the appropriate JDK for your Windows version.

2. Run the installer and follow the on-screen instructions to complete the installation.

3. Verify the installation by opening a Command Prompt and running:

   ```sh
   java -version
   ```

   You should see the installed JDK 11 version information.

## Step 2: Installing MySQL Server

1. Visit the [MySQL Community Downloads page](https://dev.mysql.com/downloads/mysql/) and choose the MySQL Server version that suits your needs (free community edition or paid version).

2. Run the installer and follow the on-screen instructions to complete the installation. Remember the password you set for the root user.

3. Start the MySQL Server service and ensure it's running.

## Step 3: Installing NGINX

1. Download the latest version of NGINX for Windows from the [official NGINX website](https://nginx.org/en/download.html).

2. Extract the downloaded archive to a location on your machine, e.g., `C:\nginx`.

3. Open a Command Prompt in Administrator mode and navigate to the NGINX installation directory:

   ```sh
   cd C:\nginx
   ```

4. Start NGINX by running:

   ```sh
   nginx
   ```

   NGINX should now be running and listening on port 80 by default.

## Step 4: Deploying Your .war File

1. Place your `.war` file in a convenient directory on your system.

2. Open a Command Prompt and navigate to the directory containing your `.war` file.

3. Deploy the `.war` file using Java's `java -jar` command:

   ```sh
   java -jar your-application.war
   ```

   Your application should now be running on `localhost:8081`.

## Step 5: Configuring NGINX Reverse Proxy

1. Navigate to the NGINX configuration directory, typically located at `C:\nginx\conf`.

2. Open the `nginx.conf` file in a text editor.

3. Inside the `http` block, add a new `server` block to configure the reverse proxy :
  ```sh
   server {
       listen 80;
       server_name your-domain.com;  # Replace with your actual domain or IP
   
       location / {
           proxy_pass http://localhost:8081;  # Points to your application's local address
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
 ```

4. Save the changes to nginx.conf.

5. Test the configuration for syntax errors:
    ```sh
   nginx -t
    ```

6. If the configuration is valid, reload NGINX to apply the changes:
    ```sh
    nginx -s reload
    ```

## Step 7: Accessing Your Application

1. Edit your system's `hosts` file (`C:\Windows\System32\drivers\etc\hosts`) and add an entry to map your domain to `localhost`:

   ```
   127.0.0.1    your-domain.com
   ```

2. Open a web browser and navigate to `http://your-domain.com`. NGINX will route the traffic to your application running on `localhost:8081`.

Congratulations! You've successfully hosted and configured a `.war` file on a Windows machine using NGINX as a reverse proxy. Your application should now be accessible via your configured domain.

2. Open a web browser and navigate to `http://your-domain.com`. NGINX will route the traffic to your application running on `localhost:8081`.

Congratulations! You've successfully hosted and configured a `.war` file on a Windows machine using NGINX as a reverse proxy. Your application should now be accessible via your configured domain.
