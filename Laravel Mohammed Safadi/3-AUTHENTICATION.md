## 📦 Composer Notes

### 🔹 `composer.json` — How to Know If a Package is Required  or Dev-Only

- **`"require"` section:**  
    Packages listed here are **essential for the application to run**, including in production.
- **`"require-dev"` section:**  
    Packages listed here are **only needed during development**, such as testing tools and debuggers.

### 🔹 Reinstalling a Package

To completely reinstall a Composer package:
`composer remove package_name'

---

## ⚙️ NPM Notes

### 🔹 Installing Dependencies


`npm install`

- Installs both `dependencies` and `devDependencies` listed in your `package.json`.
### 🔹 Run Laravel Mix for Development
`npm run dev`

- Compiles your **CSS and JavaScript** files from the `resources` folder and outputs them to the `public` directory.
---
### 🔹 Route Name Conflicts

- If you define **multiple routes with the same name**, Laravel will **override the earlier ones** and only use the **last defined route**.
---
### 🔹 Using Multiple Guards

- Defined in `config/auth.php`.
- Each guard has:
    
    - A `driver` (e.g., `session` for web or `token` for API).
    - A `provider` (which links to the user model or data source).
---
### 🔹 Facades in Laravel
- You don’t need to import their full namespaces because **aliases are defined** in `config/app.php`.
---
### 🔹 Authentication Helpers

- `Auth::check()`  
    Returns `true` if the user is authenticated, `false` otherwise.
- In Blade:
    `@auth   <!-- content for authenticated users --> @endauth`
---
### 🔹 Middleware Usage
- Middleware can be used:
    
    - **In the route directly:**
        `Route::get('dashboard', 'DashboardController@index')->middleware('auth');`
        
    - **In the controller constructor:**  
        This will apply the middleware to **all methods** in the controller:
        `public function __construct() {     $this->middleware('auth'); }`
        

---

### 🔹 CSRF and Error 419 (Page Expired)

- This error usually occurs when you **send a PUT, POST, DELETE, or PATCH request without a valid CSRF token**.
    
- Make sure to include `@csrf` in your Blade forms:
    `<form method="POST">     @csrf </form>`

---
### 🔹 Default Redirect After Login

- After a successful login, Laravel by default **redirects to a route named `dashboard`**, if defined.
---
### 🔹 Redirect Methods in Laravel

- Redirect to a **URL**:
    `return redirect('route_url');`
- Redirect to a **named route**:
    `return redirect()->route('route_name'); // or return redirect(route('route_name'));`
---
### 🔹 Email Verification for New Users

To automatically send a verification email when a new user registers:

1. Let your `User` model implement the interface:
    `use Illuminate\Contracts\Auth\MustVerifyEmail;  
    class User extends Authenticatable implements MustVerifyEmail`
    
2. Protect routes with `verified` middleware:s
    `Route::get('/dashboard', function () {     return view('dashboard'); })->middleware(['auth', 'verified']);`