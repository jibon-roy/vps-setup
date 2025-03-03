# Mongodb setup on VPS

## 📦 Installation & Setup

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

