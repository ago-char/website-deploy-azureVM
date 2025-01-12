# Website deployment in Azure
- create website
- run web server (nginx)
- Verify website is running locally
- make rule in azure to allow port 80 for all internet users
- access website via azure public ip
- link azure public ip to your domain name
- link cert for your domain name to enable https

## Create Website
1. Place your `index.html` and other website files under `/var/www/html`

## Run your webserver
Install `nginx` if you have not already with `sudo apt install nginx`
1. `sudo systemctl start nginx`

## Verify website is running locally
Use any of the command (from azure vm) to verify if your website is up and running :
1. `curl localhost`
2. `wget localhost`
3. `brave localhost`

## Make it available for all internet users
Till now your website is only available for yourself locally, make sure your port 80 is open for all internet users to make it available on the web.
1. Create Inbound rule to allow port 80 for HTTP for all internet users

## Verify if working
Use any of the following command from any system (recommended is to try from the system that is not connected to same network as of your pc)
1. `curl <public_ip>`
2. `wget <public_ip>`
3. `brave <public_ip>`

## Get and link domain name for your website
1. Visit [godaddy](http://godaddy.com) and search and buy any domain you want as per availability. Say you pick `example.life`.
2. Create DNS Record as follow:
	- **A** record with Name=@, Data=<public_ip>
	- **CNAME** record with Nmae=www, Data=example.life
	(Optionally, you can create other **CNAME**, **SOA**, etc. DNS records too)
3. Wait for 0-48 hours

## Verify if working
1. Use `nslookup example.life` and see if domain name is resolved.
2. If resolved, you can check if it is working for website:
	- `curl example.life`
	- `wget example.life`
	- `brave example.life`

## Get and link cert to enable https
1. Make sure your website is accessibe from the internet
2. As https operate on port 443, you should create inbound rule for HTTPS at port 443 for all the internet users
3. Use `certbot` from *Let's encrypt* for Cert:
	- Install Certbot
		`sudo snap install --classic certbot`
	- Prepare the Certbot command
		`sudo ln -s /snap/bin/certbot /usr/bin/certbot`
	- Get Cert and install your certs for nginx
		`certbot` will auto modify your nginx config files to get and install and use its free cert.
		Make sure nginx is running, your website is accessible from internet, because `certbot` will try and access your website for all installation process.
		`sudo certbot --nginx`
	- Test automatic renewal
		As `certbot` cert is only valid for 90 days, `certbot` places a cronjob somewhere at (`/etc/crontab` | `/etc/crom.*/*`, `systemctl list-timers`), make sure it will renew by testing with dry-run
		`sudo certbot renew --dry-run`

## Verify it HTTPS is enabled
Make sure that port 80, 443 is enabled on Azure and web server is running, after that you can visit website as `https://example.life` or use command line:
- `curl https://example.life`
- `wget https://example.life`
- `brave https://example.life`

**ENJOY! YOU HAVE YOUR OWN HTTPS ENABLED WEBSITE ON THE INTERNET.**