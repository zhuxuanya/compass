# WireGuard VPN Configuration

WireGuard provides an easy way to manage and create a VPN. It is also faster than traditional VPNs because it uses UDP rather than TCP. TCP relies on a consistent connection, so there is always a check if connection exists before it is being published. UDP does not require that check. The authentication part is taken care by private and public key matching. Meaning if a client has the server's public key and the server has the client's public key, an encrypted connection can be established. Refer to [WireGuard](https://www.wireguard.com/) for information on how to use private and public keys to authenticate connections and tunnels.

## Set up the server

1. Install wireguard desired to be the server.

   ```
   sudo apt update
   sudo apt install wireguard
   ```

2. Generate the private key for the server.

   ```
   wg genkey | sudo tee /etc/wireguard/private.key
   sudo chmod go = /etc/wireguard/private.key
   ```

3. Create the public key for the server from the private key.

   ```
   sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
   ```

4. Create the server's interface configuration at `/etc/wireguard/wg0.conf`.

   ```
   sudo nano /etc/wireguard/wg0.conf
   ```

   ```
   [Interface]
   PrivateKey = base64_encoded_private_key_goes_here
   Address = 10.8.0.1/24 # any ip address
   ListenPort = 51820
   SaveConfig = true
   ```

5. Do this step to route peer's internet traffic through the server.

   ```
   sudo vim /etc/sysctl.conf
   ```

   Edit or uncomment the following line to:

   ```
   net.ipv4.ip_forward=1
   ```

   Exit and and load the new values for current terminal session, run:

   ```
   sudo sysctl -p
   ```

6. Configure server's firewall.

   Check the public network interface using:

   ```
   ip route list default
   ```

   The result is similar to `default via 192.168.31.1 dev wlo1 proto dhcp metric 600`.

   Add the following lines to the file ` /etc/wireguard/wg0.conf` after the `SaveConfig = true` line.

   ```
   PostUp = iptables -t nat -I POSTROUTING -o wlo1 -j MASQUERADE
   PreDown = iptables -t nat -D POSTROUTING -o wlo1 -j MASQUERADE 
   ```

   Allow the ssh port.

   ```
   sudo ufw allow 51820/udp
   sudo ufw allow OpenSSH
   sudo ufw disable
   sudo ufw enable
   sudo ufw status
   ```

7. Start the server.

   ```
   sudo systemctl enable wg-quick@wg0.service
   sudo systemctl start wg-quick@wg0.service
   sudo systemctl status wg-quick@wg0.service
   ```

## Set up the peer

1. Generate the private and public keys of the peer.

   ```
   wg genkey | sudo tee /etc/wireguard/private.key
   sudo chmod go= /etc/wireguard/private.key
   sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
   ```

   Create a folder manually if using a read-only file system like Ubuntu Core.

   ```
   mkdir ~/wireguard
   wg genkey | sudo tee ~/wireguard/private.key
   sudo chmod go= ~/wireguard/private.key
   sudo cat ~/wireguard/private.key | wg pubkey | sudo tee ~/wireguard/public.key
   ```

2. Create the peer's communication file.

   ```
   sudo nano /etc/wireguard/wg0.conf
   # or
   sudo nano ~/wireguard/wg0.conf
   ```

   Add the following configuration:

   ```
   [Interface]
   PrivateKey = base64_encoded_peer_private_key_goes_here
   Address = 10.8.0.2/24 # peer ip address
   
   [Peer]
   PublicKey = public_key of server
   AllowedIPs = 10.8.0.0/24 # addresses allowed 
   Endpoint = 192.168.31.1:51820 # ip address from step 6.
   PersistentKeepalive = 25
   ```

3. Add the peer's public key to the server.

   Get the public key from the peer.

   ```
   sudo cat /etc/wireguard/public.key
   # or
   sudo cat ~/wireguard/public.key
   ```

   Log into the server, and run the following command with public key from last step:

   ```
   sudo wg set wg0 peer public_key allowed-ips 10.8.0.2
   ```

   Check the status of the tunnel.

   ```
   sudo wg
   ```

4. Connect peer to the tunnel.

   ```
   sudo apt install resolvconf # if normal unix operating system, UC20 cannot
   sudo wg-quick up wg0
   # or
   sudo wg-quick up ~/wireguard/wg0.conf # UC20
   ```

   Check status.

   ```
   sudo wg
   # and
   ifconfig # check for the client vpn ip address
   ```

   Check sshd.

   ```
   sudo systemctl status sshd
   # if not
   sudo apt install openssh-server
   ```

   Check ssh from another vpn client

   ```
   ssh ubuntu@10.8.0.2 # replace with username and client vpn ip
   # This checks if the peer-to-peer connection is forwarded by the vpn server.
   ```

5. Set up config to start on startup both on client and peer.

   ```
   sudo systemctl enable wg-quick@wg0.service
   sudo systemctl daemon-reload
   ```
