# Installation

1. Search and Download portainer-ce image in Docker of DSM

2. Create folder 

   ```
   /volume1/docker/portainer
   ```

    in DSM

4.  Turn on SSH in DSM

5. Make sure as root

6. ```shell
   docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /volume1/docker/portainer:/data portainer/portainer-ce
   ```

7. ```shell
   # Run the command below to make sure portainer up and running
   docker ps 
   ```

8. ```shell
   # Run command to grab the network interface for Maclan, for bonded network interface please use (ovs_bond[x])
   ifconfig
   ```

   

9. Go to http://IP_OF_YOUR_NAS:9000

10. Create admin user

11. Choosing Docker

12. For Steps after 12, those are for Macvlan creation, it is optional if not intended to 

13. Go to Local -> Network

14. Create a new network configuration:

    1. **[Your Application].config**
    2. macvlan
    3. Configuration
    4. Enter the interface from previous ifconfig command
    5. Network  Setup
       1. Subnet = the entire local subnet, so mine will be ***192.168.0.0/24\***
          Gateway = is usually your router ***192.168.0.1\***
          IP Range = I only want to use a single IP so I’ll put ***192.168.0.240/32\***
    6. Create the Network

15. Add a new work from the network configuration just created 

    1. macvlan
    2. Creation
    3. Configuration -> [Your network configuration]
    4. Advanced Configuration
       1. Restrict -> Y
       2. Enable manual attachment -> Y
       3. Enable access control -> Up to You

16. [Advanced installation for pi hole](#pihole)



## pihole

1. Create shared folders in docker 

```
/volume1/docker/pihole/etc/pihole
```

```
/volume1/docker/pihole/etc/dnsmasq.d
```

2.  Access Portainer UI -> Containers

   1. Create a new container

   2. Image: pihole/pihole:latest

   3. Port mapping

      1. 443 -> 443 TCP
      2. 80 -> 80 TCP
      3. 53 -> 53 TCP,UDP

   4. Enable access control - > N

   5. Volumes Mount

      1. /etc/dnsmasq.d **bind** /volume1/docker/pihole/etc/dnsmasq.d
      2. /etc/pihole **bind** /volume1/docker/pihole/etc/pihole

   6. Network Setting

      1. Network -> [your configuration network]
      2. domain name -> [up to you]
      3. IPV4 -> 192.168.0.240
      4. DNS1 -> 127.0.0.1
      5. DNS2 -> [up to you]

   7. ENV

      1. TZ -> Asia/Hong_Kong
   2. DNS1 -> ISP DNS
      3. DNS2 -> GOOGLE DNS
   
   8. Restart Policy
   
      1. Unless Stopped
   
   9. Deploy the docker
   
   10. Find the password in log
   
       