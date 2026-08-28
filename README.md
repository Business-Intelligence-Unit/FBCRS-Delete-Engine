# Delete Engine

A small desktop app that permanently deletes files listed in an Excel tracker.

You choose how many files to delete, pick the tracker, review the count, and confirm. The app then deletes the files and saves a results log.

**Deletion is permanent. Files do not go to the Recycle Bin and cannot be recovered.**

---

## Requirements

- Python 3.8 or newer
- Two packages:

```
pip install pandas openpyxl
```

- The network drive must be mapped as **Z:** before you run the app.

---

## How to run

Double-click `Delete_Engine.py`, or from a command prompt:

```
python Delete_Engine.py
```

---

## What your Excel file needs

The tracker must be `.xlsx` and must have these two columns by name:

| Column name | What goes in it |
|---|---|
| `Full File Path` | The complete path to the file |
| `File Name` | The file name (used in the results log) |

It also checks two columns **by position**, not by name:

| Position | Column | Must contain |
|---|---|---|
| 12th column | L | `destroy` |
| 16th column | P | `yes` |

Both values are case-insensitive and extra spaces are ignored.

A row is only deleted if **both** conditions are true. Rows with a blank `Full File Path` are skipped.

> Important: because columns L and P are read by position, inserting or removing columns in your tracker will break the app. Keep the column order the same.

---

## Steps

1. Type how many files you want to delete. It starts at 50.
2. Click **Select File & Start**.
3. Pick your Excel tracker.
4. The app filters rows where L = destroy and P = yes.
5. It swaps the drive letter on each path to `Z:` (for example, `O:\Records\file.pdf` becomes `Z:\Records\file.pdf`).
6. It checks which files actually exist on the server and stops once it reaches your number.
7. It shows you how many it found and asks you to confirm.
8. If you click Yes, it deletes them.
9. It saves a results file next to your tracker.

---

## The results file

Saved in the same folder as your tracker, named:

```
Deletion_Results_YYYYMMDD-HHMMSS.xlsx
```

| Column | Meaning |
|---|---|
| File Name | From your tracker |
| Checked Path | The path after the drive letter was changed to Z: |
| Status | `DELETED` or `ERROR` |

`ERROR` usually means the file was locked, in use, or you don't have permission to delete it.

---

## Working in batches

The count box is there so you can start small. Run 5 or 10 first, open the results file, and confirm the right files were removed. Once you trust it, raise the number.

---

## Common problems

**"Please enter a valid number."**
The count box is empty or has letters in it. Type digits only.

**"Could not read file."**
The file isn't a valid `.xlsx`, or it's open in Excel. Close it and try again.

**"No matching files found on the server."**
One of three things: no rows matched destroy + yes, the paths don't exist under `Z:`, or the Z: drive isn't mapped.

**Everything comes back as ERROR.**
Usually a permissions issue on the network drive, or the files are open by someone else.

---

## Things to know before you use it

- There is no undo and no quarantine step. Files are gone.
- The app only deletes files, not folders.
- Nothing is written back to your tracker. If you need to mark rows as done, do it yourself using the results file.
- Keep every results file. It's your only record of what was deleted.
