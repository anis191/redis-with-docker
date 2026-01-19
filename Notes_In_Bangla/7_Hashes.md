# 📕 Redis Hashes

Redis **Hash** হলো **field–value pair এর collection**।
এটা দেখতে অনেকটা **object / dictionary / map** এর মতো।

👉 Hash ব্যবহার করা হয় যখন:

* একটি object-এর multiple properties থাকে
* সব data একসাথে কিন্তু structured ভাবে রাখতে চাই

---

## 🔹 Redis Hash কী?

* Data type: **Hash**
* Structure:

  ```
  key → { field : value }
  ```
* Very memory-efficient (small objects এর জন্য)

Example:

```
user:1 → {
  name: "Anisul",
  email: "anis@gmail.com",
  age: "23"
}
```

---

## 1️⃣ `HSET` — Field–value set করা

### Example: User profile

```bash
HSET user:1 name "Anisul" email "anis@gmail.com" age 23
```

📌 Output:

```
(integer) 3
```

🔍 Explanation:

* `user:1` → hash key
* `name`, `email`, `age` → fields
* Value সবসময় string হিসেবে store হয়

---

## 2️⃣ `HGET` — Single field get করা

```bash
HGET user:1 name
```

📌 Output:

```
"Anisul"
```

---

## 3️⃣ `HGETALL` — পুরো hash read করা

```bash
HGETALL user:1
```

📌 Output:

```
1) "name"
2) "Anisul"
3) "email"
4) "anis@gmail.com"
5) "age"
6) "23"
```

👉 Debugging ও learning-এর জন্য খুব useful

---

## 4️⃣ `HMGET` — Multiple fields get

```bash
HMGET user:1 name age
```

📌 Output:

```
1) "Anisul"
2) "23"
```

---

## 5️⃣ `HDEL` — Field delete করা

```bash
HDEL user:1 age
```

📌 Output:

```
(integer) 1
```

```bash
HGETALL user:1
```

---

## 6️⃣ Check field exists — `HEXISTS`

```bash
HEXISTS user:1 email
```

📌 Output:

```
(integer) 1
```

---

## 7️⃣ Increment numeric field — `HINCRBY`

```bash
HSET user:1 login_count 0
HINCRBY user:1 login_count 1
```

📌 Output:

```
(integer) 1
```

👉 Counter রাখার জন্য খুব common

---

## 8️⃣ Get number of fields — `HLEN`

```bash
HLEN user:1
```

📌 Output:

```
(integer) 3
```

---

## 9️⃣ Get only keys or values

```bash
HKEYS user:1
```

```bash
HVALS user:1
```

👉 Learning/debugging-এ helpful

---

## 🔟 Real-World Use Cases

### 🔹 User Profile

```bash
HSET user:100 name "Anisul" email "anis@gmail.com" role "admin"
```

---

### 🔹 Session Store

```bash
HSET session:abc123 user_id 100 ip "127.0.0.1"
```

---

### 🔹 Product Info

```bash
HSET product:1 name "Laptop" price 65000 stock 10
```

---

### 🔹 Login Attempts

```bash
HINCRBY user:1 failed_login 1
```

---

## 📌 Redis Hash vs JSON vs String

| Feature             | Hash        | JSON   | String |
| ------------------- | ----------- | ------ | ------ |
| Structure           | Flat fields | Nested | Plain  |
| Update single field | ✅           | ✅      | ❌      |
| Nested data         | ❌           | ✅      | ❌      |
| Memory efficient    | ✅           | ❌      | ✅      |

👉 Small objects → **Hash**
👉 Complex/nested → **JSON**

---

## 🧠 Learning Tips

1. Hash = database row এর মতো ভাবো
2. Always use **namespaced keys** (`user:1`)
3. Use `HINCRBY` for counters
4. Prefer Hash over JSON for **simple objects**

---

## ✅ Key Takeaway

* Redis Hash = object-like structure
* Perfect for user profiles, configs, sessions
* Field-level update possible
* Fast & memory-efficient

---