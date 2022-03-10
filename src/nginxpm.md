---: Install NginX Proxy Manager
---
# Install NginX Proxy Manager on Raspberry Pi

Once connected we need to create a folder so type the following and press `enter`:

```sh
mkdir nginx
```

Now we need to move into that directory using the following and press `enter`:

```sh
cd nginx
```

We now need to create the file config.json use the following to open the nano editor so we can input some data then press `enter`.

```sh
nano config.json
```

Now we have a file to write to copy and paste the following into the file.

Note: Replace all `changeme` values with something unique and secure.

Note: The below details have been updated and are confirmed working as of 24th October 2021.

```json
{
  "database": {
    "engine": "mysql",
    "host": "db",
    "name": "npm",
    "user": "changeme",
    "password": "changeme",
    "port": 3306
  }
}
```
Once you have done that press `Ctrl + X` then Y to save and `Enter` to exit the nano editor.

We now have to create one more file this one is a docker-compose.yml file.

Type the following to create the file in the nano editor:

nano docker-compose.yml
Like before change all `changeme` values to match the same as set in the `config.json` file.

Note: Create a new password for `MYSQL_ROOT_PASSWORD`.

If you are using an external drive to store your container data, change the folder location under `volumes:` before the colon : to your desired location. Leave all ports values the same.

```yml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "changeme"
      DB_MYSQL_PASSWORD: "changeme"
      DB_MYSQL_NAME: "npm"
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'changeme'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'changeme'
      MYSQL_PASSWORD: 'changeme'
    volumes:
      - ./data/mysql:/var/lib/mysql
```

Once you have done that press `Ctrl + X` then Y to save and `Enter` to exit the nano editor.

To deploy the dockers run the following command:

```sh
sudo docker-compose up -d
```

This will take some time to finish.

Once complete you can check that the docker containers exist by typing the following:

```sh
sudo docker ps
```

Or you can check in Portainer by logging in via your browser and navigating to `Containers`.

Note: Replace `RASPBERRYPIIP` with your raspberry pi IP address followed by port 9000. Example `http:192.168.2.5:9000`

Default Administrator User
Email:    `admin@example.com`
Password: `changeme`