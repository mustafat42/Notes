## 🔁 Laravel Scopes

### ✅ What is a Scope?

A **scope** is a reusable condition (filter) you write inside your model so that you don't repeat the same `where()` query every time.

---

### 🔹 1. Local Scope

This is a custom method in the model that adds a filter. You **must call it** when you need it.
#### 🧱 Example in `Category.php`:

`public function scopeActive($query) {     
	return $query->where('is_active', 1);
}`

This is the same as writing:

`Category::where('is_active', 1)->get();`

#### ✅ Now you can use:


`Category::active()->get();`

This makes your code clean and reusable.

> ❌ You can't use it with `DB::table()`, only with Eloquent (`Category::` not `DB::table('categories')`).

🧠 **Think like this:**  
_"I use local scope when I want a clean shortcut for common conditions."_

---

### 🔸 2. Global Scope

A **global scope** means Laravel will **automatically** add the condition to **every query** of that model.

#### 🧱 Example in `Product.php`:


`protected static function booted() {     static::addGlobalScope('active', function ($query) {         $query->where('is_active', 1);     }); }`

Now every time you do:

`Product::all();`

Laravel will automatically add this:

`SELECT * FROM products WHERE is_active = 1;`

#### ❌ To remove it for one query:

`Product::withoutGlobalScope('active')->get();`

---

### 🛠 Using Global Scope as a Class

This is helpful if the condition is complex or you want to reuse it in other models.

#### 1. Create scope:

`php artisan make:scope ActiveScope`

#### 2. File: `App\Scopes\ActiveScope.php`

`use Illuminate\Database\Eloquent\Scope; use Illuminate\Database\Eloquent\Builder; use Illuminate\Database\Eloquent\Model;  class ActiveScope implements Scope {     public function apply(Builder $builder, Model $model) {         $builder->where('is_active', 1);     } }`

#### 3. In your model:

`use App\Scopes\ActiveScope;  protected static function booted() {     static::addGlobalScope(new ActiveScope); }`

🧠 **Think like this:**  
_"Global scope is like an automatic filter that runs on every query unless I stop it."_

---

## 🔀 Laravel Joins

Joins are used when you want data from more than one table in one query.

### 🔹 1. `innerJoin()`

Shows data **only** when it exists in **both** tables.

`DB::table('orders')   ->join('users', 'orders.user_id', '=', 'users.id')   ->select('orders.*', 'users.name')   ->get();`

> Will skip orders if user is missing.

---

### 🔸 2. `leftJoin()`

Shows **all orders**, even if user is missing (`user_id` is null).

`DB::table('orders')   ->leftJoin('users', 'orders.user_id', '=', 'users.id')   ->select('orders.*', 'users.name')   ->get();`

> This is useful if users are optional.

---

### 🔸 3. `rightJoin()`

Opposite of leftJoin – shows **all users**, even if they don’t have any orders.

`DB::table('users')   ->rightJoin('orders', 'orders.user_id', '=', 'users.id')   ->select('orders.*', 'users.name')   ->get();`

🧠 **Think like this:**

- **Inner** = Both must match
    
- **Left** = All from the left + match from right
    
- **Right** = All from the right + match from left

---

## 🗑 Laravel Soft Deletes

Laravel doesn’t delete records from the database directly — it just hides them by setting a `deleted_at` timestamp.
### 🔧 How to use:

`use Illuminate\Database\Eloquent\SoftDeletes;  class Product extends Model {     use SoftDeletes; }`

Now Laravel will add this column:

`deleted_at DATETIME`

If deleted, `deleted_at` is filled.

### 🔍 Useful Methods:

| Method          | What it does                     |
| --------------- | -------------------------------- |
| `withTrashed()` | Show all (deleted + not deleted) |
| `onlyTrashed()` | Show only deleted records        |
| `restore()`     | Bring back deleted record        |
| `forceDelete()` | Delete it forever from database  |

🧠 **Think like this:**  
_"Soft delete hides the item instead of removing it."_

---

## 🔗 Route Model Binding

Laravel automatically finds the model by ID when you use it in the route.
### 🔧 Example:

`Route::get('category/{category}', [CategoryController::class, 'show']);`

Then in your controller:

`public function show(Category $category) {     // Laravel fetches this automatically }`

> The parameter name (`{category}`) must match the variable name (`$category`).

🧠 **Think like this:**  
_"Route gives me the model without writing `find()`."_

---

## 🌱 Seeder vs Factory

Seeders and factories help add data to your database — but for different reasons.

|Feature|Seeder|Factory|
|---|---|---|
|What|Adds fixed data|Adds random fake data|
|Why|For real use (e.g. admin user)|For testing (e.g. 50 fake users)|
|File|`database/seeders`|`database/factories`|
|Example|`php artisan db:seed`|`User::factory()->create()`|

🧠 **Think like this:**

- _Seeder = Real data_
- _Factory = Fake test data_

---

## 🌐 Faker in Arabic

To make fake names, cities, etc., in Arabic:
### Step 1: In `config/app.php`, change:

`'faker_locale' => 'ar_SA',`

### Step 2: In your factory:

`'name' => $this->faker->name, 'address' => $this->faker->address,`

> Laravel will now generate Arabic text.

🧠 **Think like this:**  
_"Change locale = change language of fake data."_

---

## 🔎 More Important Laravel Tips

### ✅ `select()` vs `selectRaw()`

- `select()` → Normal and safe

`User::select('id', 'name')->get();`

- `selectRaw()` → Use when you need SQL logic
    

`User::selectRaw("CONCAT(first_name, ' ', last_name) as full_name")->get();`

Use `selectRaw()` for:

- Math (`SUM(price)`)
    
- String (`CONCAT(name, 'text')`)
    

---

### ✅ `addSelect()`

You can’t use `select()` more than once. The last one will overwrite the first one.

❌ Wrong:

`User::select('id')->select('name') // Only name will stay`

✅ Right:

`User::select('id')->addSelect('name')->get();`

🧠 **Think like this:**  
_"addSelect = add more columns"_

---

### ✅ `withCount()`

This adds a new field in the result that counts the number of related items.

`$categories = Category::withCount('products')->get();  foreach ($categories as $category) {     echo $category->products_count; }`

🧠 **Think like this:**  
_"withCount() adds a new column: `relation_count`."_