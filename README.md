# Redis Stack + RedisInsight (with Docker)

This repository is created as a **learning-focused setup guide** for running **Redis Stack** and **RedisInsight** using Docker.

---

## 📌 Project Overview

**Redis Stack + RedisInsight (Docker-based setup)** provides a quick and reliable way to run Redis locally with additional modules like **RedisJSON, RediSearch, RedisGraph**, and a powerful GUI (**RedisInsight**) for visualization and management.

This README includes:

* Quick test setup
* RedisInsight GUI connection steps
* CLI testing
* Recommended `docker-compose` configuration
* Troubleshooting tips

---

## 🧰 Prerequisites

* Docker installed
* Docker Compose (recommended)
* Free local ports: **6379** (Redis) and **5540** (RedisInsight)

---

## ⚡ Quick Setup (for testing)

Run the following commands if you just want to test Redis quickly. **Password is loaded from `.env` (not hard-coded).**

```bash
# Create volumes
docker volume create redisstack_data
docker volume create redisinsight_data

# Run Redis Stack
docker run -d --name redis-stack \
  -p 6379:6379 \
  -v redisstack_data:/data \
  --env-file .env \
  redis/redis-stack:latest

# Run RedisInsight
docker run -d --name redis-insight \
  -p 5540:5540 \
  -v redisinsight_data:/data \
  redis/redisinsight:latest
```

Once running:

* Redis → `localhost:6379`
* RedisInsight → `http://127.0.0.1:5540`

---

## 🖥️ Connect Using RedisInsight (GUI)

1. Open browser → `http://127.0.0.1:5540`
2. First time: **I accept → Get Started**
3. Click **Add Redis Database → Connection Settings**

### Connection Details

| Field          | Value                  |
| -------------- | ---------------------- |
| Database Alias | Any name               |
| Host           | `host.docker.internal` |
| Port           | `6379`                 |
| Password       | From `.env`            |
| Username       | Leave empty            |
| TLS            | Disabled               |

Click **Add Database** → Done ✅

> 🐧 **Linux users:** If `host.docker.internal` doesn’t work, use `172.17.0.1` or your Docker host IP.

---

## 🧪 First Test (Workbench / CLI tab)

Run the following commands inside RedisInsight:

```redis
SET name "Anisul Alam"
SET city "Chittagong"
SET job "Python Developer"
INCR visitor_count
```

Expected output:

```
1) "OK"
2) "OK"
3) "OK"
4) (integer) 1
```

---

## 🖥️ Test Using Docker CLI

```bash
docker exec -it redis-stack redis-cli
```

Inside Redis CLI:

```redis
AUTH <password_from_env>
GET name
GET city
GET job
INCR visitor_count
```

---

## ✅ Recommended Setup (docker-compose)

For long-term development, use Docker Compose.

### `docker-compose.yaml`

```yaml
services:
  redis-stack:
    image: redis/redis-stack:latest
    container_name: redis-stack
    restart: unless-stopped
    ports:
      - "6379:6379"
    volumes:
      - redisstack_data:/data
    environment:
      - REDIS_ARGS=--appendonly yes --requirepass ${REDIS_PASSWORD}

  redis-insight:
    image: redis/redisinsight:latest
    container_name: redis-insight
    restart: unless-stopped
    ports:
      - "5540:5540"
    volumes:
      - redisinsight_data:/data

volumes:
  redisstack_data:
  redisinsight_data:
```

### `.env` file

```env
REDIS_PASSWORD=your_redis_password_here
```

### Commands

```bash
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# Restart (data persists)
docker-compose up -d

# View Redis logs
docker-compose logs -f redis-stack
```

---

## 🛠️ Troubleshooting

| Issue                              | Solution                                    |
| ---------------------------------- | ------------------------------------------- |
| RedisInsight shows blank page      | Wait 30–60 seconds and refresh              |
| `host.docker.internal` not working | Use `172.17.0.1` or Docker host IP          |
| Connection refused                 | Check containers using `docker ps`          |
| Forgot Redis password              | Remove containers and volumes, then restart |

---

## 🔄 Full Reset (Delete Everything)

```bash
docker-compose down -v
# Or manually:
docker volume rm redisstack_data redisinsight_data
```

---

## 🔐 Best Practices

* Always use strong passwords in production
* Enable ACL, TLS, and proper networking when deploying publicly
* Keep volume backups if data persistence matters

---