## 📁 Laravel File Upload Notes & Best Practices

### 🔐 1. When to Store Files in the Database

- If the uploaded image or file is **very sensitive** (e.g., passports, ID cards, contracts), it's better to **store it directly in the database** as a BLOB (binary).
    
- Otherwise, it's recommended to store it in the **file system** (e.g., `storage/app/public`) and just store the **file path** in the database for better performance and scalability.
    

---

### 📤 2. Difference Between `store()` and `storeAs()`

#### ✅ `store()`
`$file->store('folder', 'disk');`

- Automatically generates a **unique file name**.
    
- Takes **2 arguments**:
    
    1. Folder name where the file will be stored.
        
    2. Disk type (e.g., `local`, `public`).
        

#### ✅ `storeAs()`

`$file->storeAs('folder', 'filename.jpg', 'disk');`

- Lets you set a **custom file name**.
    
- Takes **3 arguments**:
    
    1. Folder name.
        
    2. Custom file name.
        
    3. Disk type.
        

#### 📌 Notes

- If you define the disk in `.env`:
    `FILESYSTEM_DISK=public`
    
    Then:
    
    - `store()` only needs **1 argument** (folder).
        
    - `storeAs()` needs **2 arguments** (folder and filename).
        

---

### 🗑️ 3. Deleting Files from Storage

`Storage::disk('public')->delete($path);`

- Deletes the file from the specified path on the given disk.
    

---

### ⚙️ 4. File System Configuration

File system settings are located in:

`config/filesystems.php`

#### 🛠️ Example: Creating a Custom Disk

`'disk_name' => [     'driver' => 'local',     'root' => public_path('disk_name'),     'url' => env('APP_URL').'/disk_name',     'visibility' => 'public',     'throw' => false,     'report' => false, ],`

You can use it like this:

`Storage::disk('disk_name')->put(...);`

---

### 🔗 5. Linking Storage to Public Directory
`php artisan storage:link`

- Creates a shortcut:  
    `public/storage → storage/app/public`
    
- Allows users to access uploaded files through the browser.
    

---

### 📍 6. Laravel Path Helper Functions

|Function|Description|
|---|---|
|`storage_path()`|Returns the full path to the `storage` folder.|
|`public_path()`|Returns the full path to the `public` folder.|

---

### 📎 7. File Information Methods

`$file = $request->file('image');`

|Method|Description|
|---|---|
|`$file->getClientOriginalName()`|Gets the original file name from the user's device.|
|`$file->getClientOriginalExtension()`|Gets the file extension (e.g., `jpg`, `pdf`).|
|`$file->getSize()`|Gets the file size in bytes.|
|`$file->getMimeType()`|Gets the MIME type (e.g., `image/png`, `application/pdf`).|

---

### ✨ 8. Clean Code Tip for `if` Conditions

Instead of writing:

`if ($value == 1) {     return 'Yes'; } return 'No';`

Use a cleaner version:

`return $value == 1 ? 'Yes' : 'No';`

- This makes your code shorter and easier to read.
    

---

### 🧪 9. Using `isset()` in PHP

`if (isset($variable)) {     // Do something }`

- `isset()` checks whether a variable **is defined and not null**.
- Helps prevent "undefined variable" errors.