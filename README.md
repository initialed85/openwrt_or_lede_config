### Openwrt/LEDE configuration stuff

This repo contains some useful OpenWrt/LEDE configurations that (I feel) aren't clearly documented elsewhere.

#### dot1q_fed_to_vaps

This folder contains a configuration for a TP-Link Archer C7 platform running LEDE. The specifics are as follows:

* Physical port 4 receives a VLAN trunk from a Cisco switch
* Physical ports 1, 2 and 3 provides access to VLAN10
* The device itself is present on Vlan7 (used as the management VLAN)
* wlan0 and wlan1 put out four SSIDs each:
	* SSID_Vlan6 - guest wireless (no security)
	* SSID_Vlan8 - Vlan8 (secured)
	* SSID_Vlan9 - Vlan9 (secured)
	* SSID_Vlan10 - Vlan10 (secured)

With regard to the switch configuration in etc_config_network:

* Port 0 = eth1 = CPU; this is needed if you want to interact with any switched traffic using the device
* Port 1 = eth0; unused on this platform
* Port 2 = physical port 1
* Port 3 = physical port 2
* Port 4 = physical port 3
* Port 5 = physical port 4

So, we tag all VLANs on port 0 (CPU) so that we can ultimately pull them into the OS and assign them interfaces (which
enables us to bridge them to VAPs) and we tag all VLANs on port 5 (physical port 4) because that's the root of those VLANs
(from the Cisco switch).
