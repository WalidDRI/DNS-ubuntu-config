# DNS-ubuntu-config
To configure BIND9 for a local network, web service, FTP service, and mail service with MX record, you can follow these steps:

1. Install BIND9:

   ```bash
   sudo apt update
   sudo apt install bind9
   ```

2. Configure BIND9 for the Local Network:

   - Open the BIND configuration file using a text editor:
   
     ```bash
     sudo nano /etc/bind/named.conf.local
     ```

   - Inside the file, add the following configuration for your local network zone, replacing `localnetwork.com` with your desired domain name and `192.168.1.0/24` with your local network's IP range:
   
     ```bind
     zone "localnetwork.com" {
         type master;
         file "/etc/bind/zones/localnetwork.com.db";
     };
     ```

   - Save the file and exit the text editor.

   - Create the zone file for your local network:
   
     ```bash
     sudo nano /etc/bind/zones/localnetwork.com.db
     ```

   - Inside the file, add the following content, replacing `localnetwork.com` with your domain name and `192.168.1.0/24` with your local network's IP range:
   
     ```bind
     $TTL 86400
     @       IN      SOA     ns1.localnetwork.com. admin.localnetwork.com. (
                                     2023053101   ; serial number
                                     3600         ; refresh
                                     1800         ; retry
                                     604800       ; expire
                                     86400        ; minimum TTL
                                     )
     
             IN      NS      ns1.localnetwork.com.
     
     ns1     IN      A       192.168.1.1   ; Replace with your DNS server's IP address
     web     IN      A        192.168.1.2   ; Replace with your web server's IP address
     ftp     IN      A        192.168.1.3   ; Replace with your FTP server's IP address
     mail    IN      A        192.168.1.4   ; Replace with your mail server's IP address
     ```

   - Save the file and exit the text editor.

3. Configure BIND9 for Web Service:

   - Open the BIND configuration file using a text editor:
   
     ```bash
     sudo nano /etc/bind/named.conf.local
     ```

   - Inside the file, add the following configuration for your web service, replacing `example.com` with your domain name and `192.168.1.2` with your web server's IP address:
   
     ```bind
     zone "example.com" {
         type master;
         file "/etc/bind/zones/example.com.db";
     };
     ```

   - Save the file and exit the text editor.

   - Create the zone file for your web service:
   
     ```bash
     sudo nano /etc/bind/zones/example.com.db
     ```

   - Inside the file, add the following content, replacing `example.com` with your domain name and `192.168.1.2` with your web server's IP address:
   
     ```bind
     $TTL 86400
     @       IN      SOA     ns1.example.com. admin.example.com. (
                                     2023053101   ; serial number
                                     3600         ; refresh
                                     1800         ; retry
                                     604800       ; expire
                                     86400        ; minimum TTL
                                     )
     
             IN      NS      ns1.example.com.
             IN      A       192.168.1.2   ;

 Replace with your web server's IP address
     www     IN      A       192.168.1.2   ; Replace with your web server's IP address
     ```

   - Save the file and exit the text editor.

4. Configure BIND9 for FTP Service:

   - Open the BIND configuration file using a text editor:
   
     ```bash
     sudo nano /etc/bind/named.conf.local
     ```

   - Inside the file, add the following configuration for your FTP service, replacing `ftp.example.com` with your FTP domain name and `192.168.1.3` with your FTP server's IP address:
   
     ```bind
     zone "ftp.example.com" {
         type master;
         file "/etc/bind/zones/ftp.example.com.db";
     };
     ```

   - Save the file and exit the text editor.

   - Create the zone file for your FTP service:
   
     ```bash
     sudo nano /etc/bind/zones/ftp.example.com.db
     ```

   - Inside the file, add the following content, replacing `ftp.example.com` with your FTP domain name and `192.168.1.3` with your FTP server's IP address:
   
     ```bind
     $TTL 86400
     @       IN      SOA     ns1.example.com. admin.example.com. (
                                     2023053101   ; serial number
                                     3600         ; refresh
                                     1800         ; retry
                                     604800       ; expire
                                     86400        ; minimum TTL
                                     )
     
             IN      NS      ns1.example.com.
             IN      A       192.168.1.3   ; Replace with your FTP server's IP address
     ftp     IN      A       192.168.1.3   ; Replace with your FTP server's IP address
     ```

   - Save the file and exit the text editor.

5. Configure BIND9 for Mail Service with MX Record:

   - Open the BIND configuration file using a text editor:
   
     ```bash
     sudo nano /etc/bind/named.conf.local
     ```

   - Inside the file, add the following configuration for your mail service, replacing `mail.example.com` with your mail domain name and `192.168.1.4` with your mail server's IP address:
   
     ```bind
     zone "example.com" {
         type master;
         file "/etc/bind/zones/example.com.db";
     };

     zone "1.168.192.in-addr.arpa" {
         type master;
         file "/etc/bind/zones/1.168.192.db";
     };
     ```

   - Save the file and exit the text editor.

   - Create the zone file for your mail service:
   
     ```bash
     sudo nano /etc/bind/zones/example.com.db
     ```

   - Inside the file, add the following content, replacing `example.com` with your mail domain name and `192.168.1.4` with your mail server's IP address:
   
     ```bind
     $TTL 86400
     @       IN      SOA     ns1.example.com. admin.example.com. (
                                     2023053101   ; serial number
                                     3600         ; refresh
                                     1800         ; retry
                                     604800       ; expire
                                     86400        ; minimum TTL
                                     )
     
             IN      NS      ns1.example.com.
             IN      MX      10 mail.example.com.  ; Replace with your mail server's IP address
     mail    IN      A       192.168.1.4   ; Replace

 with your mail server's IP address
     ```

   - Save the file and exit the text editor.

   - Create the reverse zone file for your mail service:
   
     ```bash
     sudo nano /etc/bind/zones/1.168.192.db
     ```

   - Inside the file, add the following content, replacing `1.168.192` with your local network's IP range in reverse order:
   
     ```bind
     $TTL 86400
     @       IN      SOA     ns1.example.com. admin.example.com. (
                                     2023053101   ; serial number
                                     3600         ; refresh
                                     1800         ; retry
                                     604800       ; expire
                                     86400        ; minimum TTL
                                     )
     
             IN      NS      ns1.example.com.
     4       IN      PTR     mail.example.com.   ; Replace with your mail server's IP address in reverse order
     ```

   - Save the file and exit the text editor.

6. Restart BIND9:

   ```bash
   sudo systemctl restart bind9
   ```

With these configurations, your BIND9 server should be set up to serve DNS records for your local network, web service, FTP service, and mail service with the MX record.
