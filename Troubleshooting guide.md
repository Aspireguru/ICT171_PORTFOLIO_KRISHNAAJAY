# 🛠️🐞💡 TROUBLESHOOTING ISSUES WITH TIPS

## 🌐🛠️🔥 WEB SERVER ISSUES (NGINX)
In case Nginix configuration fails: If sudo nginx -t shows an error, read the message carefully. It could be on a specific file or line.

### Examples
Missing “;” or “}” in the config will cause failure. We need to open the file and fix syntax
server_names_hash_bucket_size – means the domain name is long, and the default hash bucket size for server names is not enough. This error suggests increasing server_names_hash_bucket_size.

### How to fix it
Open /etc/nginx/nginx.conf

Look for the line:
- server_names_hash_bucket_size 64;
- Remove the # to set the bucket size to 64.
- Save the file and run nginx -t again.
- Nginx service won’t start/restart: Use systemctl status nginx to see the log output.
- A common cause could be syntax errors like above or a port conflict. We also need to make sure we did ln -s the config into sites-enabled. If the file is in sites-available but not enabled, Nginx won't load, and you'll see the default page. If the site isn't showing at all, run this command: ls /etc/nginx/sites-enabled/ //confirms symlink
- If you are able to see main.krishnaajay.online good if not create the symlink and reload Nginx.
  
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🌐🧭⚠️ DNS ISSUES

### DNS propagation not working
Double-check that the A record is correctly added on Namecheap with the exact host portfolio and correct IP. Always remember it can take 30-1 hour for a DNS change to be globally acknowledged. Use tools like whatsmydns.net to see if DNS has propagated in various regions.

### Records conflicting:
In case you accidentally created two A records for portfolio (or a CNAME and an A record for the same name). DNS could not be working properly. To fix this, remove any duplicates or unnecessary records. Only A record for the portfolio should exist unless you have IPv6, then an AAAA record could also be added for an IPv6 address.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## ⚠️🌐📄 SEEING DEFAULT NGINX PAGE, NOT PORTFOLIO

### Possible reason list:
- Server name might not match. There could be a typo in the config main.krishnaajay.online it must match the domain. If you are unsure we can just add If unsure, you can add a line like server_name main.krishnaajay.online http://main.krishnaajay.online. This is just for testing, not final; config.

- DNS could be pointing to the wrong server. If you have multiple servers, verify the IP address connection and that the domain resolves to this IP address.

- The default server block could be taking precedence. This happens when the request doesn’t match any server name, Nginx will use the default server by disabling the default config, and we ensure that DNS points correctly. The severe block is used. If the default config isn't removed, try removing it and reloading it.
  
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🚫📁🔐 PATH OR FILE PERMISSION ISSUES (404/403 ERRORS)
We are able to reach the site, but things are not loaded, such as CSS/JS/images.  Check Nginx error logs (/var/log/nginx/error.log). If we see 403 Forbidden or 404 Not Found, we can proceed further.

### Error messages
403 Forbidden(Nginx could not read the file). Make sure the file exists in the correct location and that permissions are not restrictive.  chmod 755 on the web directory should prevent this by giving world-read access. If you set different permissions, ensure that at least the www-data user can read the files.

### Best fix is to set permissions/ ownership to nbinx:
### Linus commands 
- sudo chown -R www-data:www-data /var/www/krishnaajay.online/main
- sudo chmod -R 755 /var/www/krishnaajay.online/main
404 Not Found. This typically means files are not where they are supposed to be, or the paths in the HTML are wrong. For example, if HTML is referencing/css/style.css but on the server the CSS folder is named differently or not uploaded, it will show 404. To fix this make sure folder structure matches what HTML expects, We can use droplet console to do this.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 🔥🚫🛡️ FIREWALL COULD BE BLOCKING ACCESS
The firewall is causing issues with the firewall settings. If UFW is enabled but only Nginx HTTP is allowed, after enabling HTTPS, you should also allow Nginx HTTPS, or we should switch to combined profiles. We can do this by: 

#### Linus command
- sudo ufw allow 'Nginx Full'
- sudo ufw delete allow 'Nginx HTTP'
This command opens ports 80 and 443 and removes the HTTP-only rule.

## 🔐📜⚠️ SSL/CERTBOT ISSUES
If sudo certbot --nginx didn’t succeed, the output usually explains

