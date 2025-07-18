---
github_issue: 2
---
## 📁 `.env` File Notes & Best Practices

### 1. **Why `.env` Starts with a Dot (`.`)**

- On **Linux systems**, any file that starts with a dot (`.`) is **hidden by default**.
    
- Since most servers are Linux-based, the `.env` file is hidden for **security reasons**, to reduce exposure of sensitive configuration variables.
    

---

### 2. **.env Variable Format**

- If a value contains **spaces**, wrap it with **double quotes (`"`)**:
    
    env
    
    
    `APP_NAME="My Awesome App"`
    
- If the value does **not** contain spaces, quotes are **optional**:
    
    `APP_ENV=local`
    

---

### 3. **`ASSET_URL` Usage**

- `ASSET_URL` is used when you serve your **static assets (images, CSS, JS)** from a **different domain/server**.
    
- It’s used by Laravel's `asset()` helper.
    
- If `ASSET_URL` is **null**, Laravel falls back to using `APP_URL`.
    

---

### 4. **Caching Configuration**

#### ✅ `php artisan config:cache`

- Combines all configuration files into a single cached file to **improve performance**.
- The generated cache file is stored at:    
    `bootstrap/cache/config.php`


#### ⚠️ Note on `env()` Usage:

- Avoid using `env()` **directly in your application code**, especially outside of config files.
    
- After running `config:cache`, values fetched via `env()` **won’t be available**, because `.env` is no longer read at runtime.
    
- ✅ **Best practice:** Use `config('your_config_key')` instead of `env()`.
#### ❌ `php artisan config:clear`

- Deletes the cached config file (`bootstrap/cache/config.php`), so Laravel reverts to loading each config file on every request.
    

---

### 5. **PostgreSQL vs MySQL**

- **PostgreSQL** is a powerful open-source relational database with **more advanced features** than MySQL.
    
- However, for **basic relational operations and performance**, **MySQL is often simpler and faster** in typical Laravel applications.
    

---

### 6. **Using Multiple Databases In Project

- Laravel allows **multiple database connections**, even of the **same type** (e.g., multiple MySQL instances).
    
- You can define additional connections in `config/database.php` under the `connections` array:
    
    `'connections' => [     'mysql' => [...],     'mysql_reporting' => [...], // new connection ]`
    
- For each new connection, define separate `.env` variables:
    
    env
    
    `DB_REPORT_HOST=127.0.0.1 
    DB_REPORT_DATABASE=reporting 
    DB_REPORT_USERNAME=root 
    DB_REPORT_PASSWORD=secret`
    

---

### ✅ Summary / Reminders:

| Topic                   | Key Point                                                               |
| ----------------------- | ----------------------------------------------------------------------- |
| `.env` file             | Hidden on Linux for security.                                           |
| Quoting values          | Use `"` only if the value contains spaces.                              |
| `ASSET_URL`             | Used for loading assets from external servers.                          |
| `config:cache`          | Boosts performance, stores config in `bootstrap/cache/config.php`.      |
| `env()` vs `config()`   | Use `config()` for accessing env values safely in your code.            |
| `config:clear`          | Removes config cache.                                                   |
| PostgreSQL vs MySQL     | PostgreSQL has advanced features, MySQL is better for simple relations. |
| Multiple DB connections | Add new configs in `config/database.php` + define in `.env`.            |

### 🔤 `utf8mb4` – Recommended Character Set for Multilingual Data

- `utf8mb4` is a **character encoding** (not encryption) used in MySQL databases.
- It fully supports **UTF-8**, including **Arabic, English, emojis**, and all international characters.
- The default `utf8` in MySQL only supports up to **3 bytes**, which **excludes emojis and some special characters**.
- `utf8mb4` supports **up to 4 bytes**, which is **required for full UTF-8 compatibility**.