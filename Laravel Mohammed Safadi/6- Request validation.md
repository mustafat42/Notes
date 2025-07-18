## ✅ Laravel Validation – Complete Notes

### 🔢 Types of Validation

1. **Frontend Validation**
    
    - Performed in the browser using HTML/JavaScript (e.g., `required`, `maxlength`).
    - Provides fast feedback but **not secure** (can be bypassed).
2. **Backend Validation**
    - Handled in Laravel controllers or FormRequest classes using `validate()` or rules.
    - **Secure and required**, because it checks input on the server.
3. **Database Validation**
    - Done at the database level using constraints (e.g., `unique`, `not null`, `foreign key`).
    - Prevents invalid data from being stored.
        

---

### 📁 File Validation in Laravel

#### ✅ `mimetypes` vs `mimes`

- `mimetypes` → Validates **exact MIME types** (e.g., `application/pdf`, `image/jpeg`).
- `mimes` → Validates based on **file extensions** (e.g., `jpg`, `pdf`).
- ✅ `mimetypes` is **stricter and more secure** than `mimes`.
#### 📏 Image Dimensions Validation

To validate the **dimensions** of an uploaded image:

`'dimensions:width=100,height=100'`

> Validates if the image is exactly 100x100 pixels.

To use **minimum width/height**:

`'dimensions:min_width=100,min_height=100'`

To set a **range** (min and max):

`'dimensions:min_width=100,max_width=500,min_height=100,max_height=500'`

---

### ❗ Handling Validation Errors

#### ✅ Check if there are any errors:

`$errors->any(); // returns true if there are any validation errors`

#### ✅ Get first error for a specific input:

`$errors->first('name'); // returns first error message for the 'name' field`

#### ✅ Get all errors for a specific input:

`$errors->get('name'); // returns an array of all error messages for 'name'`

#### ✅ Blade syntax for showing an error:

`@error('name')     <p>{{ $message }}</p> @enderror`

> This will display the **first error** message for the `name` field.

---

### 🔄 `old()` Helper Function

`old('name'); // returns the previously submitted value from session`

- You can also provide a **default value**:

`old('name', 'default name');`

> Used to refill form inputs after validation errors.

---

### 🛠️ Custom Validation Rules in Laravel

Laravel offers **3 main ways** to write custom validation rules:

#### 1. **Closure-based Rule in `FormRequest`**

Inside your `rules()` method:

`'name' => [     'required',     function ($attribute, $value, $fail) {         if ($value === 'forbidden') {             $fail('This value is not allowed.');         }     }, ],`

---

#### 2. **Custom Rule Class**

- Create with Artisan:

`php artisan make:rule ValidPhoneNumber`

- In the rule class:

`public function passes($attribute, $value) {     return preg_match('/^05[0-9]{8}$/', $value); }  public function message() {     return 'The :attribute must be a valid Saudi phone number.'; }`

- Use it like this in validation:

`'phone' => ['required', new ValidPhoneNumber],`

---

#### 3. **Register a Global Rule Using `Validator::extend`**

In `AppServiceProvider` > `boot()` method:

`use Illuminate\Support\Facades\Validator;  Validator::extend('custom_rule', function ($attribute, $value, $parameters, $validator) {     return $value === 'special'; }, 'The :attribute must be special.');`

- Then use it like:

`'name' => 'required|custom_rule',`