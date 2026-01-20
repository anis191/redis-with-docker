# 📒 Redis Sorted Sets (ZSET)

Redis **Sorted Set** হলো **unique elements + score** এর collection,
যেখানে elements গুলো **score অনুযায়ী automatically sorted** থাকে।

👉 Sorted Set ব্যবহার করা হয় যখন:

* Ranking দরকার
* Leaderboard বানাতে হবে
* Priority / score based data handle করতে হবে

---

## 🔹 Sorted Set কী?

* Data type: **ZSET**
* Structure:

```
member → score
```

* Member: unique (string)
* Score: number (int / float)
* Sorting হয় **score অনুযায়ী**

Example:

```
leaderboard → {
  "user1": 120,
  "user2": 95,
  "user3": 150
}
```

---

## 1️⃣ `ZADD` — Add members with score

### Example: Game leaderboard

```bash
ZADD leaderboard 120 user1
ZADD leaderboard 95 user2
ZADD leaderboard 150 user3
```

👉 Same member আবার add করলে **score update হয়**

```bash
ZADD leaderboard 180 user1
```

---

## 2️⃣ `ZRANGE` — Get sorted members (ascending)

```bash
ZRANGE leaderboard 0 -1
```

📌 Output:

```
user2
user1
user3
```

👉 Lowest score → highest score

---

## 3️⃣ Get score with members — `WITHSCORES`

```bash
ZRANGE leaderboard 0 -1 WITHSCORES
```

📌 Output:

```
user2 95
user1 180
user3 150
```

---

## 4️⃣ Descending order — `ZREVRANGE`

```bash
ZREVRANGE leaderboard 0 -1 WITHSCORES
```

📌 Output:

```
user1 180
user3 150
user2 95
```

👉 Leaderboard-এ সবচেয়ে বেশি ব্যবহৃত

---

## 5️⃣ Get member score — `ZSCORE`

```bash
ZSCORE leaderboard user1
```

📌 Output:

```
"180"
```

---

## 6️⃣ Get rank of a member

### Ascending rank — `ZRANK`

```bash
ZRANK leaderboard user1
```

📌 Output:

```
(integer) 1
```

### Descending rank — `ZREVRANK`

```bash
ZREVRANK leaderboard user1
```

📌 Output:

```
(integer) 0
```

👉 Rank zero-based (0 = top)

---

## 7️⃣ Increment score — `ZINCRBY`

```bash
ZINCRBY leaderboard 20 user3
```

📌 Output:

```
"170"
```

👉 Score update + re-sort automatically

---

## 8️⃣ Range by score — `ZRANGEBYSCORE`

```bash
ZRANGEBYSCORE leaderboard 100 200
```

📌 Output:

```
user1
user3
```

👉 Score-based filtering

---

## 9️⃣ Remove members — `ZREM`

```bash
ZREM leaderboard user2
```

---

## 🔟 Real-World Use Cases

### 🔹 Game Leaderboard

```bash
ZADD game:score 500 player1
ZINCRBY game:score 50 player1
ZREVRANGE game:score 0 9 WITHSCORES
```

---

### 🔹 Student Ranking

```bash
ZADD exam:rank 85 student1
ZADD exam:rank 92 student2
```

---

### 🔹 Priority Queue

```bash
ZADD tasks 1 low_task
ZADD tasks 10 high_task
ZREVRANGE tasks 0 -1
```

---

### 🔹 Trending Posts

```bash
ZINCRBY post:trending 1 post1
```

---

## 📌 Important Commands Summary

| Command         | Purpose           |
| --------------- | ----------------- |
| `ZADD`          | Add/update member |
| `ZRANGE`        | Ascending order   |
| `ZREVRANGE`     | Descending        |
| `ZSCORE`        | Get score         |
| `ZRANK`         | Asc rank          |
| `ZREVRANK`      | Desc rank         |
| `ZINCRBY`       | Increment score   |
| `ZRANGEBYSCORE` | Filter by score   |
| `ZREM`          | Remove member     |

---

## 🧠 Learning Tips

1. Think **member + score**
2. Always remember: **sorted by score**
3. Use `ZREVRANGE` for leaderboard
4. Rank starts from **0**

---

## ✅ Key Takeaway

* Sorted Sets = Sets + ordering
* Unique members
* Automatic sorting
* Perfect for ranking systems

---