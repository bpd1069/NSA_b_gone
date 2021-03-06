# NSA_b_gone

A Linux shell script to improve your privacy online.

# What it does

This script does the following:

- Implements a MAC address randomizer. A random MAC address is assigned to the configured network device.
- Implements a hostname randomizer. A random hostname is created for your machine, e.g. ```mqiCbaDyhelw```.
- Implements a very strict firewall which, by default, allows *no* internet traffic. To gain internet access, you need to run the application under the "internet" group (see [Usage](#usage)).

# Why?

The idea behind this script is that no rogue apps/services on your system can spy on you and send data to the internet. This is because you will very finely control which application can access the internet, by running them from a specific terminal (or with a specific command, see [Usage](#usage)). This hopefully means that keyloggers/camera spy apps/botnet apps/etc. won't be able to send their collected data upstream, or receive commands remotely. 

In addition, each time you run the script, your machine will look like a different machine on the network (due to the random MAC address and hostname). This prevents tracking you from your machine's unique MAC address or it's machine name. 

Some unintended upshots:

- you can gain infinite usage free WiFi by running this script each time your free WiFi allocation is used up
- you get *much* better control of your bandwidth usage (esp. useful when tethering over a mobile data connection). App updates, etc. won't automatically run in the background.

# Installation

- Download the script
- Optional: Edit the script and change the ```LAN=wlan0``` line to use whichever device you connect with.
- Optional: add the script to ```/etc/rc.local``` to have it run automatically on boot, or better yet, to ```/etc/network/if-pre-up.d/``` (but remove the ```.sh``` suffix and comment out the network stop/start commands) to have it apply everytime you connect to a network.

# Usage

- At startup, or *each time you get online*, run ```sudo ./nsa_b_gone.sh```. This will drop your network, erect the firewall, randomize your MAC address and hostname, and then re-enable networking.
- To run, for example, firefox with internet access, use the command: ```sudo -g internet firefox```. Note that this does **not** run Firefox as root, it still runs as your logged-in user.
- You can open a terminal with internet access by running: ```sudo -g internet -s```. Note that this does **not** open a root shell, but rather a shell with your user and the added internet group. Now, any app started from this terminal will have internet access. Any app spawned by an app started from this terminal will also have internet access.
- To stop the madness, simply run ```sudo ./stop_firewall.sh``` to get full internet back. It might look like I missed a trick by not calling it ```nsa_b_back.sh```, but that would've made tab-completion harder.

# Known Issues

- Your ```/etc/hosts``` file will, over time, need cleaning up from all the random hostnames inserted at the end. Simply delete all but the last hostname for the ```127.0.0.1``` address.
- The first byte of the random MAC address is not changed, since it needs to be even. Feel free to change this in the script.
- The ```ping``` command (ICMP, DNS) still works without the internet group. This means that DNS-tunneling is not prevented.

# Warnings

- This tool can be extremely frustrating to use at first, since you automatically expect all apps to have internet access. This will catch you out a lot, but have some patience, and set up your launcher short-cuts or command-line aliases to allow your most commonly used apps to have internet access.
- This tool does not anonymize your internet browsing. Your IP address, DNS requests, etc. can still be tracked. Use Tor for anonymity.
- This tool does not protect you from cookies or other tracking methods (e.g. applications you've logged into).
- For more information on staying private online, see: https://www.reddit.com/r/privacy/wiki/index
