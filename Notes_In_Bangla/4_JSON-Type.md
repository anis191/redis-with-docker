# 📗 Redis JSON Type

Redis JSON হলো **Redis এর একধরনের data type** যা **JSON objects, arrays এবং nested structures** in-memory store করতে দেয়।
RedisJSON module ব্যবহার করলে আমরা **Redis কে একটি NoSQL JSON database** হিসেবে ব্যবহার করতে পারি।

> Pre-requisite: RedisJSON module install থাকতে হবে।
> সাধারণ Redis server default-এ JSON support দেয় না।

---

## 🔹 Redis JSON কী?

* RedisJSON হল একটি **Redis module**
* JSON objects/key-value pairs, arrays, nested data structures store করতে পারে
* Commands prefix হয়: `JSON.*`
* In-memory হওয়ায় খুব fast

---

## 1️⃣ JSON SET — Store JSON data

```bash
JSON.SET user:1 $ '{"name":"anisul","age":23,"email":"anis@gmail.com"}'
```

**Explanation:**

* `user:1` → key
* `$` → root path of JSON (সব সময় JSON root denote করে)
* Value → valid JSON object

```bash
JSON.GET user:1
```

📌 Output:

```json
{"name":"anisul","age":23,"email":"anis@gmail.com"}
```

---

## 2️⃣ Access JSON fields — `JSON.GET path`

```bash
JSON.GET user:1 $.name
```

📌 Output:

```json
"anisul"
```

🔹 `$` = root
🔹 `.name` = field access

```bash
JSON.GET user:1 $.age
```

📌 Output: `23`

---

## 3️⃣ Update JSON fields — `JSON.SET path`

```bash
JSON.SET user:1 $.name '"anisul islam"'
```

📌 Output: `OK`

```bash
JSON.GET user:1 $.name
```

📌 Output: `"anisul islam"`

> Notes:

* Strings must be in double quotes inside `' '`
* Numbers/booleans don’t need extra quotes

---

## 4️⃣ Increment numeric fields — `JSON.NUMINCRBY`

```bash
JSON.NUMINCRBY user:1 $.age 1
```

📌 Output: `24`

🔹 Useful for counters inside JSON, like:

* `cart:items_count`
* `profile:login_attempts`

---

## 5️⃣ Arrays in JSON

```bash
JSON.SET user:2 $ '{"name":"Rafi","hobbies":["football","coding"]}'
```

```bash
JSON.GET user:2 $.hobbies
```

📌 Output:

```json
["football","coding"]
```

### Add item to array — `JSON.ARRAPPEND`

```bash
JSON.ARRAPPEND user:2 $.hobbies '"reading"'
```

```bash
JSON.GET user:2 $.hobbies
```

📌 Output:

```json
["football","coding","reading"]
```

---

## 6️⃣ Delete JSON fields — `JSON.DEL`

```bash
JSON.DEL user:2 $.hobbies[0]
```

📌 Output: `1` (number of elements deleted)

```bash
JSON.GET user:2
```

📌 Output:

```json
{"name":"Rafi","hobbies":["coding","reading"]}
```

---

## 7️⃣ Real-world Use Cases

1. **User Profiles**

```bash
JSON.SET user:100 $ '{"name":"Anisul","email":"anis@gmail.com","age":23}'
```

2. **Shopping Cart**

```bash
JSON.SET cart:1 $ '{"user_id":100,"items":[{"product_id":1,"qty":2}]}'
```

3. **Feature Flags**

```bash
JSON.SET feature:1 $ '{"feature":"dark_mode","enabled":true}'
```

4. **Analytics / Stats**

```bash
JSON.SET stats:page_1 $ '{"views":150,"likes":25}'
JSON.NUMINCRBY stats:page_1 $.views 1
```

---

## 8️⃣ Advantages of Redis JSON

* **Structured Data** → store key-value + nested objects
* **Fast** → Redis in-memory speed
* **Atomic updates** → update fields without overwriting whole object
* **Compatible** → works with caching, session store, analytics

---

## 9️⃣ Key RedisJSON Commands Summary

| Command          | Use                               |
| ---------------- | --------------------------------- |
| `JSON.SET`       | Store JSON object/array           |
| `JSON.GET`       | Retrieve JSON data                |
| `JSON.DEL`       | Delete JSON path / field          |
| `JSON.NUMINCRBY` | Increment numeric field           |
| `JSON.ARRAPPEND` | Append to array                   |
| `JSON.ARRPOP`    | Pop item from array               |
| `JSON.MGET`      | Get JSON field from multiple keys |

---

## 🔟 Tips & Best Practices

1. Always **use namespaced keys**
   Example: `user:100`, `cart:200`
2. For **dynamic nested structures**, prefer JSON over Strings
3. Combine RedisJSON + Redis TTL for **expiring cache**
4. Use `JSON.GET` with **path queries** for efficient reads
5. Use `NUMINCRBY` for **atomic counters inside JSON**

---