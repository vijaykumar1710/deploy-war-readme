# deploy-war-readme
## Guide: Hosting and Running .war File on Windows with NGINX Reverse Proxy

This guide provides detailed instructions on how to host and run a `.war` file on a Windows machine using NGINX as a reverse proxy to route traffic from a specific domain to your application running on `localhost:8081`. The guide assumes you are using Java version 11 and postgres sql
## Prerequisites

- Windows operating system
- Java Development Kit (JDK) 11 installed
- postgres sql / MySQL Server (free community edition or paid version) installed ( recommend use postgres )
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

## Step 4: Installing postgres

1. Download postgress based on your OS : https://www.postgresql.org/download/
2. Follow the installer screen instructions and complete the installation
3. Start the postgres Server instance and ensure it's running.

   Using PgAdmin application, You can create server, database , modify , run the queries etc just like mysql workbench

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

## Step 6: Setting Up Port Forwarding

### On Your Windows Firewall

1. Search for "Windows Defender Firewall" in the Windows search bar and open it.

2. Click on "Advanced settings" on the left sidebar.

3. In the "Inbound Rules" section, click "New Rule..." on the right sidebar.

4. Choose the "Port" option and click "Next."

5. Select "TCP" and enter `8081` as the specific local ports. Click "Next."

6. Choose "Allow the connection" and click "Next."

7. Provide a name for the rule, e.g., "Port Forwarding for My Application." Click "Finish" to create the rule.

### On Your Network Router

1. Open a web browser and enter your router's IP address in the address bar. This is usually something like `192.168.1.1`.

2. Log in to your router using the administrator credentials.

3. Look for a section related to "Port Forwarding" or "Virtual Server." The wording may differ depending on your router.

4. Add a new port forwarding rule:
   - External Port: Choose a port number (e.g., `8080`) that external users will use to access your application.
   - Internal IP Address: Enter the internal IP address of your Windows machine (e.g., `192.168.1.100`).
   - Internal Port: Enter `8081`, the port your application is running on.
   - Protocol: Select "TCP."

5. Save or apply the changes. Your router will now forward external requests on the chosen port to your Windows machine.

Please note that the process for setting up port forwarding can vary based on the router's make and model. Refer to your router's documentation for specific instructions.

## Step 7: Accessing Your Application

1. Edit your system's `hosts` file (`C:\Windows\System32\drivers\etc\hosts`) and add an entry to map your domain to `localhost`:

   ```
   127.0.0.1    your-domain.com
   ```

2. Open a web browser and navigate to `http://your-domain.com`. NGINX will route the traffic to your application running on `localhost:8081`.

Congratulations! You've successfully hosted and configured a `.war` file on a Windows machine using NGINX as a reverse proxy. Your application should now be accessible via your configured domain.

2. Open a web browser and navigate to `http://your-domain.com`. NGINX will route the traffic to your application running on `localhost:8081`.

Congratulations! You've successfully hosted and configured a `.war` file on a Windows machine using NGINX as a reverse proxy. Your application should now be accessible via your configured domain.
