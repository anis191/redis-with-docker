# 📙 Redis Lists

Redis List হলো **ordered collection of strings**।
এটি কাজ করে **Linked List**–এর মতো।

👉 List-এ:

* Order থাকে (left → right)
* Duplicate allowed
* Very fast push/pop (O(1))

---

## 🔹 Redis List কী?

* Data type: **List**
* Structure: **Linked List**
* Elements: **String**
* Direction: **Left (head)** & **Right (tail)**

👉 Think like:

```
[left]  A → B → C  [right]
```

---

## 1️⃣ `LPUSH` & `RPUSH` (Insert elements)

### Example: Task queue

```bash
LPUSH tasks "task1"
LPUSH tasks "task2"
LPUSH tasks "task3"
```

List becomes:

```
task3 → task2 → task1
```

```bash
RPUSH tasks "task4"
```

List becomes:

```
task3 → task2 → task1 → task4
```

---

## 2️⃣ `LRANGE` (Read list items)

```bash
LRANGE tasks 0 -1
```

📌 Output:

```
1) "task3"
2) "task2"
3) "task1"
4) "task4"
```

🔍 Explanation:

* `0` = first index
* `-1` = last index
* Most used command for lists

---

## 3️⃣ `LLEN` (List length)

```bash
LLEN tasks
```

📌 Output:

```
(integer) 4
```

👉 Useful for:

* Queue size
* Pending tasks count

---

## 4️⃣ `LPOP` & `RPOP` (Remove elements)

```bash
LPOP tasks
```

📌 Output:

```
"task3"
```

```bash
RPOP tasks
```

📌 Output:

```
"task4"
```

👉 Queue / Stack implement করতে খুব useful।

---

## 5️⃣ Redis List as Stack (LIFO)

```bash
LPUSH stack "A"
LPUSH stack "B"
LPUSH stack "C"
```

```bash
LPOP stack
```

📌 Output:

```
"C"
```

👉 Last In First Out (Stack)

---

## 6️⃣ Redis List as Queue (FIFO)

```bash
RPUSH queue "job1"
RPUSH queue "job2"
```

```bash
LPOP queue
```

📌 Output:

```
"job1"
```

👉 First In First Out (Queue)

---

## 7️⃣ Blocking operations (`BLPOP`)

### Example: Worker waiting for job

```bash
BLPOP jobs 0
```

🔍 Explanation:

* `0` → wait forever
* Redis blocks until data arrives

Another terminal:

```bash
RPUSH jobs "email_task"
```

📌 Output:

```
1) "jobs"
2) "email_task"
```

👉 Used in:

* Background jobs
* Message queues

---

## 8️⃣ Update list item (`LSET`)

```bash
LSET tasks 1 "updated_task"
```

```bash
LRANGE tasks 0 -1
```

📌 Output:

```
task3
updated_task
task1
```

⚠️ Index out of range হলে error দেয়।

---

## 9️⃣ Remove by value (`LREM`)

```bash
LREM tasks 1 "task1"
```

📌 Output:

```
(integer) 1
```

👉 Removes matching value(s)

---

## 🔟 Real-World Use Cases

### 🔹 Task Queue

```bash
RPUSH task_queue "send_email"
RPUSH task_queue "generate_report"
LPOP task_queue
```

### 🔹 Chat Messages

```bash
RPUSH chat:room1 "Hi"
RPUSH chat:room1 "Hello"
LRANGE chat:room1 0 -1
```

### 🔹 Recent Activity

```bash
LPUSH recent:users "anisul"
LPUSH recent:users "rafi"
LRANGE recent:users 0 9
```

---

## 📌 Important Commands Summary

| Command  | Purpose      |
| -------- | ------------ |
| `LPUSH`  | Left insert  |
| `RPUSH`  | Right insert |
| `LPOP`   | Left remove  |
| `RPOP`   | Right remove |
| `LRANGE` | Read list    |
| `LLEN`   | Length       |
| `BLPOP`  | Blocking pop |
| `LSET`   | Update index |
| `LREM`   | Remove value |

---

## 🧠 Learning Tips

1. Practice **left vs right** carefully
2. Implement **stack & queue** manually
3. Use `BLPOP` to understand **real worker systems**
4. Always check list length with `LLEN`

---

## ✅ Key Takeaway

* Redis Lists are perfect for:

  * Queues
  * Stacks
  * Task systems
  * Chat messages
* Very fast
* Order is preserved
* Easy to use

---