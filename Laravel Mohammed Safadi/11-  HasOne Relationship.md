## 🔤 Difference Between `CHAR` and `VARCHAR` in Databases

### 🔹 `CHAR(n)`

- **Fixed length**
- Always uses exactly `n` characters.
- If the value is shorter, the database **fills the rest with spaces**.

#### ✅ Example:

`name CHAR(10)`

Now if you insert:

`INSERT INTO users (name) VALUES ('Ali');`

➡️ The database **stores** it as:  
`'Ali '` (with 7 spaces)

🧠 **Remember**:  
_`CHAR` always books the full space, even if unused._

---

### 🔸 `VARCHAR(n)`

- **Variable length**
- Only uses space for **actual characters**, plus a little extra (1 or 2 bytes) to store the length.
#### ✅ Example:

`name VARCHAR(10)`

Now if you insert:

`INSERT INTO users (name) VALUES ('Ali');`

➡️ The database stores:  
`'Ali'` only — **no extra spaces**.

🧠 **Remember**:  
_`VARCHAR` = only use space for what you need._

---

### 🆚 Summary Table

|Feature|`CHAR(n)`|`VARCHAR(n)`|
|---|---|---|
|Size|Fixed|Flexible|
|Space used|Always `n` bytes|Only needed length + 1/2 bytes|
|Padding|Adds spaces|No padding|
|Performance|Slightly faster for fixed-size|More efficient in space|

---

## 🔗 `withDefault()` in Laravel

✅ The `withDefault()` method is used to prevent errors when a relationship is `null` (not found).

It is used **only** with:

- `belongsTo()`
    
- `hasOne()`
### ❌ Not for:

- `hasMany()`
    
- `morphMany()`
    
- `belongsToMany()`
### ✅ Example:

`public function category() {     return $this->belongsTo(Category::class)->withDefault([         'name' => 'Uncategorized',     ]); }`

Now this:

`$product->category->name;`

➡️ Will return `"Uncategorized"` if `category_id` is null or not found.

🧠 **Remember**:  
_Use `withDefault()` for single object relationships only (1-to-1 or many-to-1)._

---

## 🔁 `PUT` vs `PATCH` in Laravel Routes

### 🔸 `PUT`

- Used for **full update** (replace the whole record).
- The route usually needs an **ID**.

`Route::put('/users/{id}', [UserController::class, 'update']);`

🧠 Think of it like: _“Replace the whole user”_

---

### 🔹 `PATCH`

- Used for **partial update** (only some fields).
    
- **Can be used without ID** if you know the model from somewhere else (e.g. authenticated user).

`Route::patch('/profile', [ProfileController::class, 'update']);`

🧠 Think of it like: _“Just update a part”_

---

## 📌 Summary Notes

|Concept|Explanation|Example|
|---|---|---|
|`CHAR(n)`|Fixed size, fills unused space|`'Ali '`|
|`VARCHAR(n)`|Variable size, no extra padding|`'Ali'`|
|`withDefault()`|Used with `belongsTo()` or `hasOne()` to avoid null errors|`$product->category->name`|
|`PUT`|Full update, must have `{id}` in route|`Route::put('/user/{id}')`|
|`PATCH`|Partial update, may not need `{id}`|`Route::patch('/profile')`|

---