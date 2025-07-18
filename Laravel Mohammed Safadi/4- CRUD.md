## 🔍 Laravel Concepts Explained Simply

---

### ✅ `all()` vs `get()` in Laravel

|Feature|`all()`|`get()`|
|---|---|---|
|Belongs To|Model class|Eloquent or Query Builder|
|Purpose|Get **all records** from the table|Get **filtered records** (with `where`)|
|Use Case|No conditions|With `where`, `orderBy`, `limit`, etc.|
|Return Type|`Collection` of model objects|`Collection` of model objects|

**➕ Notes:**

- Both return a **Laravel Collection** — so you can use `foreach`, `map`, `filter`, etc.
    
- `get()` is more flexible; you can use it after filtering with `where()`.
### 📝 Example

`// All users $users = User::all();  // Active users only $activeUsers = User::where('status', 'active')->get();`

---

### ✏️ Form Method Spoofing in Laravel

- HTML forms only support `GET` and `POST`.    
- To send `PUT`, `PATCH`, or `DELETE`, we **spoof the method** inside a `POST` form using:

`<form method="POST" action="{{ route('post.delete') }}">     @csrf     @method('DELETE') <!-- this line makes it act like a DELETE request -->     <button type="submit">Delete</button> </form>`

---
### ⚠️ Note About Objects vs Arrays

- **Object (like a Model instance):** Even if it's empty or doesn't exist, it's still **not null**.
    
- **Array:** You can use `empty()` or `count()` to check.

---

### 🔁 Blade Loops with `@forelse`

Instead of writing:


`@if(count($users))     @foreach($users as $user)         {{ $user->name }}     @endforeach @else     No users found. @endif`

You can write this:

`@forelse($users as $user)     {{ $user->name }} @empty     No users found. @endforelse`

✅ Cleaner and shorter!

---

### 📥 `$request->post()` vs `$request->query()` vs `$request->all()`

| Method                  | Description                                       |
| ----------------------- | ------------------------------------------------- |
| `$request->post('id')`  | Gets value of input **from the form body** (POST) |
| `$request->query('id')` | Gets value from the **URL query string**          |
| `$request->all()`       | Returns **all input fields** as an array          |

---

### 🔁 PRG Pattern – Post/Redirect/Get

- Prevents duplicate form submissions if user refreshes after submitting.
    
- After storing data with `POST`, redirect the user to a `GET` route.


`public function store(Request $request) {     Category::create($request->all());     return redirect()->route('category.index'); // this avoids double submissions }`

---

### 🔐 `$fillable` vs `$guarded` in Models

| Concept     | Purpose                                                     | Example                                    |
| ----------- | ----------------------------------------------------------- | ------------------------------------------ |
| `$fillable` | Only these fields are **mass assignable** (whitelist)       | `protected $fillable = ['name', 'email'];` |
| `$guarded`  | All fields are mass assignable **except these** (blacklist) | `protected $guarded = ['is_admin'];`       |

✅ Use one — not both — in your model.

---

### ➕ `$request->merge()` – Add Data to Incoming Request

If you want to **add fields** to the current request before validation or saving:

`$request->merge(['user_id' => auth()->id()]);`

Now `$request->all()` will also include `user_id`.

---

## ✅ Summary Table

| Feature                  | Usage                                     |
| ------------------------ | ----------------------------------------- |
| `all()`                  | Get all records                           |
| `get()`                  | Get filtered records                      |
| `@method('DELETE')`      | Spoof HTTP methods in forms               |
| `@forelse` / `@empty`    | Blade syntax for clean loops              |
| `$request->post()`       | Get data from form                        |
| `$request->query()`      | Get data from URL                         |
| `$request->all()`        | Get all input data                        |
| `redirect()->route()`    | Used in PRG pattern after form submission |
| `$fillable` / `$guarded` | Mass assignment protection                |
| `$request->merge()`      | Add data to current request               |

## 🔁 **1. `Route::resource()` – Naming & Customization**

Laravel’s `Route::resource()` creates **7 RESTful routes** (index, create, store, show, edit, update, destroy) for a controller.

### ✅ Custom Route Names

You can assign custom names like this:

`Route::resource('categories', CategoryController::class)->names([     'index' => 'categories.index',     'create' => 'categories.create',     'store' => 'categories.store',     'show' => 'categories.show',     'edit' => 'categories.edit',     'update' => 'categories.update',     'destroy' => 'categories.destroy', ]);`

📝 **Note:** You must define **all 7 route names** if you use `.names([])`.

---
### ❌ Exclude Unwanted Routes

To avoid generating some routes:

`Route::resource('categories', CategoryController::class)->except(['index', 'destroy']);`

Or to include only specific routes:

`Route::resource('categories', CategoryController::class)->only(['index', 'store']);`

---

## 🖊️ **2. Blade Directives for Forms**

### ✅ `@selected(condition)` — for `<select>` options

Auto-select an option when a condition is met:
`<option value="{{ $parent->id }}" @selected($cat->parent_id == $parent->id)>     {{ $parent->name }} </option>`

### ✅ `@checked(condition)` — for checkboxes/radios

Use inside:

`<input type="checkbox" @checked($user->is_admin)>`

---

## 📘 **3. SQL Not Equal**

In SQL:

`WHERE id <> 5`

This is the same as:

`WHERE id != 5`

Both mean **"not equal to"**.

---

## 💾 **4. `$model->fill($request->all())`**

`fill()` allows you to bulk assign values from the request to a model:

`$cat->fill($request->all()); $cat->save();`

**⚠️ Important:** The fields being filled must be **listed in the `$fillable` array** in your model.

---

## 🛑 **5. `abort()` Method**

Stops the current request and returns an error page (like 404 or 403):

`abort(404); // Not Found abort(403); // Forbidden abort(500); // Server Error`

Useful for custom access checks or failed conditions.

---

## 🔍 **6. `findOrFail($id)`**

Fetches a model by ID. If the record is **not found**, it triggers a **404 error** (same as `abort(404)`).

`$cat = Category::findOrFail($id);`

Good for ensuring a valid record is present before proceeding.

---

## 🗑️ **7. Deleting Data**

### ✅ Delete One Record:

`Category::destroy($id); // Deletes by primary key`

### ✅ Delete All Records:

`Category::truncate(); // More efficient // or Category::query()->delete(); // Deletes all rows`

🚨 Use with caution — especially in production!

---

## 🐞 **8. Debugging with `dd()`**

To print a model or data:

`dd(Category::find($id));`

To dump a SQL query:

`dd(Category::where('id', $id)->toSql());`

> ❌ `Category::find($id)->dd()` doesn’t work because `dd()` is not a method on the model instance.

---

## ✅ Summary Table

|Feature|Usage|
|---|---|
|`Route::resource()`|Creates 7 RESTful routes|
|`@selected`, `@checked`|Blade directives for select & checkbox|
|`<>` or `!=` in SQL|Means "not equal"|
|`fill()` + `save()`|Mass assign values to a model|
|`abort(404)`|Return a specific HTTP error page|
|`findOrFail()`|Retrieve or throw 404 if not found|
|`destroy()` vs `delete()`|Destroy = by ID, Delete = for multiple/all records|
|`dd()`|Debug values or SQL queries|
|`$request->post()`|Get data from POST form|
|`$request->query()`|Get data from URL parameters|
|`$fillable`/`$guarded`|Control what fields can be filled|
|`merge()`|Add fields to request before saving|