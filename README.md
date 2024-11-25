# 2420 as 2 part 1

## Set up a new server

This section has three main part: creating a new user, set up nginx and set up ufw

nginx will do serving static content (e.g., HTML, CSS, JS) or act as a reverse proxy for backend servers
ufw will do seting up firewall rules, allowing only specific ports while blocking others

### Create a new user account

```bash
sudo useradd -r -m -d /var/lib/webgen -s /usr/bin/nologin webgen
```
- this command line will make a new non-login user
- -r: create a system account
- -m: create the user's home directory
- -d: the new user will be created using HOME_DIR as the value for the user's login directory
- -s: sets the path to the user's login shell

```bash
sudo mkdir -p /var/lib/webgen/bin /var/lin/webgen/HTML
# making two directories same as the provided directory structure
# -p: pass to make the directory if the directory already exists

sudo chown -R webgen:webgen /var/lib/webgen
# change the directory owner and group to webgen
# -R: operate on files and directories recursively
```

### Set up nginx

```bash
sudo pacman -Syu nginx # install nginx on Arch Linux

sudo mkdir -p /etc/nginx/sites-available # create the directory for server block file
sudo mkdir -p /etc/nginx/sites-enabled

sudo nvim /etc/nginx/sites-available/webgen.conf

sudo ln -s /etc/nginx/sites-available/webgen.conf /etc/nginx/sites-enabled/webgen.conf
# create the symbolic link to enable the configuration
```

In the webgen.conf file, you need to add the text below

```bash
server {
	listen 80;
    	listen [::]:80;
    
    	server_name _;
    
    	root /var/lib/webgen/HTML;
    	index index.html;

		location / {
        	try_files $uri $uri/ =404;
    	}
}
```

Check the main nginx.conf file

```bash
include /etc/nginx/sites-enabled/webgen.default;
# if it's not there, add it to nginx.conf
```
```bash
sudo nginx -t # test the configuration
sudo systemctl start nginx # start nginx
sudo systemctl status nginx # check nginx status
```

The benefit of creating a system user for this task rather than using a regular user or root 

There are three benefits: enhanced security, separation of concerns and principle of least privilege.

- Enhanced security: since system user has limit privillege, it can reduce the system wide damage.
- Separation of concerns: system user isolates tasks so it makes logs and processes easier to manage.
- Principle of least privilege: system user minimizes the attack surface by having only necessary permissions.

Verifying timer, check logs and service execution

```bash
sudo systemctl status generate-index.timer # check timer status
sudo systemctl status generate-index.service # check service status
sudo journalctl -u generate-index.service 
# check logs, use --since or --until; these filter the specific period
```

Importance of using separate block file

- Avoiding errors: modifying the main nginx.conf directly increases the risk of breaking entire server
- Ease of management: individual server block files can be enabled or disabled with symbolic links, simplifying maintenance.

Check nginx status and test nginx configuration

```bash
sudo systemctl status nginx # check nginx status
sudo nginx -t # test nginx configuration
```

Check firewall status
```bash
sudo ufw status verbose # check firewall status
```



