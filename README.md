# What & Why & How

For servers whose addresses are represented solely by a literal IP, e.g. 192.168.2.10:25565, disable reverse DNS lookups in the corresponding InetAddress object

Many non-loopback IPs lack associated domain names, which makes reverse lookups time-consuming

     // java.net.InetAddress#getHostName(boolean)
     String getHostName(boolean check) {
     if (holder().getHostName() == null) {  // It will be null if InetAddress.getByName() received a literal IP
     holder().hostName = InetAddress.getHostFromNameService(this, check);  // <-- takes forever
     }
     return holder().getHostName();
     }

This option sets the domain of those servers directly to their IP, bypassing the reverse DNS check

This results in a 1s ~ 5s reduction in time for servers with literal IP address. Affects the following environments:

    Pinging the server in the server list screen
    Connecting to the server

# Environment

    Client-side only
    Fabric / Forge / NeoForge mod loader. No extra requirement is needed
