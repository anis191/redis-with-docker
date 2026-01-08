# 🧱 Redis Data Types

Redis হলো **Key–Value Data Structure Server**।  
Key দিয়ে value access করি। Value বিভিন্ন ধরনের হতে পারে।  

---

## 1️⃣ String

- একক value (যেমন text বা number)  
- Example:

```bash
SET name "Anisul"
GET name  # আউটপুট: Anisul
````

* Use: Simple caching, Counters (page views, likes)

---

## 2️⃣ List

* Ordered (যে order এ add করি, সেই order মেনে চলে)
* Duplicates allowed
* Example:

```bash
LPUSH fruits "Apple"   # left থেকে add
RPUSH fruits "Banana"  # right থেকে add
LRANGE fruits 0 -1     # সব item দেখাবে
```

* Use: Queues, Recent items

---

## 3️⃣ Set

* Unordered, **unique** values
* Example:

```bash
SADD colors "Red"
SADD colors "Blue"
SMEMBERS colors
```

* Use: Tags, Unique items

---

## 4️⃣ Hash

* Key → Field → Value (যেমন small object বা dict)
* Example:

```bash
HSET user:1 name "Anisul"
HSET user:1 age 22
HGETALL user:1
```

* Use: User profile, Object storage

---

## 5️⃣ Sorted Set (ZSet)

* Unique member + score (auto sorted by score)
* Example:

```bash
ZADD leaderboard 100 "Alice"
ZADD leaderboard 200 "Bob"
ZRANGE leaderboard 0 -1 WITHSCORES
```

* Use: Leaderboards, Ranking

---

## 6️⃣ Stream

* Time-based log, append-only
* Example:

```bash
XADD mystream * sensor 1 temp 22
XREAD STREAMS mystream 0
```

* Use: Real-time events, Chat messages

---

## 📌 Summary Table (দ্রুত মনে রাখার জন্য)

| Type       | Order | Duplicate | Use                  |
| ---------- | ----- | --------- | -------------------- |
| String     | No    | Yes       | Cache, Counter       |
| List       | Yes   | Yes       | Queue, Recent        |
| Set        | No    | No        | Tags, Unique items   |
| Hash       | No    | Yes       | Object, Profile      |
| Sorted Set | Yes   | No        | Leaderboard, Ranking |
| Stream     | Yes   | Yes       | Events, Chat         |

---