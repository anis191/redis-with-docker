# 📔 Redis Streams

Redis **Streams** হলো **append-only log / event stream data structure**।
এটি ব্যবহার করা হয় যখন **events, messages, logs, real-time data flow** handle করতে হয়।

👉 সহজভাবে:

* Data শুধু **append** হয়
* Order সবসময় **maintained**
* Consumer group দিয়ে multiple consumers handle করা যায়

---

## 🔹 Redis Stream কী?

* Data type: **Stream**
* Structure:

```
stream_key → [
  ID → { field : value },
  ID → { field : value },
  ...
]
```

Example:

```
orders → [
  1700000000000-0 → { user:1, item:"book" }
]
```

---

## 1️⃣ Add data to Stream — `XADD`

### Example: Order events

```bash
XADD orders * user_id 1 product "book" price 350
```

📌 Output:

```
1700001234567-0
```

🔍 Explanation:

* `orders` → stream key
* `*` → auto-generated ID (timestamp-based)
* field–value pairs → event data

---

## 2️⃣ Read stream entries — `XRANGE`

```bash
XRANGE orders - +
```

📌 Output (example):

```
1) 1700001234567-0
   1) "user_id" "1"
   2) "product" "book"
   3) "price" "350"
```

👉 `- +` মানে → from start to end

---

## 3️⃣ Read last N messages

```bash
XREVRANGE orders + - COUNT 1
```

👉 Latest event দেখতে খুব useful

---

## 4️⃣ Consumer Groups — Core Feature

Streams-এর সবচেয়ে powerful feature হলো **Consumer Groups**।

### Create consumer group

```bash
XGROUP CREATE orders order_group $
```

🔍 `$` → start from new messages only

---

## 5️⃣ Read as consumer — `XREADGROUP`

```bash
XREADGROUP GROUP order_group consumer1 COUNT 1 BLOCK 0 STREAMS orders >
```

🔍 Explanation:

* `consumer1` → consumer name
* `>` → new messages only
* `BLOCK 0` → wait forever

---

## 6️⃣ Acknowledge message — `XACK`

```bash
XACK orders order_group 1700001234567-0
```

👉 Message processed mark করা হয়

---

## 7️⃣ Pending messages — `XPENDING`

```bash
XPENDING orders order_group
```

👉 Crash হলে unprocessed messages দেখা যায়

---

## 8️⃣ Real-World Use Cases

### 🔹 Order Processing System

* Web app → order event push
* Worker → order process

---

### 🔹 Chat System

```bash
XADD chat:room1 * user "anisul" msg "Hello"
```

---

### 🔹 Logging System

```bash
XADD logs * level "error" msg "DB down"
```

---

### 🔹 IoT / Sensor Data

```bash
XADD sensor:temp * value 32.5
```

---

## 9️⃣ Stream vs List vs Pub/Sub

| Feature         | Stream           | List  | Pub/Sub      |
| --------------- | ---------------- | ----- | ------------ |
| Persistence     | ✅                | ✅     | ❌            |
| Consumer Groups | ✅                | ❌     | ❌            |
| Replay          | ✅                | ❌     | ❌            |
| Use case        | Event processing | Queue | Live message |

👉 Reliable messaging → **Streams**
👉 Simple queue → Lists
👉 Live broadcast → Pub/Sub

---

## 🔟 Important Commands Summary

| Command      | Purpose         |
| ------------ | --------------- |
| `XADD`       | Add event       |
| `XRANGE`     | Read stream     |
| `XREVRANGE`  | Reverse read    |
| `XGROUP`     | Create group    |
| `XREADGROUP` | Consume message |
| `XACK`       | Acknowledge     |
| `XPENDING`   | Pending list    |

---

## 🧠 Learning Tips

1. Think **event-driven**
2. Always acknowledge (`XACK`)
3. Use consumer groups for scalability
4. Streams = reliable message queue

---

## ✅ Key Takeaway

* Redis Streams = persistent event log
* Ordered & reliable
* Perfect for microservices & background workers
* More powerful than Lists & Pub/Sub

---