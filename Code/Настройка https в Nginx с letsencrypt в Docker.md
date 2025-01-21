---
tags:
  - docker
  - nginx
  - https
  - letsencrypt
---


<span style="font-size:12px; color:#888888;">Created: 21.01.2025 14:59</span>

Installation of Let's Encrypt certificates on a dockerized Nginx deployment involves:

- Creating a Docker Compose file.
- Adjusting the Nginx server configuration.
- Running the Certbot client.

The steps below describe the most straightforward method to obtain Let's Encrypt certificates.

### Step 1: Create Directory

Create a project directory in which to store the Docker Compose file. Use the [cd command](https://phoenixnap.com/kb/linux-cd-command) to navigate to the newly created directory. Execute both commands on a single line:

```
sudo mkdir letsencrypt && cd letsencrypt
```

### Step 2: Create Docker Compose File

Docker Compose is a tool for creating and running multi-container Docker applications. The _docker-compose.yml_ file defines and configures the containers participating in the deployment.

Create the file with a [text editor](https://phoenixnap.com/kb/best-linux-text-editors-for-coding) such as [Nano](https://phoenixnap.com/kb/use-nano-text-editor-commands-linux):

```
nano docker-compose.yml
```

Next, paste the following code into the file:

```
version: '3'

services:
  webserver:
    image: nginx:latest
    ports:
      - 80:80
      - 443:443
    restart: always
    volumes:
      - ./nginx/conf/:/etc/nginx/conf.d/:ro
      - ./certbot/www/:/var/www/certbot/:ro
  certbot:
    image: certbot/certbot:latest
    volumes:
      - ./certbot/www/:/var/www/certbot/:rw
      - ./certbot/conf/:/etc/letsencrypt/:rw
```

![Editing docker-compose.yml file.](https://phoenixnap.com/kb/wp-content/uploads/2023/09/editing-docker-compose-yml-letsencrypt-nginx-docker.png)

**Save the file** and exit.

The code defines two containers (_webserver_ and _certbot_) and connects them by mapping them to the _/var/www/certbot/_ directory. It also provides [read and write permissions](https://phoenixnap.com/kb/linux-file-permissions) for the _certbot_ container to allow Certbot to create certificates.

### Step 3: Create Configuration File

Before applying the Docker Compose file, configure the Nginx server to allow Certbot to access the files it needs. To achieve this, create a configuration file:

```
sudo nano /etc/nginx/conf.d/app.conf
```

Copy and paste the code below, replacing **`[domain-name]`** with your actual domain name:

```
server {
    listen 80;
    listen [::]:80;

    server_name [domain-name] www.[domain-name];
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://[domain-name]$request_uri;
    }
}
```

![Creating Nginx server configuration file.](https://phoenixnap.com/kb/wp-content/uploads/2023/09/editing-etc-nginx-conf-d-app-conf-letsencrypt-nginx-docker.png)

**Save the file** and exit.

The first **`location`** block serves the files necessary for Certbot to authenticate the server and create the certificate. The second **`location`** block sends the rest of the port **80** [HTTP traffic to HTTPS](https://phoenixnap.com/kb/redirect-http-to-https-nginx).

**Note**: The current setup would receive error **301** because HTTPS is not defined in Nginx. In a later step, this traffic will be redirected to port **443** (HTTPS).

### Step 4: Run Certbot

With the necessary configuration in place, apply the Docker Compose file with the **`docker-compose run`** command. Since Let's Encrypt limits the amount of available free certificates per month, test the command in a dry run first:

```
docker-compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d [domain-name]
```

When prompted, enter your email for notices from Let's Encrypt. This step is optional, and you can skip it by typing `c` and pressing **Enter**.

![Running the Docker Compose file.](https://phoenixnap.com/kb/wp-content/uploads/2023/09/output-from-docker-compose-run-rm-certbot-certonly-webroot-etc-letsencrypt-nginx-docker.png)

Agree to the **Terms of Service** by typing **`y`** and pressing **Enter**.

![Agreeing to the Let's Encrypt terms of service.](https://phoenixnap.com/kb/wp-content/uploads/2023/09/output-from-docker-compose-run-rm-certbot-certonly-webroot-cont-letsencrypt-nginx-docker-pnap.png)

Wait for the procedure to finish. If Docker reports no errors, run the command without the **`--dry-run`** flag:

```
docker-compose run --rm certbot certonly --webroot --webroot-path /var/www/certbot/ -d [domain-name]
```

### Step 5: Add HTTPS to Nginx Configuration File

Once Certbot authenticates the server, add an HTTPS server block to the configuration file you created earlier. Follow the steps below to edit your Nginx deployment:

1. Open the configuration file:

```
sudo nano /etc/nginx/conf.d/app.conf
```

2. Add the following server block to the end of the file. Replace **`[domain-name]`** with your actual domain name.

```
server {
    listen 443 default_server ssl http2;
    listen [::]:443 ssl http2;

    server_name [domain-name];

    ssl_certificate /etc/nginx/ssl/live/[domain-name]/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/live/[domain-name]/privkey.pem;
    
    location / {
    	proxy_pass http://[domain-name];
    }
}
```

The entire file should look like in the example below.

![Adding HTTPS to Nginx configuration file.](https://phoenixnap.com/kb/wp-content/uploads/2023/09/editing-etc-nginx-conf-d-app-conf-2-letsencrypt-nginx-docker.png)

3. Reload Nginx:

```
docker-compose restart
```

Alternatively, if you cannot afford the downtime the command above causes, execute the command below:

```
docker-compose exec webserver nginx -s reload
```

### Step 6: Renew Certificates

Let's Encrypt certificates last for three months, after which it is necessary to renew them. To renew certificates, execute the following command:

```
docker-compose run --rm certbot renew
```