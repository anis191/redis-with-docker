# 📘 Redis Sets

Redis **Set** হলো **unordered collection of unique strings**।

👉 Set-এ:

* ❌ Duplicate নেই
* ❌ Order নেই
* ✅ Very fast operations (O(1))

---

## 🔹 Redis Set কী?

* Data type: **Set**
* Elements: **Unique strings**
* Order: **Unordered**

Think like:

```
{ "python", "django", "redis" }
```

---

## 1️⃣ `SADD` — Add elements to Set

### Example: Skills list

```bash
SADD skills "python"
SADD skills "django"
SADD skills "redis"
```

```bash
SADD skills "python"
```

📌 Output:

```
(integer) 0
```

🔍 Explanation:

* `python` আগে থেকেই ছিল
* Duplicate add হয়নি

---

## 2️⃣ `SMEMBERS` — Get all elements

```bash
SMEMBERS skills
```

📌 Output:

```
"python"
"django"
"redis"
```

⚠️ Order random হতে পারে (unordered)

---

## 3️⃣ `SCARD` — Count elements

```bash
SCARD skills
```

📌 Output:

```
(integer) 3
```

👉 Unique item count করার জন্য perfect

---

## 4️⃣ `SISMEMBER` — Check existence

```bash
SISMEMBER skills "django"
```

📌 Output:

```
(integer) 1
```

```bash
SISMEMBER skills "java"
```

📌 Output:

```
(integer) 0
```

👉 Membership check খুব fast

---

## 5️⃣ `SREM` — Remove element

```bash
SREM skills "redis"
```

```bash
SMEMBERS skills
```

📌 Output:

```
"python"
"django"
```

---

## 6️⃣ Set Operations (Powerful Feature)

### 🔹 Union (`SUNION`)

```bash
SADD backend "django" "laravel"
SADD frontend "html" "css" "js"
```

```bash
SUNION backend frontend
```

📌 Output:

```
"django"
"laravel"
"html"
"css"
"js"
```

👉 Combine multiple sets

---

### 🔹 Intersection (`SINTER`)

```bash
SINTER backend skills
```

📌 Output:

```
"django"
```

👉 Common elements বের করতে

---

### 🔹 Difference (`SDIFF`)

```bash
SDIFF skills backend
```

📌 Output:

```
"python"
```

👉 A − B

---

## 7️⃣ Random element (`SRANDMEMBER`)

```bash
SRANDMEMBER skills
```

📌 Output:

```
"python"
```

👉 Use case:

* Random winner
* Recommendation

---

## 8️⃣ Pop random element (`SPOP`)

```bash
SPOP skills
```

📌 Output:

```
"django"
```

👉 Element remove + return

---

## 9️⃣ Real-world Use Cases

### 🔹 Unique Users Online

```bash
SADD online_users user1
SADD online_users user2
SCARD online_users
```

---

### 🔹 Post Likes (No duplicate)

```bash
SADD post:1:likes user1
SADD post:1:likes user1   # ignored
```

---

### 🔹 Tags System

```bash
SADD post:1:tags "redis" "backend" "database"
```

---

### 🔹 Friends / Followers

```bash
SADD user:1:followers user2 user3
```

---

## 📌 Commands Summary

| Command       | Purpose      |
| ------------- | ------------ |
| `SADD`        | Add elements |
| `SMEMBERS`    | Get all      |
| `SCARD`       | Count        |
| `SISMEMBER`   | Check exists |
| `SREM`        | Remove       |
| `SUNION`      | Union        |
| `SINTER`      | Intersection |
| `SDIFF`       | Difference   |
| `SRANDMEMBER` | Random get   |
| `SPOP`        | Random pop   |

---

## 🧠 Learning Tips

1. Use Sets when **duplicates must not exist**
2. Don’t rely on order
3. Practice union/intersection logic
4. Ideal for **likes, followers, tags**

---

## ✅ Key Takeaway

* Redis Sets = unique + unordered
* Very fast membership check
* Powerful mathematical set operations
* Perfect for real-world systems

---