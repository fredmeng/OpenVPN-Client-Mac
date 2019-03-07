# OpenVPN-Client-Mac

<p>This is about how to set up the OpenVPN client on your <b>Mac</b> with step-by-step instructions.</p>

<p><br>Step 1: Find following 3 files on your OpenVPN server and then scp to your Mac</p>

<pre>
/etc/easy-rsa/easyrsa3/pki/ca.crt
/etc/easy-rsa/easyrsa3/pki/issued/macbook.crt
/etc/easy-rsa/easyrsa3/pki/private/macbook.key
</pre>

<p><br>Step 2: Install <b>Tunnelblick</b></p>

<p>Download Tunnelblick, a free software for OpenVPN on OS X and macOS on <a href="https://tunnelblick.net/" target="_blank">https://tunnelblick.net/</a> and install it.</p>

<p><br>Step 3: Create a config file for the OpenVPN client (Tunnelblick) i.e. macbook.ovpn. Following is an example.</p>

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
dev tun

# The hostname/IP and port of the server.
# You can have multiple remote entries
# to load balance between the servers.
remote ec2-1-2-3-4.x.compute.amazonaws.com 1194 udp

# Keep trying indefinitely to resolve the
# host name of the OpenVPN server.  Very useful
# on machines which are not permanently connected
# to the internet such as laptops.
resolv-retry infinite

# Most clients don't need to bind to
# a specific local port number.
nobind

# Try to preserve some state across restarts.
persist-key
persist-tun

# SSL/TLS parms.
# See the server config file for more
# description.  It's best to use
# a separate .crt/.key file pair
# for each client.  A single ca
# file can be used for all clients.
ca ca.crt
cert macbook.crt
key macbook.key

# Select a cryptographic cipher.
# If the cipher option is used on the server
# then you must also specify it here.
# Note that v2.4 client/server will automatically
# negotiate AES-256-GCM in TLS mode.
# See also the ncp-cipher option in the manpage
cipher AES-256-CBC

# Enable compression on the VPN link.
# Don't enable this unless it is also
# enabled in the server config file.
;comp-lzo

# Set log file verbosity.
verb 3

# Silence repeating messages
;mute 20
</pre>

<p><br>Step 4: Put ca.crt, macbook.crt, macbook.key and macbook.ovpn in the same folder, i.e. ~/ovpn</p>

<p><br>Step 5: Import the configuration (i.e. macbook.ovpn) to your Tunnelblick</p>

<p><br>Step 6: Enjoy safer internet on your Mac!</p>

<p><br><br>Are you looking for the instructions for the server too? No worries. Check this out - <a href="https://github.com/fredmeng/OpenVPN-Server" target="_blank">Build your private OpenVPN server on Amazon AWS</a>. </p>
