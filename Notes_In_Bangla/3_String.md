# 📕 Redis String Data Type

Redis-এ **String** হলো সবচেয়ে basic কিন্তু সবচেয়ে powerful data type।
এটি text, number, JSON string, token, counter — সবকিছুই store করতে পারে।

---

## 🔹 Redis String কী?

* Redis String মূলত **byte sequence**
* Maximum size: **512 MB**
* Text, number, serialized object — সব রাখা যায়

👉 Redis-এ প্রায় সব beginner ও production-level কাজই String দিয়ে শুরু হয়।

---

## 1️⃣ Basic `SET` & `GET`

### Example: Simple key-value store

```bash
SET site:name "EventZone"
GET site:name
```

📌 Output:

```
"EventZone"
```

🔍 ব্যাখ্যা:

* `SET` → key ও value store করে
* `GET` → value read করে

👉 Use case:

* App name
* Website title
* Config value

---

## 2️⃣ Key না থাকলে `(nil)`

```bash
GET admin:name
```

📌 Output:

```
(nil)
```

🔍 ব্যাখ্যা:

* Key exist না করলে Redis `(nil)` দেয়
* Error না, simply data নেই

---

## 3️⃣ Namespaced Key (Best Practice)

```bash
SET book:1 "Bangla"
SET book:2 "English"
SET book:3 "Math"
```

```bash
GET book:1
```

📌 Output:

```
"Bangla"
```

🔍 কেন `book:1` ব্যবহার করা ভালো?

* Data organize থাকে
* বড় project-এ conflict হয় না

👉 Real-world example:

* `user:1:name`
* `order:25:status`
* `product:10:price`

---

## 4️⃣ Redis String as Counter (`INCR`)

### Example: User counter

```bash
SET users 0
INCR users
INCR users
INCR users
```

```bash
GET users
```

📌 Output:

```
"3"
```

🔍 ব্যাখ্যা:

* Redis internally string হলেও number হিসেবে handle করে
* `INCR` atomic (thread-safe)

👉 Real use cases:

* Total users
* Page views
* Likes count
* API request count

---

## 5️⃣ `INCRBY` — Increment by N

```bash
INCRBY users 10
```

📌 Output:

```
(integer) 13
```

🔍 ব্যবহার:

* Bulk increment
* Bonus points
* Score system

---

## 6️⃣ String Overwrite Behavior

```bash
SET name "anisul alam"
SET name "anis"
```

```bash
GET name
```

📌 Output:

```
"anis"
```

🔍 গুরুত্বপূর্ণ:

* Redis-এ **same key আবার SET করলে আগের value replace হয়ে যায়**

⚠️ Caution:

* Accidental overwrite হতে পারে

---

## 7️⃣ `NX` — Set only if key DOES NOT exist

```bash
SET username "anisul" NX
```

📌 Possible outputs:

* `OK` → key নতুন
* `(nil)` → key আগে থেকেই ছিল

🔍 Use case:

* Username / email uniqueness
* Duplicate insert prevent করা

👉 Real example:

```bash
SET email:anis@gmail.com 1 NX
```

---

## 8️⃣ `XX` — Set only if key EXISTS

```bash
SET profile:name "Anisul Islam" XX
```

📌 Output:

```
OK
```

🔍 ব্যাখ্যা:

* Key exist না করলে update হবে না
* Safe update operation

👉 Use case:

* Profile update
* Status change
* Cached data refresh

---

## 9️⃣ `NX` + `XX` Comparison

| Option  | কাজ              |
| ------- | ---------------- |
| `NX`    | key না থাকলে set |
| `XX`    | key থাকলে set    |
| default | সবসময় overwrite  |

---

## 🔟 Redis String — Real Project Examples

### 🔹 Login Token Store

```bash
SET auth:token:123 "jwt_token_here"
```

### 🔹 OTP Store

```bash
SET otp:phone:017xxxx 4567
```

### 🔹 Feature Flag

```bash
SET feature:payment true
```

### 🔹 Cache Data

```bash
SET cache:homepage "HTML_CONTENT"
```

---

## 🧠 Important Redis String Notes

* Redis String ≠ only text
* Numbers + text দুটোই store করা যায়
* Counter operations atomic
* Very fast (in-memory)

---

## ✅ Summary

* Redis String = simplest + most used data type
* Supports:

  * Key-value store
  * Counter
  * Conditional set (NX / XX)
* Perfect for:

  * Caching
  * Analytics
  * Auth tokens
  * User metrics

---