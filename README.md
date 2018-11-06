

# MARVELO
MARVELO stands for Multicast Aware Routing for Virtual network Embedding with Loops in Overlays. MARVELO framework is used to distribute functions (let them be network or signal processing) among nodes in the network and allow communication between the functinos. It is intended for transforming existing centralized implementations into distributed versions.
The distribution can be prepared manually (by creating or editing an XML file) or auto-generated by giving MARVELO the available and required resources in CSV format (auto-creating CSV files is in progress).

This framework has been tested on Raspberry Pi 3 (RPi3) and Debian distrubtions (Raspbian strech and Kali). Thus, the network can consists of Laptops instead of RPis.
## Quick Setup
There are two roles in MARVELO; Server(the brain who will distribute the blocks) and Client (who will run the process). For the sake of consistency, please make sure that you have user "asn" on your RPis
```sh
$ adduser asn
$ usermod -aG sudo asn
```

Note that this may be needed for the client but not necessary for the server.
### Server Installation
* copy the **asn_server** folder to **/home/asn**
* Download the required python packages 
        ```  $ sudo apt-get install  python    python-pip    python-dev    build-essential    ntp    ntpdate    rsync    python-lxml ```
       ```  $ pip install netifaces pyomo pandas```
       
* Setup the ad-hoc network. If you have limited experience, here is a quick setup for ad-hoc network
  * change the network configuration of wireless interface 
         ```
      $ sudo nano /etc/network/interfaces
         ```
  *  comment your wireless interface config (let’s say ***wlan0***) and replace it with
  ``` 
       iface wlan0  inet static
       
       address 10.1.1.254
       
       netmask 255.255.255.0
       
       wireless-channel 1
       
       wireless-essid marvelo_network
       
       wireless-mode ad-hoc 
  

### Client installation

* Copy **asn_daemon** folder to **/home/asn_daemon**
* Download packages needed to run the daemon (and some demos for testing)
     ```
     $ sudo apt-get install  python    python-pip    python-dev    build-essential    ntp    ntpdate    rsync    systemd    python-lxml    python-scipy 
      
     $ pip install numpy netifaces 
     ```
* Setup the ad-hoc network. Similar to the server, edit `/etc/network/interfaces`  and choose an IP in the same subnet for your network. Example 
```                        
            allow-hotplug wlan0

            iface wlan0 inet static

            address 10.1.1.101

            netmask 255.255.255.0

            wireless-channel 1

            wireless-essid marvelo_network

            wireless-mode ad-hoc
```
* Add the daemon as a service and make it start at reboot
  *  copy the file `asn_server/ansible/ templates/asn_daemon.service.j2`to `/etc/systemd/system/asn_daemon.service` [on the client]
  * run the following commands on the client:
      ```
      $sudo systemctl start asn_daemon  
      
      $sudo systemctl enable asn_daemon
      ```
