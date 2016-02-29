---
title: Set up automatic detection for DHCP or DNS servers using IEAK 11
description: Set up automatic detection for DHCP or DNS servers using IEAK 11
ms.assetid: c6bfe7c4-f452-406f-b47e-b7f0d8c44ae1
ms.prod: IE11
ms.mktglfcycl: deploy
ms.sitesec: library
---

# Set up automatic detection for DHCP or DNS servers using IEAK 11


Set up your network to automatically detect and customize Internet Explorer 11 when it’s first started. Automatic detection is supported on both Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS), letting your servers detect and set up your employee’s browser settings from a central location, using a configuration URL (.ins file) or a JavaScript proxy configuration file (.js, .jvs, or .pac).

Before you can set up your environment to use automatic detection, you need to turn the feature on.

![](images/wedge.gif)**To turn on the automatic detection feature**

-   Open Internet Explorer Administration Kit 11 (IEAK 11), run the IE Customization Wizard 11 and on the **Automatic Configuration** page, check **Automatically detect configuration settings**. For more information, see [Automatic Configuration: Internet Explorer Customization Wizard](automatic-configuration-internet-explorer-customization-wizard.md).

## <span id="Automatic_detection_on_DHCP_and_DNS_servers"></span><span id="automatic_detection_on_dhcp_and_dns_servers"></span><span id="AUTOMATIC_DETECTION_ON_DHCP_AND_DNS_SERVERS"></span>Automatic detection on DHCP and DNS servers


Automatic detection works even if the browser wasn't originally set up or installed by the administrator.

-   **Using DHCP servers:** For local area network (LAN)-based users. This server type lets you specify your global and subnet TCP/IP parameters centrally, defining your users' parameters by using reserved addresses. By doing it this way, a computer can move between subnets, automatically reconfiguring for TCP/IP when it starts.

    **Note**  
    Your DHCP servers must support the DHCPINFORM message, to obtain the DHCP options.

     

-   **Using DNS servers:** For users on dial-up connections. This server type uses a set of protocols and services on a TCP/IP network, which lets users search for other computers by using hierarchical, user-friendly names (hosts), instead of numeric IP addresses. To use this, you have to set up either the host record or the CNAME alias record in the DNS database file.

**Note**  
DHCP has a higher priority than DNS for automatic configuration. If DHCP provides the URL to a .pac, .jvs, .js, or .ins configuration file, the process stops and the DNS lookup doesn't happen.

 

![](images/wedge.gif)**To set up automatic detection for DHCP servers**

-   Open the [DHCP Administrative Tool](http://go.microsoft.com/fwlink/?LinkId=302212), create a new option type, using the code number 252, and then associate it with the URL to your configuration file. For detailed instructions about how to do this, see [Create an option 252 entry in DHCP](http://go.microsoft.com/fwlink/p/?LinkId=294649).

    **Examples**

    ``` syntax
    http://www.microsoft.com/webproxy.pac
    ```

    ``` syntax
    http://marketing/config.ins
    ```

    ``` syntax
    http://###.#.###.#/account.pac
    ```

    For more detailed information about how to set up your DHCP server, see your server documentation.

![](images/wedge.gif)**To set up automatic detection for DNS servers**

1.  In your DNS database file, the file that’s used to associate your host (computer) names to static IP addresses in a zone, you need to create a host record named, **WPAD**. This record contains entries for all of the hosts that require static mappings, such as workstations, name servers, and mail servers. It also has the IP address to the web server storing your automatic configuration (.js, .jvs, .pac, or .ins) file.

    The syntax is: &lt;*host name*&gt; IN A &lt;*ip address of host*&gt;

    **Examples**

    ``` syntax
    corserv IN A 192.55.200.143
    ```

    ``` syntax
    nameserver2 IN A 192.55.200.2
    ```

    ``` syntax
    mailserver1 IN A 192.55.200.51
    ```

    -OR-

    Create a canonical name (CNAME) alias record, named **WPAD**. This record lets you use more than one name to point to a single host, letting you host both an FTP server and a web server on the same computer. It also includes the resolved name (not the IP address) of the server storing your automatic configuration (.pac) file.

    **Note**  
    For more information about creating a **WPAD** entry, see [Creating a WPAD entry in DNS](http://go.microsoft.com/fwlink/?LinkId=294651).

     

2.  After the database file propagates to the server, the DNS name, *wpad.&lt;domain&gt;.com* resolves to the server name that includes your automatic configuration file.

    **Note**  
    IE11 creates a default URL template based on the host name,**wpad**. For example, *http://wpad.&lt;domain&gt;.com/wpad.dat*. Because of this, you need to set up a file or redirection point in your web server **WPAD** record, named **wpad.dat**. The **wpad.dat** record delivers the contents of your automatic configuration file.

     

 

 




