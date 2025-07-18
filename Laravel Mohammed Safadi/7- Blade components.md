## ✅ Laravel Blade Components & `Route::currentRouteName()` – Easy Guide

---

## 🔹 Anonymous Blade Components

### ✅ What is it?

An **anonymous component** is a Blade file **only** — no PHP class behind it.

Useful when:

- You want to reuse UI like buttons, cards, alerts, etc.
- You **don’t need logic** in PHP.

### 📁 Where is it saved?

Laravel puts it here:

`resources/views/components/`
### 🛠 How to create:

`php artisan make:component Button --view`

This creates:

`resources/views/components/button.blade.php`

No PHP class is created.

### ✅ How to use in your Blade:

`<x-button />`

Or with content:

`<x-button>Click Me</x-button>`

🧠 **Memory Tip**:  
_"Anonymous = no class. Just the view."_

---

## 🔸 Class-Based Blade Components

### ✅ What is it?

A **class component** has:
1. A **PHP class** (for logic)
2. A **Blade view** (for HTML)

Used when:

- You want to **pass data**
- You need **logic or conditions** in PHP

### 🛠 How to create:

`php artisan make:component Card`

This creates:

`app/View/Components/Card.php resources/views/components/card.blade.php`

---

### ✅ Passing Data to Component

In your Blade view:

`<x-card :title="$title" />`

In your class file `Card.php`:

`public $title;  public function __construct($title) {     $this->title = $title; }`

Now in `card.blade.php`:

`<h1>{{ $title }}</h1>`

🧠 **Memory Tip**:  
_"Class = logic. You can pass data to it."_

---

## 🧭 `Route::currentRouteName()` – Get Current Route Name

### ✅ What it does:

It returns the **name** of the route you are on.
### 🛠 Example:

In Blade or controller:

`if (Route::currentRouteName() == 'dashboard') {     echo 'You are on the dashboard page'; }`

### ✅ Great for:

- Highlighting active menu:

`<a href="/dashboard" class="{{ Route::currentRouteName() == 'dashboard' ? 'active' : '' }}">     Dashboard </a>`

- Showing different layout or titles:

`<title>     {{ Route::currentRouteName() == 'settings' ? 'Settings Page' : 'My App' }} </title>`

🧠 **Memory Tip**:  
_Use `Route::currentRouteName()` to know where the user is._

---

## 📝 Summary Table

|Feature|What it Does|When to Use|
|---|---|---|
|Anonymous Component|Blade file only, no class|Simple UI (e.g. button, card)|
|Class Component|Has class + Blade view|When you need logic or pass data|
|`Route::currentRouteName()`|Get current route name|Highlight menu, dynamic titles, conditions|

---