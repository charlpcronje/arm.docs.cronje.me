# Install Docker on Raspberry Pi

Now is time to install Docker! Fortunately, Docker provides a handy install script for that, just run:

## Step 1

```sh
curl -sSL https://get.docker.com | sh
```

## Step 2 - Add a Non-Root User to the Docker Group

By default, only users who have administrative privileges (root users) can run containers. If you are not logged in as the root, one option is to use the sudo prefix.
However, you could also add your non-root user to the Docker group which will allow it to execute docker commands.

The syntax for adding users to the Docker group is:

```sh
sudo usermod -aG docker [user_name]
```

To add the permissions to the current user run:

```
sudo usermod -aG docker ${USER}
## Check it running:
groups ${USER}
```

Reboot the Raspberry Pi to let the changes take effect.

## Step 3 - Install Docker-Compose
Docker-Compose usually gets installed using pip3. For that, we need to have python3 and pip3 installed. If you don't have it installed, you can run the following commands:

```sh
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip
```

Once python3 and pip3 are installed, we can install Docker-Compose using the following command:

```sh
sudo pip3 install docker-compose
```

## Step 4

Enable the Docker system service to start your containers on boot

This is a very nice and important addition. With the following command you can configure your Raspberry Pi to automatically run the Docker system service, whenever it boots up.

```sh
sudo systemctl enable docker
```

With this in place, containers with a restart policy set to always or unless-stopped will be re-started automatically after a reboot.

## Step5 - Run Hello World Container

The best way to test whether Docker has been set up correctly is to run the Hello World container.

To do so, type in the following command:

```sh
docker run hello-world
```

Once it goes through all the steps, the output should inform you that your installation appears to be working correctly.

## Step 6 - A sample Docker Compose file

This section shows a quick sample of a Docker-Compose file, which starts three containers that once started will automatically come up, if the Raspberry Pi get fully power cycled. To learn more about the sample project, visit Docker Speed Test project on GitHub.

```yml
version: '3'
services:
  # Tests the current internet connection speed
  # once per hour and writes the results into an
  # InfluxDB instance
  speedtest:    
    image: robinmanuelthiel/speedtest:0.1.1
    restart: always
    depends_on:
      - influxdb
    environment:
      - LOOP=true
      - LOOP_DELAY=3600 # Once per hour
      - DB_SAVE=true
      - DB_HOST=http://influxdb:8086
      - DB_NAME=speedtest
      - DB_USERNAME=admin
      - DB_PASSWORD=<MY_PASSWORD>

  # Creates an InfluxDB instance to store the
  # speed test results
  influxdb:
    image: influxdb
    restart: always
    volumes:
      - influxdb:/var/lib/influxdb
    ports:
      - "8083:8083"
      - "8086:8086"
    environment:
      - INFLUXDB_ADMIN_USER=admin
      - INFLUXDB_ADMIN_PASSWORD=<MY_PASSWORD>
      - INFLUXDB_DB=speedtest

  # Displays the results in a Grafana dashborad
  grafana:
    image: grafana/grafana:latest
    restart: always
    depends_on:
      - influxdb
    ports:
      - 3000:3000
    volumes:
      - grafana:/var/lib/grafana

volumes:
  grafana:
  influxdb:
```  
  
To start the containers using Docker-Compose, run the following command:

```sh
docker-compose -f docker-compose.yaml up -d
```

Find Raspberry Pi Docker Images

Raspberry Pi is based on ARM architecture. Hence, not all Docker images will work on your Raspberry Pi.

Remember that when searching for images to pull from Docker Hub. Apply the Architectures filter to search for supported apps.

## How to Upgrade Docker on Raspberry Pi?

Upgrade Docker using the package manager with the command:

```
sudo apt-get upgrade
```
