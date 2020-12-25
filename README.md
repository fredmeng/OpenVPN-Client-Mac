# OpenVPN-Client-Mac

<p>This is about how to set up the OpenVPN client on your <b>Mac</b> with step-by-step instructions.</p>

<p><br>Step 1: Find following 3 files on your OpenVPN server and then scp to your Mac</p>

<pre>
/etc/easy-rsa/easyrsa3/pki/ca.crt
/etc/easy-rsa/easyrsa3/pki/issued/mac.crt
/etc/easy-rsa/easyrsa3/pki/private/mac.key
</pre>

<p><br>Step 2: Install <b>OpenVPN Connect for Mac OS</b></p>

<p>Download <b>OpenVPN Connect for Mac OS</b> on <a href="https://openvpn.net/client-connect-vpn-for-mac-os/" target="_blank">https://openvpn.net/client-connect-vpn-for-mac-os/</a> and install it.</p>

<p><br>Step 3: Create a config file for the OpenVPN client (e.g. OpenVPN Connect for Mac OS) i.e. mac.ovpn. Following is an example.</p>

<pre>
# Specify that we are a client and that we
# will be pulling certain config file directives
# from the server.
client

# Use the same setting as you are using on
# the server.
# On most systems, the VPN will not function
# unless you partially or fully disable
# the firewall for the TUN/TAP interface.
;dev tap
dev tun

# Windows needs the TAP-Windows adapter name
# from the Network Connections panel
# if you have more than one.  On XP SP2,
# you may need to disable the firewall
# for the TAP adapter.
;dev-node MyTap

# Are we connecting to a TCP or
# UDP server?  Use the same setting as
# on the server.
proto udp4

# The hostname/IP and port of the server.
# You can have multiple remote entries
# to load balance between the servers.
remote ec2-1-2-3-4.x.compute.amazonaws.com 1194

# Choose a random host from the remote
# list for load-balancing.  Otherwise
# try hosts in the order specified.
;remote-random

# Keep trying indefinitely to resolve the
# host name of the OpenVPN server.  Very useful
# on machines which are not permanently connected
# to the internet such as laptops.
resolv-retry infinite

# Most clients don't need to bind to
# a specific local port number.
nobind

# Downgrade privileges after initialization (non-Windows only)
#user nobody
#group nobody

# Try to preserve some state across restarts.
persist-key
persist-tun

# If you are connecting through an
# HTTP proxy to reach the actual OpenVPN
# server, put the proxy server/IP and
# port number here.  See the man page
# if your proxy server requires
# authentication.
;http-proxy-retry # retry on connection failures
;http-proxy [proxy server] [proxy port #]

# Wireless networks often produce a lot
# of duplicate packets.  Set this flag
# to silence duplicate packet warnings.
mute-replay-warnings

# SSL/TLS parms.
# See the server config file for more
# description.  It's best to use
# a separate .crt/.key file pair
# for each client.  A single ca
# file can be used for all clients.
ca ca.crt
cert mac.crt
key mac.key

# Verify server certificate by checking
# that the certicate has the nsCertType
# field set to "server".  This is an
# important precaution to protect against
# a potential attack discussed here:
#  http://openvpn.net/howto.html#mitm
#
# To use this feature, you will need to generate
# your server certificates with the nsCertType
# field set to "server".  The build-key-server
# script in the easy-rsa folder will do this.
;ns-cert-type server

# If a tls-auth key is used on the server
# then every client must also have the key.
;tls-auth ta.key 1

# Select a cryptographic cipher.
# If the cipher option is used on the server
# then you must also specify it here.
cipher AES-256-CBC

# Enable compression on the VPN link.
# Don't enable this unless it is also
# enabled in the server config file.
;compress

# Set log file verbosity.
verb 3

# Silence repeating messages
;mute 20
</pre>

<p><br>Step 4: Put ca.crt, mac.crt, mac.key and mac.ovpn in the same folder, i.e. ~/ovpn</p>

<p><br>Step 5: Import the configuration (i.e. mac.ovpn) to your OpenVPN Connect for Mac OS</p>

<p><br>Step 6: Enjoy safer internet on your Mac!</p>

<p><br><br>Are you looking for the instructions for the server too? No worries. Check this out - <a href="https://github.com/fredmeng/OpenVPN-Server" target="_blank">Build your private OpenVPN server on Amazon AWS</a>. </p>
