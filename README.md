# Smart Parking Management System (C)

A console-based parking management system written in C. Users can register, log in, book a parking slot for a number of hours, pay for it, and look back at their records. A separate admin login lets staff view all records and active sessions.

Built as a team project during our 2nd semester of B.Tech Computer Science at Graphic Era Deemed to be University, Dehradun.

## What it does

- **User accounts:** register and log in. Credentials are stored in `users.txt`.
- **Parking:** book a slot by ID for a chosen number of hours. Active slots are kept in a linked list and saved to `slots.txt`.
- **Fee and payment:** the fee is worked out from the hours booked, then the user picks Cash, UPI or Card. Payment is simulated, with no real gateway.
- **Records:** every booking (slot, hours, fee) is appended to `record.txt` and can be viewed later.
- **Sessions:** a linked-list session module tracks active parking sessions (slot, vehicle, user, in-time).
- **Admin panel:** a separate admin login to view all records and sessions.

## Concepts used

- Singly linked lists for slots and sessions (insert, delete, search, traverse)
- File handling for persistent storage (`fopen`, `fscanf`, `fprintf`, `fgets`)
- Structs and dynamic memory (`malloc`, `free`)
- Modular design: one `.c` / `.h` pair per feature

## Project structure

| File | Purpose |
|---|---|
| `main.c` | Entry point, prints the banner and starts the main menu |
| `menu.c / menu.h` | Main menu and the user parking menu |
| `auth.c / auth.h` | User registration and login (`users.txt`) |
| `admin.c / admin.h` | Admin login and admin panel |
| `parking.c / parking.h` | Slot linked list: add, show, save, find next free slot |
| `fees.c / fees.h` | Duration and cost calculation, transaction log (`transactions.txt`) |
| `payment.c / payment.h` | Payment method selection (simulated) |
| `record.c / record.h` | Booking records (`record.txt`) |
| `session.c / session.h` | Active session linked list: add, end, show, search by slot |
| `util.c / util.h` | Clear-screen and pause helpers |

## Data files

The program creates these files in the folder where it runs:

| File | Contents |
|---|---|
| `users.txt` | One `username password` pair per line |
| `record.txt` | One line per booking: slot, hours, fee |
| `slots.txt` | Currently active slots (rewritten on every save) |
| `transactions.txt` | Transaction log written by `fees.c` |

## Build and run

You need a C compiler such as GCC.

```bash
gcc *.c -o parking
./parking          # on Windows: parking.exe
```

## How to use

1. Start the program and choose **Login**, **Register** or **Admin Login**.
2. After logging in, the parking menu offers:
   1. Park Vehicle: enter a slot ID and the number of hours, then pay
   2. View Slots
   3. View Records
   4. View Sessions
   5. Exit
3. The admin panel offers **View All Records** and **View Sessions**.

## Known issues

These are things we know are not finished or not good enough yet:

- **The code does not build as it stands.** `menu.c` calls `calcFee()` and `saveSession()`, and `menu.c` and `admin.c` call `showSession()`, but none of these exist. `fees.c` has `calcDuration()` and `recordTransaction()` instead, and `session.c` defines `showSessions()` (with an "s"). These need to be matched up before `gcc *.c` links.
- Admin credentials are hard-coded in `admin.c`, and user passwords are saved in plain text in `users.txt`. Fine for a college project, not for real use.
- Slots are held in memory and the list is lost when the program closes.
- `MAX_SLOTS` (10) and `getNextFreeSlot()` exist, but the menu lets the user type any slot ID instead of using them.
- `recordTransaction()`, `endSession()`, `findSessionBySlot()` and the helpers in `util.c` are written but not yet connected to the menus.

## Future improvements

- Connect the session module so a booking opens a session and ending it frees the slot
- Use `getNextFreeSlot()` to assign slots automatically and enforce the slot limit
- Hash passwords and move admin credentials out of the source code
- Reload slots from `slots.txt` on startup
- Per-minute billing through `recordTransaction()`

## Team

Add your team members' names and GitHub profiles here.
