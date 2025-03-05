<!DOCTYPE html>
<html>
<body>
  <h1>Install and Configure Nginx for Next.js with PM2 on Ubuntu 22.04</h1>

  <h2>1. Install Nginx</h2>
  <ol>
    <li>Update the package list:
      <pre><code>sudo apt update
sudo apt upgrade -y</code></pre>
    </li>
    <li>Install Nginx:
      <pre><code>sudo apt install nginx -y</code></pre>
    </li>
    <li>Start and enable Nginx:
      <pre><code>sudo systemctl start nginx
sudo systemctl enable nginx</code></pre>
    </li>
  </ol>

  <h2>2. Install Node.js and PM2</h2>
  <ol>
    <li>Install Node.js (LTS version):
      <pre><code>curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs</code></pre>
    </li>
    <li>Install PM2 globally:
      <pre><code>npm install -g pm2</code></pre>
    </li>
    <li>Verify installation:
      <pre><code>node -v
npm -v
pm2 -v</code></pre>
    </li>
  </ol>

  <h2>3. Deploy Your Next.js Project</h2>
  <ol>
     <li> Check UFW status
      <pre><code>sudo ufw status</code></pre>
    </li>
     <li>UFW allow 3000 port
      <pre><code>sudo ufw allow 3000</code></pre>
    </li>
    <li>Reload UFW
      <pre><code>sudo ufw reload</code></pre>
    </li>
     <li> Check UFW status
      <pre><code>sudo ufw status</code></pre>
    </li>
    <li>Build your Next.js project:
      <pre><code>npm run build</code></pre>
    </li>
    <li>Start the application with PM2:
      <pre><code>pm2 start npm --name "nextjs-app" -- start</code></pre>
    </li>
    <li>Save the PM2 process list to start on boot:
      <pre><code>pm2 save
pm2 startup</code></pre>
      Follow the printed instructions to configure the PM2 startup script.
    </li>
  </ol>

  <h2>4. Configure Nginx as a Reverse Proxy</h2>
  <ol>
    <li>Create a new Nginx configuration file for your site:
      <pre><code>sudo nano /etc/nginx/sites-available/nextjs-app</code></pre>
    </li>
    <li>Add the following configuration (replace <code>example.com</code> with your domain or server IP):
      <pre><code>server {
    listen 80;
    server_name example.com www.example.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
  }
</code></pre>
    </li>
    <li>Enable the configuration:
      <pre><code>sudo ln -s /etc/nginx/sites-available/nextjs-app /etc/nginx/sites-enabled/</code></pre>
    </li>
    <li>Test Nginx configuration:
      <pre><code>sudo nginx -t</code></pre>
    </li>
    <li>Restart Nginx:
      <pre><code>sudo systemctl restart nginx</code></pre>
    </li>
  </ol>

  <h2>5. Secure with HTTPS (Optional, Recommended)</h2>
  <ol>
    <li>Install Certbot:
      <pre><code>sudo apt install certbot python3-certbot-nginx -y</code></pre>
    </li>
    <li>Obtain an SSL certificate:
      <pre><code>sudo certbot --nginx -d example.com -d www.example.com</code></pre>
    </li>
    <li>Test certificate renewal:
      <pre><code>sudo certbot renew --dry-run</code></pre>
    </li>
  </ol>

  <h2>6. Verify Setup</h2>
  <ul>
    <li>Visit your domain (<code>http://example.com</code>) to verify that your Next.js project is running.</li>
    <li>Check logs for debugging:
      <ul>
        <li>Nginx logs: <code>/var/log/nginx/error.log</code></li>
        <li>PM2 logs: <code>pm2 logs nextjs-app</code></li>
      </ul>
    </li>
  </ul>

  <p>This setup ensures your Next.js app runs smoothly in production with PM2 process management and Nginx as a reverse proxy for better performance and scalability.</p>
</body>
</html>
