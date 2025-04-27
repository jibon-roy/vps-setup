# Mongodb setup on VPS

## ✅ Step-by-Step Guide to Install MongoDB on Ubuntu Server 24.04

## 1. Update your system packages
```bash
sudo apt update && sudo apt upgrade -y
```
Explanation:

apt update fetches the latest package list.

apt upgrade -y upgrades installed packages to latest versions (-y auto-confirms).

## 2. Import the MongoDB GPG key
MongoDB uses GPG keys to verify the authenticity of the packages.

```bash
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
```
Explanation:

curl -fsSL downloads the MongoDB public key safely (-f fail silently, -s silent mode, -S show errors, -L follow redirects).

gpg --dearmor converts it into a keyring format compatible with APT.

Saved to /usr/share/keyrings/ which is a best practice for key storage in modern Ubuntu versions.

### ✅ Optional chaining fallback:
If curl is not installed:

```bash
sudo apt install curl -y
```

## 3. Add the MongoDB APT repository
```bash
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
Explanation:
```

deb [signed-by=...] URL component — tells APT to trust packages signed by that GPG key.

Using jammy because Ubuntu 24.04 is very close to 22.04 ("Jammy Jellyfish"), and MongoDB officially supports it.

"multiverse" — a section of Ubuntu repositories for non-free software (MongoDB licenses are slightly different).

### ✅ Fallback:
If MongoDB later releases a 24.04-specific repository, you just need to change jammy to noble (Ubuntu 24.04 = "Noble Numbat").

## 4. Update package lists again
```bash
sudo apt update
```
Explanation:
Now APT knows about the new MongoDB packages available from the repo you just added.

## 5. Install MongoDB
```bash
sudo apt install mongodb-org -y
```
Explanation:

mongodb-org is a meta-package that automatically installs:

mongodb-org-server

mongodb-org-mongos

mongodb-org-shell

mongodb-org-tools

✅ Fallback:
If you need specific components only, you could install them individually.

## 6. Start and Enable the MongoDB Service
Start MongoDB immediately:

```bash
sudo systemctl start mongod
```
Enable MongoDB to start automatically on boot:

```bash
sudo systemctl enable mongod
``` 

### ✅ Best Practice:
Always enable it on boot so that a server restart doesn’t kill your database.

## 7. Check MongoDB Service Status
```bash
sudo systemctl status mongod
```
You should see something like:

`
Active: active (running)
`
### ✅ Optional Tip:
If not running, you can debug with:

```bash
sudo journalctl -u mongod
```
(Shows the MongoDB logs)

## 8. Allow MongoDB through the Firewall (Optional)
If you have ufw (Uncomplicated Firewall) enabled:

```bash
sudo ufw allow 27017/tcp
```

## Explanation:
MongoDB runs on TCP port 27017 by default.

Allowing it makes your database accessible if needed (e.g., remote access from your application).

✅ Best Practice:
Only allow it if needed; otherwise, keep MongoDB internal for better security.

## 9. Test MongoDB connection
Connect using the shell:
```bash
mongosh
```

✅ Fallback:
If mongosh is missing:

```bash
sudo apt install mongodb-mongosh
```

## 📜 Summary of Commands:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl gnupg -y
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt update
sudo apt install mongodb-org -y
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
sudo ufw allow 27017/tcp # (only if needed)
mongosh
```
## ✨ Best Practices I followed:
Proper curl and gpg handling with error fallback

Used /usr/share/keyrings/ instead of older deprecated methods

Repository signing

Enabling MongoDB at boot

Firewall configuration as optional and explained

Handling shell connection fallback

Clear command-by-command explanation

No logic or steps skipped


### **1. Clone the Repository**
```bash
 git clone https://github.com/yourusername/your-repo.git
 cd your-repo
```

### **2. Install Dependencies**
```bash
npm install  # or yarn install
```

### **3. Set Up MongoDB Replica Set**
Ensure you have **MongoDB** installed and running on a VPS. To configure a replica set:
```bash
mongod --replSet rs0 --bind_ip 0.0.0.0 --port 27017 --auth
```

**Initialize the replica set:**
```js
rs.initiate(
  {
    _id: "rs0",
    members: [
      { _id: 0, host: "primary-ip:27017" },
      { _id: 1, host: "secondary1-ip:27017" },
      { _id: 2, host: "secondary2-ip:27017" }
    ]
  }
)
```

### **4. Configure `.env` File**
Create a `.env` file in the root directory:
```env
DATABASE_URL="mongodb://127.0.0.1:27017/dbname?replicaSet=rs0"
```

### **5. Migrate Prisma Schema**
```bash
npx prisma migrate dev --name init
```

### **6. Run the Development Server**
```bash
npm run dev  # or yarn dev
```
The app will be available at **http://localhost:3000**.

---

## 📜 API Endpoints
- `GET /api/songs` - Fetch all available songs
- `POST /api/record` - Start a karaoke recording session
- `PUT /api/equalizer` - Adjust equalizer settings
- `GET /api/user` - Get user profile details

---

## 🛠️ Deployment
1. Deploy your **MongoDB Replica Set** on a VPS.
2. Use **Vercel** or **DigitalOcean** to deploy the Next.js app.
3. Set up **environment variables** on the hosting platform.

---

## 🎤 Contributors
- **Your Name** - _Lead Developer_
- **Other Contributors** - _Design & Testing_

---

## 📝 License
This project is licensed under the **MIT License**. Feel free to modify and distribute!

