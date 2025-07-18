---
github_issue: 2
---
## 🗃️ MySQL & Laravel Migration Notes

### 🔢 Auto-Increment Columns

- In **MySQL**, to make a column `AUTO_INCREMENT`, it **must be a PRIMARY KEY**.
    
- In some other databases (e.g., PostgreSQL), auto-incrementing is supported without being a primary key.
    

---

### 🌍 Set Laravel Application Timezone

- You can change the timezone of your Laravel app from:
    
    `config/app.php`
    
    Just update the `timezone` value:
    
    `'timezone' => 'Asia/Riyadh', // or any valid timezone`
    

---

### 🔢 Integer Types in MySQL

| Type            | Description                                  |
| --------------- | -------------------------------------------- |
| `tinyInteger`   | Max value: **127** (or **255** if unsigned)  |
| `smallInteger`  | Max value: **32,767**                        |
| `mediumInteger` | Max value: **8 million+**                    |
| `integer`       | Max value: **2 billion+**                    |
| `bigInteger`    | Max value: **very large**, takes **8 bytes** |

- Use **`unsigned()`** if the column should not accept negative values:
    
    `$table->integer('views')->unsigned();`
    

---

### 🆚 `string()` vs `text()` in Migrations

| Method     | Use Case                                      |
| ---------- | --------------------------------------------- |
| `string()` | Equivalent to `VARCHAR`. Specify max length:  |
|            | `$table->string('phone', 20);` (Max 20 chars) |
| `text()`   | Used for longer text (up to ~64,000 chars)    |

---

### 🆚 `unique()` vs `primary()`

|Feature|Can Be NULL|Multiple Columns Allowed?|
|---|---|---|
|`primary()`|❌ No|✅ Yes (composite keys)|
|`unique()`|✅ Yes|✅ Yes|

- `primary()` also creates an **index** and enforces uniqueness.
    

---

### 🔘 `enum()` Column Type

- Restricts values to a predefined set:
    
    `$table->enum('status', ['Active', 'InActive']);`
    

---

### 🧭 Rollback Migrations

#### Command:

`php artisan migrate:rollback`

- Reverts the **last batch** of migrations.
    
- Executes the `down()` method inside each rolled-back migration file.



## 🧱 Laravel Migration & Eloquent Tips

### 🔗 Foreign Keys & Relations

#### 🔹 `foreignId()`
- Creates a `BIGINT` column with **`UNSIGNED`**.
- Typically used for foreign key references.

#### 🔹 `constrained()`

- Creates a **foreign key constraint**.
- By default, links to the primary key (`id`) of the referenced table.
- You can customize like:
    `$table->foreignId('user_id')->constrained('users', 'id');`

#### 🔹 Deletion Behaviors

- `cascadeOnDelete()` → When the parent is deleted, the child rows are deleted too.
- `restrictOnDelete()` → Prevent deletion of the parent if it has related child rows. (Laravel default behavior)
- `nullOnDelete()` → When the parent is deleted, the child foreign key is set to `null`.
---

### ⚙️ Artisan Migration Commands

| Command                                 | Purpose                                            |
| --------------------------------------- | -------------------------------------------------- |
| `php artisan migrate:status`            | Show which migrations have run                     |
| `php artisan migrate:rollback --step=2` | Roll back the last 2 batches                       |
| `php artisan migrate:reset`             | Roll back **all** migrations                       |
| `php artisan migrate:refresh`           | Rollback **all**, then re-run all migrations       |
| `php artisan migrate:fresh`             | **Drop all tables**, then run all migrations again |

---

### 🧮 Table Engines in MySQL

| Engine   | Supports Relations?                               |
| -------- | ------------------------------------------------- |
| `MyISAM` | ❌ No foreign keys                                 |
| `InnoDB` | ✅ Supports foreign keys (default on most servers) |

---

### 🛠️ Fixing "Key too long" Error

If you get an error like:

`Specified key was too long`

✅ Fix it in `config/database.php` under MySQL connection:

`'charset' => 'utf8', // instead of utf8mb4 'collation' => 'utf8_unicode_ci', 'engine' => 'InnoDB', // force engine`

---

### 📌 Eloquent Model Customization

| Feature                                        | Usage                                                     |
| ---------------------------------------------- | --------------------------------------------------------- |
| `protected $connection = 'custom_connection';` | Use a non-default DB connection                           |
| `protected $primaryKey = 'uuid';`              | Custom primary key field name                             |
| `public $incrementing = false;`                | Disable auto-increment behavior                           |
| `public $timestamps = false;`                  | Disable automatic timestamps (`created_at`, `updated_at`) |
| `protected $keyType = 'string';`               | Set Primary key type other than integer                   |
|                                                |                                                           |

#### Custom Timestamp Column Names:

`const CREATED_AT = 'created_date'; const UPDATED_AT = 'updated_date';`

---

### 🔐 Hash vs Encrypt

|Feature|Description|
|---|---|
|`hash()`|One-way. Cannot be reversed. Used for passwords.|
|`encrypt()` / `decrypt()`|Two-way. Encrypted value can be decrypted. Used for sensitive data (tokens, etc).|

---

### 📝 Using Query Builder & Timestamps

- If you insert data using **Query Builder** like:
    `DB::table('users')->insert([...]);`
    And don't manually assign `created_at` or `updated_at`, they will be set to `NULL`.
