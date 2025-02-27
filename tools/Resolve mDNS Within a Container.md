# Resolve mDNS Within a Container

Often, when setting up services in a private network, domain name resolution is required. While it is possible to refer to hosts directly by using static IP addresses, it needs additional configurations on setting up static IPs in `/etc/hosts`. And if the IP of a host changes, it won't work anymore. 

This is where Multicast Domain Name System (mDNS) becomes crucial. Similar to DNS, mDNS resolves domain names to IP addresses, but it does not require a central server or additional configurations. 

In practice, applications often utilize Docker for deployment due to its scalability and isolation benefits. However, implementing mDNS within Docker containers has several challenges.

- The container operates within a separate network created by Docker, which limits direct interaction with services on the host network.
- Even if the container is configured to use the host network, the avahi-daemon requires access to the system's dbus and the avahi-daemon socket.
- Most base images used for creating the application's image lack the necessary tools for mDNS name resolution.

To address these issues, here is a general solution.

## Dockerfile

To ensure the container has the necessary tools for mDNS, the Dockerfile needs to install `avahi-daemon`, `avahi-utils`, and `dbus` for building images.

```dockerfile
RUN apt update && apt -y install avahi-daemon avahi-utils dbus
```

## EntryPoint

Add this part to the existing `entrypoint.sh`. The following lines configure the avahi-daemon and ensures it starts correctly with modification to the hostname based on environment variables. 

```bash
# ensure the dbus system daemon is running
mkdir -p /var/run/dbus
dbus-daemon --system --fork

# configure the avahi server with a custom hostname if provided
if [[ -v AVAHI_HOSTNAME ]]; then
    if ! grep -q '^host-name=' /etc/avahi/avahi-daemon.conf; then
        sed -i "/^\[server\]/a host-name=$AVAHI_HOSTNAME" /etc/avahi/avahi-daemon.conf
    else
        sed -i "s/^host-name=.*/host-name=$AVAHI_HOSTNAME/" /etc/avahi/avahi-daemon.conf
    fi
fi

# start the avahi daemon
service avahi-daemon start
```

`AVAHI_HOSTNAME` is an environment variable provided in docker-compose file to modify the hostname in `/etc/avahi/avahi-daemon.conf`.

## Docker-Compose File

Based on the existing compose file, add the `entrypoint:`  to overwrite `entrypoint.sh` and provide a custom hostname after `AVAHI_HOSTNAME:`. It is necessary to use the `host` network, allowing it to interact with other services on the same network. 

```yaml
version: 
services:
  mdns:
    image: 
    network_mode: "host"
    command:
    entrypoint: /path/to/entrypoint.sh
    environment:
      AVAHI_HOSTNAME: 'device'
    volumes:
```
