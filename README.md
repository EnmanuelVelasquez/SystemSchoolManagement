# 🎓 CalifiC — School Management System in C

A terminal-based school management system written in **C**, designed to handle the core administrative needs of an educational institution. The system supports role-based access, CRUD operations for users and subjects, and file-based data persistence — all built from scratch without external libraries.

---

## 📖 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Data Model](#data-model)
- [File Persistence](#file-persistence)
- [How to Compile & Run](#how-to-compile--run)
- [Login & Access](#login--access)
- [Menu Navigation](#menu-navigation)
- [C Concepts Applied](#c-concepts-applied)
- [Known Limitations & Future Improvements](#known-limitations--future-improvements)

---

## Overview

**CalifiC** is a console application that simulates a school's administrative backend. It allows administrators to log in and manage teachers, students, subjects, and grades through an interactive menu system. Data is stored in plain `.txt` files, simulating a lightweight database using structured text parsing with `fscanf` and `fprintf`.

---

## Features

- ✅ Secure login system with role verification (Admin)
- ✅ Full CRUD for **Administrators**
- ✅ Menu-driven interface for managing **Teachers (Docentes)**
- ✅ Menu-driven interface for managing **Students (Estudiantes)**
- ✅ Subject (**Asignatura**) management with grade tracking
- ✅ File-based persistence — data survives between program runs
- ✅ Session management with logout and re-login
- ✅ Graceful exit with a timed delay

---

## Project Structure

```
CalifiC/
├── main.c               # Entry point — launches welcome screen and login
├── login.h              # Login logic, session handling, and logout
├── admin.h              # Admin struct, CRUD operations, and admin menu
├── docente.h            # Teacher struct and menu declarations
├── estudiante.h         # Student struct and menu declarations
├── asignatura.h         # Subject struct and menu declarations
└── data/
    ├── admin.txt        # Persisted admin records
    ├── docente.txt      # Persisted teacher records
    └── estudiante.txt   # Persisted student records
```

> ⚠️ The `data/` directory must exist before running the program, as the system opens files directly without creating the folder automatically.

---

## Data Model

### Admin
| Field | Type | Description |
|---|---|---|
| `id` | `int` | Unique identifier |
| `nombre` | `char[50]` | Admin username |
| `contrasena` | `char[50]` | Admin password |

### Docente (Teacher)
| Field | Type | Description |
|---|---|---|
| `id` | `int` | Unique identifier |
| `nombre` | `char[50]` | First name |
| `apellido` | `char[50]` | Last name |
| `contrasena` | `char[50]` | Password |
| `asignaturas` | `Asignatura[3]` | Up to 3 assigned subjects |

### Estudiante (Student)
| Field | Type | Description |
|---|---|---|
| `id` | `int` | Unique identifier |
| `nombre` | `char[50]` | First name |
| `apellido` | `char[50]` | Last name |
| `contrasena` | `char[50]` | Password |

### Asignatura (Subject)
| Field | Type | Description |
|---|---|---|
| `nombre` | `char[50]` | Subject name |
| `calificaciones` | `float[6]` | Up to 6 grade slots |

---

## File Persistence

Data is stored in a custom plain-text format inside the `data/` folder. Each record occupies one line using a semicolon-delimited key-value structure:

```
Tipo:Admin;ID:1;Nombre:admin;Password:adminpass
Tipo:Admin;ID:1112793002;Nombre:andres;Password:andres123
```

The system reads and writes these files using:
- `fopen` / `fclose` for file handling
- `fprintf` for writing structured records
- `fscanf` / `fgets` + `sscanf` for parsing records back into structs
- A **temp file strategy** for safe deletions — writes surviving records to `temp.txt`, then replaces the original file

---

## How to Compile & Run

### Prerequisites
- GCC compiler (MinGW on Windows recommended)
- Windows OS (the project uses `windows.h` and `Sleep()`)
- A `data/` folder in the same directory as the executable

### Compile

```bash
gcc main.c -o main.exe
```

### Run

```bash
./main.exe
```

Or simply run the included `main.exe` directly on Windows.

> 💡 If you want to port this to Linux/macOS, replace `#include <windows.h>` and `Sleep(ms)` with `#include <unistd.h>` and `sleep(seconds)`.

---

## Login & Access

On launch, the program greets the user and prompts for credentials:

```
-----Bienvenido a CalifiC-----
Ingrese su usuario:
Ingrese su contraseña:
```

Credentials are validated against `data/admin.txt`. A default admin account is pre-loaded:

| Usuario | Contraseña |
|---|---|
| `admin` | `adminpass` |

The login function reads the file line by line, parses each record, and compares the input using `strcmp`. On success, the admin menu is displayed. On failure, the user is notified and can retry.

---

## Menu Navigation

```
Menú Administrador
├── 1. Gestionar administrador
│   ├── 1. Crear
│   ├── 2. Leer
│   ├── 3. Actualizar
│   └── 4. Eliminar
├── 2. Gestionar docente       (declared)
├── 3. Gestionar estudiante    (declared)
├── 5. Gestionar asignatura    (declared)
├── 6. Gestionar calificacion  (declared)
├── 7. Cerrar sesión           → returns to login screen
└── 0. Salir                   → exits the program
```

---

## C Concepts Applied

| Concept | How it's used |
|---|---|
| **Structs** | `Admin`, `Docente`, `Estudiante`, `Asignatura` model each entity |
| **Header guards** | `#ifndef / #define / #endif` prevent double inclusion in all `.h` files |
| **File I/O** | `fopen`, `fclose`, `fprintf`, `fscanf`, `fgets`, `sscanf` for persistence |
| **Pointers** | Function parameters pass strings as `char*` for login validation |
| **String functions** | `strcmp`, `strcspn`, `strlen` from `<string.h>` for parsing and comparison |
| **Temp file strategy** | Safe record deletion by writing to a temp file and renaming |
| **`do-while` loops** | Menu interaction keeps looping until the user selects exit |
| **`switch` statements** | Clean dispatch of menu options |
| **Macros** | `#define MAX_ADMINS 100` sets a fixed buffer size for in-memory reads |
| **Error handling** | `fopen` return checks, `perror`, and `strerror(errno)` for file errors |

---

## Known Limitations & Future Improvements

### Current Limitations
- ⚠️ Passwords are stored in **plain text** — no hashing or encryption
- ⚠️ `windows.h` dependency makes the project non-portable to Linux/macOS
- ⚠️ `scanf("%s")` has no buffer size limit — potential buffer overflow risk
- ⚠️ Only the **Admin CRUD** is fully implemented; Teacher, Student, Subject, and Grade management are declared but not yet implemented
- ⚠️ The `data/` folder must be created manually before first run

### Planned Improvements
- [ ] Implement full CRUD for Docente, Estudiante, and Asignatura
- [ ] Add student grade report generation (`generarBoletin`)
- [ ] Add Teacher and Student login roles with their own menus
- [ ] Replace `scanf` with `fgets` for safer string input
- [ ] Replace `Sleep`/`windows.h` with cross-platform alternatives
- [ ] Hash passwords before storing them
- [ ] Auto-create the `data/` directory if it doesn't exist

---

> 💡 *This project is part of a personal learning path to strengthen systems programming fundamentals in C, focusing on file I/O, struct-based data modeling, and modular design with header files.*