### Two main errors that can cause this can happen are
- DNS problem: NXDOMAIN looking up A for portfolio.krishnaajay.online” (no DNS record found)
- We can verify by running dig portfolio.krishnaajay.online on the server or another system, it should return your droplet’s IP. If not, fix the DNS and wait.
- “Timeout”
- Let’s Encrypt couldn’t connect to your server on port 80. This could be because Nginx was not running, or the firewall is blocking port 80. This could also happen if you provided the wrong port configuration.
- We were troubleshooting this by making sure Nginx is running (systemctl status nginx) and is on port 80. We can also verify that http://main.krishnaajay.online is working on an external network. If you find that UFW or another firewall is the issue, port 80 is closed; open it and retry certbot.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🚫📈🔐 RATE LIMITING ISSUE WITH CERTBOT
If you ran Certbot many times unsuccessfully, you might hit Let’s Encrypt’s rate limits. If this happens, don't spam, wait for roughly an hour, then retry. Use --dry-run with Certbot can test the process without counting against limits.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🔓🌐⚠️ HTTPS PAGE NOT SHOWING LOCK
HTTPS is working, but shows a grey padlock or has a warning. Check if your site is loading things over http://. Browsers will flag “mixed content”. Since my site is static, links should be relative or protocol-agnostic, but if any link in the HTML is hard-coded to http:// (like an external script or image), change it to https:// if available.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## ♻️🔐⚠️ RENEWAL ISSUES
If auto-renewal fails, you should get an email from Let’s Encrypt if a cert is nearing expiry without successful renewal. This could be due to the cron job not being able to reach the server (maybe the server was down or the firewall changed). 

### To troubleshoot this, run 
### Linus command
1. sudo certbot renew manually.
2. sudo systemctl list-timers --all | grep certbot //ensures cron or systemd timers are active
These commands should fix the issues.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 🖼️🔗📚 REFERENCE FOR IMAGES USED IN PROJECT SECTION
- Chinability. (n.d.). Currency converter. Chinability. Retrieved May 17, 2025, from https://www.chinability.com/tag/currency-converter/
- Vecteezy. (n.d.). Digital clock face showing time, hours, minutes, and seconds [Vector graphic]. Vecteezy. Retrieved May 17, 2025, from https://www.vecteezy.com/vector-art/11514536-digital-clock-face-showing-time-hours-minutes-and-seconds
- Hoffman, C. (2021, August 23). How to install Windows 11 on an unsupported PC. How-To Geek. https://www.howtogeek.com/759925/how-to-install-windows-11-on-an-unsupported-pc/
- AnyViewer. (n.d.). What is remote access software? AnyViewer. Retrieved May 17, 2025, from https://www.anyviewer.com/how-to/what-is-remote-access-software-0427.html

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## 📄🔧📚 REFERENCE FOR INSTALLATION GUIDE
- Vanderknaap, R. (n.d.). How to deploy a static site to DigitalOcean. Robinvanderknaap.dev. Retrieved May 17, 2025, from https://robinvanderknaap.dev/blog/how-to-deploy-a-static-site-to-digitalocean/
- DigitalOcean. (2022, April 25). How to install Nginx on Ubuntu 22.04. DigitalOcean. https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
- DigitalOcean. (2022, April 25). How to secure Nginx with Let's Encrypt on Ubuntu 22.04. DigitalOcean. https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-22-04
- Namecheap. (n.d.). How can I set up an A address record for my domain? Namecheap. Retrieved May 17, 2025, from https://www.namecheap.com/support/knowledgebase/article.aspx/319/2237/how-can-i-set-up-an-a-address-record-for-my-domain/
- DigitalOcean. (n.d.). How to fix common Let's Encrypt errors. DigitalOcean. Retrieved May 17, 2025, from https://www.digitalocean.com/community/tutorials/how-to-fix-common-letsencrypt-errors
- Let’s Encrypt Community. (2021, December 13). Certbot + nginx: Some challenges have failed. Let’s Encrypt Community. https://community.letsencrypt.org/t/certbot-nginx-some-challenges-have-failed/194199
- GitHub. (n.d.). GitHub. Retrieved May 17, 2025, from https://github.com/
- ThemeWagon. (n.d.). Ronaldo – Free Bootstrap 4 HTML5 one-page personal portfolio website template. ThemeWagon. Retrieved May 17, 2025, from https://themewagon.com/themes/free-bootstrap-4-html5-one-page-personal-portfolio-website-template-ronaldo/
- Code Explained. (2020, May 26). Form validation in JavaScript [Video]. YouTube. https://www.youtube.com/watch?v=rsd4FNGTRBw
- MDN Contributors. (n.d.). Client-side form validation. MDN Web Docs. Mozilla. https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation

