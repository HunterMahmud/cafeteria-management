# BUBT Cafeteria Management System (Console • C)

A simple Windows console application for managing a cafeteria's inventory and billing.
The program provides password-protected access to features like adding/editing items,
searching, deleting, displaying inventory, and generating customer bills.

> **Note:** This code targets **Windows** (uses `windows.h`, `conio.h`, console colors, cursor positioning).

---

## 🧭 Overview

- **Tech:** C (Console), Windows API for cursor & color
- **Storage:** Binary file `record.txt` (app creates/uses it in the same directory)
- **Login:** Password-protected main menu

### Main Features
- **Calculate Bill:** Add multiple items by code, quantity → shows line items + grand total and auto-decrements stock.
- **Add Goods:** Add new items with unique short code, name, rate, quantity.
- **Edit Goods:** Update name/code/rate/quantity of an existing item.
- **Display All:** Paginated list of all items.
- **Search:** Find a single item by code.
- **Delete Goods:** Remove an item by code (via a temporary swap file).
- **Exit:** Credits & graceful termination.

---

## 🔐 Login

- **Username:** _(not required)_
- **Password:** `mahmud`

The password is hardcoded in `main()`:

```c
char pass[15], password[] = "mahmud";
```
---
## 📦 Data Model

Each inventory record is stored as a binary `struct` in `record.txt`:

```c
typedef struct {
    char  name[15];   // item name (<=14 chars)
    char  code[4];    // item code (<=3 chars)
    float rate;       // unit price
    int   quantity;   // stock
} rec;
```

> ⚠️ **Constraints**
>
> * `code` must be **unique** and max **3 characters** (e.g., `C01`, `B2`).
> * `name` max **14 characters** (15 with null terminator).
> * The program **expects `record.txt` to be in the same directory** as the executable. If it does not exist, it will be created on first use.

---

## ▶️ How to Build & Run (Windows)

### Option A: MinGW (GCC)

1. Install **MinGW-w64** and ensure `gcc` is in your PATH.
2. Save the source as `cafeteria.c` (or your filename).
3. Compile:

   ```bash
   gcc cafeteria.c -o cafeteria.exe -std=c99
   ```
4. Run:

   ```bash
   .\cafeteria.exe
   ```

### Option B: Microsoft Visual C++ (MSVC / Developer Command Prompt)

1. Open **Developer Command Prompt for VS**.
2. Compile:

   ```cmd
   cl /Fe:cafeteria.exe cafeteria.c
   ```
3. Run:

   ```cmd
   cafeteria.exe
   ```

> The program sets console title/color and uses cursor positioning (`gotoxy`) via Windows API.
> It creates/reads `record.txt` and may use a temporary `record1.txt` during delete operations.

---

## 🧑‍💻 Usage Guide

1. **Launch** the program → splash screen → time/date.
2. Enter **password**: `mahmud`.
3. Main Menu:

   ```
   1. Calculate Bill
   2. Add Goods
   3. Edit Goods
   4. Display All
   5. Search
   6. Delete Goods
   0. Exit
   ```

### 1) Calculate Bill

* Enter **item code** and **quantity** repeatedly.
* Type `end` to finish.
* The program shows a formatted bill and **reduces stock** accordingly.

### 2) Add Goods

* Prompts **new unique code** (max 3 chars).
* Enter **rate**, **quantity**, **name** (max 14 chars).
* Appends to `record.txt`.

### 3) Edit Goods

* Enter **existing item code**.
* Choose to edit: **name / code / rate / quantity**.
* Saves back to `record.txt`.

### 4) Display All

* Shows a table of all items with pagination (press any key for next page).

### 5) Search

* Enter an **item code** to display a single item record.

### 6) Delete Goods

* Enter **item code** to delete.
* Internally writes all other records to `record1.txt`, deletes `record.txt`,
  then renames `record1.txt` → `record.txt`.

### 0) Exit

* Shows credits and exits.

---

## 🗂️ Files Created/Used

* `record.txt` – main binary data file for inventory
* `record1.txt` – temporary file used during deletion
* (Console output only; no logs are persisted.)

---

## 🧰 Troubleshooting

* **Wrong password / can’t login**
  → Check the hardcoded password in `main()` (`password[] = "mahmud"`), recompile.

* **Garbled UI / cursor position wrong**
  → Must run in **Windows console**. Resize console if text overlaps. The app uses `gotoxy()`.

* **Compile errors about `<conio.h>` or `<windows.h>`**
  → Make sure you compile on **Windows** with MinGW or MSVC.

* **Item not found while billing/searching**
  → Ensure the item **code** exists and is **≤ 3 characters**. Use “Display All” to confirm.

* **Accidentally deleted/edited data**
  → This is a binary file app; there is **no undo**. Back up `record.txt` manually before heavy changes.

---

## 🔒 Notes on Safety & Limits

* No input sanitization beyond simple checks; avoid spaces/newlines in names and codes.
* No concurrency; single-user console workflow.
* All data is local; no encryption.

---

## 👥 Credits

Developed by **Team Dark Knight**

* Hasan Al Mahmud (18192103239)
* Shamiul Islam (18192103241)
* Khaled Hasan (18192103266)
* Haseeb Rahman (18192103276)

Department of CSE, Bangladesh University of Business and Technology (BUBT)

---

## 📜 License

Academic/educational use. Use at your own risk.
