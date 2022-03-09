# Install Portainer - Raspberry Pi

With Docker set up and configured, we can use it to install Portainer to our Raspberry Pi.

As Portainer is available as a Docker container on the official Docker hub, we can pull the latest version using the following command.

```sh
sudo docker pull portainer/portainer-ce:latest
```

This command will download the docker image to your device, which will allow us to run it.

Using `:linux-arm` at the end of the pull request, we explicitly ask for it to download the ARM version of the container.

Once Docker finishes downloading the Portainer image to your Raspberry Pi, we can now run it.

Telling Docker to run this container requires us to pass in a few extra parameters.

In the terminal on your Pi, run the following command to start up Portainer.

```sh
sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

A few of the big things we do here is first define the ports we want Portainer to have access to. In our case, this will be port 9000.

We assign this docker container the name “portainer” so we can quickly identify it if we ever needed.

Additionally, we also tell the Docker manager that we want it to restart this Docker if it is ever unintentionally offline.

## Using Portainer’s Web User Interface

This section will show you how you can access the Portainer web interface that is running on your Raspberry Pi.

We will also show you how to complete the initial setup experience of the software.

## Accessing the Web Interface

Before we can do anything with the software, you will need to connect to its web interface.

To do this, you will need a web browser of your choice and know your Raspberry Pi’s IP address.

1. If you don’t know your Raspberry Pi’s local IP address, it is relatively straightforward to retrieve it.

By using the hostname command, we can have it print out the local IP assigned to our Pi.

```sh
hostname -I
```

Make sure to use a capital I when using this command.

2. Now, all you need to do to access Portainer is to go to the following in your browser.

You can see that in this address, we have referenced the port `:9000` at the end. This is the port that Portainer makes itself available at.


`http://[PIIPADDRESS]:9000`

Make sure that you replace `[PIIPADDRESS]` with the local IP of your Raspberry Pi. You should have retrieved this IP in the previous step.