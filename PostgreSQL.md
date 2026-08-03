## 1. Installation process

1. **Update system packages**

```bash
sudo apt update && sudo apt upgrade -y
```

1. **Install postgresql**

```bash
sudo apt install -y postgresql postgresql-contrib
```

1. Verify:

```bash
psql --version
```

1. **Check service:**

```bash
sudo systemctl status postgresql
```

1. **Enable it on boot:**

```bash
sudo systemctl enable postgresql
```

1. **Now test the psql is running or not and log in to the postgres user.**

```bash
sudo -u postgres psql
```

## 2. Check PostgreSQL is listening on all interfaces

1. **Edit:**

```bash
sudo nano /etc/postgresql/*/main/postgresql.conf
```

1. **Find:**

```bash
#listen_addresses = 'localhost'
```

1. **Change it to:**

```bash
listen_addresses = '*'
```

## 3. Allow remote connections

1. **Edit:**

```bash
sudo nano /etc/postgresql/*/main/pg_hba.conf
```

1. **Add:**

```bash
host    all    all    0.0.0.0/0    scram-sha-256
```

Or restrict it to your own IP for better security.

## 4. Open port 5432 in the firewall

1. **If using UFW:**

```bash
sudo ufw allow 5432/tcp
sudo ufw reload
```

1. **Restart:**

```bash
sudo systemctlrestart postgresql
```

### 5. Enter to PostgreSQL shell-

```
sudo-u postgres psql
```

---

### 6. Create new user

> **Note:** `#` কোনো সমস্যা না, তাই password single quote (`'`) এর মধ্যে রাখবে।

```sql
CREATEUSER rantpostWITH PASSWORD'rantpost#brendaspace999';
```

---

### ৩. Database তৈরি করো

```sql
CREATE DATABASE rantpost OWNER rantpost;
```

---

### ৪. সব Privilege দাও

```sql
GRANTALLPRIVILEGESON DATABASE rantpostTO rantpost;
```

---

### ৫. বের হয়ে আসো

```sql
\q
```

---

## Test করে দেখো

```sql
psql"postgresql://rantpost:rantpost#brendaspace999@YOUR_VPS_IP:5432/rantpost"
```

**যদি `#` এর কারণে URL parse error পাও**, তাহলে password URL encode করতে হবে।

`#` → `%23`

তাহলে connection string হবে:

```sql
postgresql://rantpost:rantpost%23brendaspace999@YOUR_VPS_IP:5432/rantpost
```

অথবা এভাবে connect করলে URL encoding লাগবে না:

```bash
psql-h YOUR_VPS_IP-U rantpost-d rantpost
```

তারপর password চাইলে দেবে:

```bash
rantpost#password
```

এটাই বেশি নিরাপদ এবং ঝামেলামুক্ত।
