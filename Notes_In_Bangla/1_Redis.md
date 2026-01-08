## 🧠 Redis কী?

**Redis = Remote Dictionary Server**

সহজ ভাষায় বললে
Redis হলো **একটা super fast ডেটা স্টোর**, যেখানে ডেটা **RAM (মেমোরি)**-তে থাকে।

---

## ✨ Redis-এর মূল বৈশিষ্ট্য

* **In-memory database** (RAM-এ ডেটা থাকে)
* **Key – Value based**
* খুবই **দ্রুত (extremely fast)**

---

## 🧩 সহজ উদাহরণ

```
key → value
```

বাস্তব উদাহরণ:

```
"user:1:name" → "Anisul"
```

মানে,

* `user:1:name` = key
* `"Anisul"` = value

---

## ⚖️ Redis কেন আলাদা?

চল, MySQL-এর সাথে সহজভাবে তুলনা করি

| বিষয়      | MySQL             | Redis                   |
| --------- | ----------------- | ----------------------- |
| ডেটা থাকে | Disk (হার্ডডিস্ক) | RAM (মেমোরি)            |
| Speed     | ধীর               | খুব দ্রুত               |
| Query     | SQL               | Simple command          |
| Structure | Table, Row        | Key–Value               |
| Use case  | Permanent data    | Cache, session, counter |

**Redis কখনোই MySQL/PostgreSQL-এর replacement না**
Redis হলো **helper / support system**

---

## 🌍 Redis কোথায় ব্যবহার হয়? (Real-life Use)

তুমি যেহেতু **Django / Backend** জানো, এগুলো খুব important

---

### 🚀 1) Caching (সবচেয়ে common)

* Homepage cache
* API response cache

Result: Website অনেক fast হয়

---

### 🔐 2) Session Management

* User login session
* JWT / token store

Database hit কমে যায়

---

### 🔢 3) Counters

* Page views
* Likes
* Downloads

```bash
INCR page_views
```

No race condition (safe)

---

### 📬 4) Queue / Background Jobs

* Email sending
* Notifications
* Task queue (Celery + Redis)

---

### ⚡ 5) Real-time System

* Chat application
* Online users count
* Live notifications

---

## 🗄️ Redis কি Database?

Traditional relational database না
**NoSQL + In-memory data store**

এখানে **কিছু নেই**

* Table নেই
* Row নেই
* Column নেই

শুধু আছে

```
Key → Value
```

কিন্তু **value শুধু string না**

---

## 🧱 Redis Data Types

Redis আসলে একটা **Data Structure Server**

Value হতে পারে:

* String
* List
* Set
* Hash
* Sorted Set
* Stream

এজন্য Redis অনেক powerful

---

## 💾 Redis কি Permanent Storage?

এখানে সবাই confuse হয়

Redis মূলত **RAM-based**
কিন্তু **Disk-এ save করা যায়**

### দুইভাবে save হয়:

1. **RDB** – Snapshot নেয়
2. **AOF** – Command log রাখে

Important:

* Redis = speed first
* Permanent data = PostgreSQL / MySQL

Redis **main DB না**, support system

---

## 🧵 Redis Single-threaded কেন?

Redis সাধারণত **single-threaded**
তবুও এটা খুব fast

### কেন fast?

* No locking overhead
* Simple commands
* RAM access

Result:

* Predictable performance
* Stable & reliable

---

## 🖥️ Redis CLI কী?

Redis-এর সাথে কথা বলার command tool

```bash
redis-cli
```

কিছু basic command:

```bash
SET name Anisul
GET name
DEL name
```

---

## ⭐ Important Concept

**Redis = Data Structure Server**

মানে:

* শুধু string store করে না
* Data structure বুঝে কাজ করে
* Atomic operation দেয় (thread-safe)

---

## 🧠 এক লাইনে Redis

> **Redis হলো RAM-based super fast key-value data structure store, যেটা caching, session, counter আর real-time system-এর জন্য ব্যবহার হয়।**

---
