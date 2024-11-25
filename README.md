# 2420 as 2 part 1

# Set up a new server

## Create a new user account

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

[!Question] The benefit of creating a system user for this task rather than using a regular user or root 

There are three benefits: enhanced security, separation of concerns and principle of least privilege.

- Enhanced security: since system user has limit privillege, it can reduce the system wide damage.
- Separation of concerns: system user isolates tasks so it makes logs and processes easier to manage.
- Principle of least privilege: system user minimizes the attack surface by having only necessary permissions.

[!Question] Verifying timer, check logs and service execution

```bash
sudo systemctl status generate-index.timer # check timer status
sudo systemctl status generate-index.service # check service status
sudo journalctl -u generate-index.service # check logs, use --since or --until; these filter the specific period
```

[!Question] Importance of using separate block file



