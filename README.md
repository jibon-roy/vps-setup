# vps-setup
# VPS SETUP

### STEP 1
<pre>
  <code id="example-code">
   ssh root@your_ip_adress
  </code>
</pre>
Then enter your password

### STEP 2
Updated backdated packges
<pre>
  <code id="example-code">
   sudo apt update
  </code>
</pre>
<pre>
  <code id="example-code">
   sudo apt upgrade
  </code>
</pre>

### STEP 3
Enabling and Configuring UFW (Uncomplicated Firewall)

<pre>
  <code id="example-code">
   sudo ufw enable
  </code>
</pre>

<pre>
  <code id="example-code">
   sudo ufw allow 22
  </code>
</pre>

Check allow port status 

<pre>
  <code id="example-code">
   sudo ufw status
  </code>
</pre>

### STEP 3
Install NVM

<pre>
  <code id="example-code">
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  </code>
</pre>

<pre>
  <code id="example-code">
    nvm -v
  </code>
</pre>
Note: If version not showing please close the terminal and try again

## STEP 4
git hub connection 
<pre>
  <code id="example-code">
    ls -al ~/.ssh
  </code>
</pre>
<pre>
  <code id="example-code">
    ssh-keygen -t rsa -b 4096 -C "github@smtech24.com"
  </code>
</pre>
<pre>
  <code id="example-code">
    eval "$(ssh-agent -s)"
  </code>
</pre>
<pre>
  <code id="example-code">
    cat ~/.ssh/id_rsa.pub
  </code>
</pre>
Then copy the ssh key and pas it on your SSH and GPG kesy on your git hub clik new ssh key and write project name and past the key

<pre>
  <code id="example-code">
   ssh -T git@github.com
  </code>
</pre>

### STEP 5
Setup nginx 
<pre>
  <code id="example-code">
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    sudo ufw reload
  </code>
</pre>
