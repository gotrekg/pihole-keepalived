### Change pihole Web port and automatic redirect to /admin 

Create new file:

      sudo nano /etc/lighttpd/conf-enabled/20-custom-port.conf
Paste this values:

      server.port := 8080
      $HTTP["url"] !~ "^/admin" {
      url.redirect = (".*" => "/admin")


Restart lighttpd service:

            sudo systemctl restart lighttpd.service
      
