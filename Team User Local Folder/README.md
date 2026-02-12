# 📂 Team User Local Folder
A Power Query (M) utility for cross‑machine path resolution. This module provides a set of functions designed for scenarios where different users or machines work with local or cloud‑synced folders, avoiding the need to hard‑code machine‑specific paths.

This module is provided as <ins>Hybrid</ins> implementation, supporting both *Single monolithic file* and *Modular multi-file*.


## 📘 Description

The module resolves a valid local base folder by checking a user‑defined list of possible root paths. The system evaluates a list of candidate folders, checks which ones exist on the current machine, and returns the first valid one. Once resolved, this base path can be reused throughout your queries to build portable, machine‑independent root paths.

An helper function is also included to join a relative path with the resolved root folder, ensuring consistent path construction across environments.

This module is particularly useful for **checking whether a folder exists** and/or when multiple users collaborate on the same Excel / Power BI / Power Query project and **each machine stores files in different root locations** (different usernames, sync tools, root folder structures, etc).


## 📦 Module Structure
| File | Description | Output |
|-|-|-|
| **rawLocalFolderList.powerquerym** | List of possible local root folders for each user/machine. | list (list of paths) |
| **LocalFolderExists.powerquerym** | Checks whether a folder exists on the current machine. | logical (`true` = exists) |
| **TeamUserLocalFolder.powerquerym** | Returns the first folder path in the list that exists on the current machine; if no folder exists, returns `null`. | text (folder path) |
| **TeamUserLocalFolder_full.powerquerym** | <ins>Single monolithic file</ins> version containing all 3 previous logic in one file. | text (folder path) |
| **JoinLocalPath.powerquerym** | Combines the required relative path with an optional root path, defaulting to the automatically resolved base path when no custom root path is provided. | text (full combined path) |


## ⚙️ Function & Parameters
| Function | Parameter | Type | Required | Description |
|-|-|-|-|-|
| `rawLocalFolderList` | *none* | | | The list of possible local root folders for each user/machine. |
| `LocalFolderExists` | `LocalFolderPath` | text | Yes | Folder path to test. |
| `TeamUserLocalFolder` | *none* | | | Uses `rawLocalFolderList` and `LocalFolderExists` funtion to return the first valid folder. |
| `TeamUserLocalFolder` (full) | *none* | | | Same behavior as `TeamUserLocalFolder`, but <ins>Single monolithic file</ins>. |
| `JoinLocalPath` | `relativePath` | text | Yes | The relative path to append. |
| | `customPrefix` | text | No | Optional custom root folder; defaults to `TeamUserLocalFolder`. | 


## 💡 Usage Examples
<details>
<summary>Check if a folder exists (folder does not exist)</summary>

```PowerQueryM
LocalFolderExists("C:\folder\that\does\not\exist")
```
- On `dev1`’s machine/session, this returns: `false`
- On `dev2`’s machine/session, this returns: `false`
- On `dev3`’s machine/session, this returns: `false`

---
</details>

<details>
<summary>Check if a folder exists (folder exists)</summary>

```PowerQueryM
LocalFolderExists("C:\Users\dev1\Desktop\syncedFolder")
```
- On `dev1`’s machine/session, this returns: `true`
- On `dev2`’s machine/session, this returns: `false`
- On `dev3`’s machine/session, this returns: `false`

---
</details>

<details>
<summary>Get the first valid local folder</summary>

```PowerQueryM
TeamUserLocalFolder
```
- On `dev1`’s machine/session, this returns: `C:\Users\dev1\Desktop\syncedFolder`
- On `dev2`’s machine/session, this returns: `C:\Users\dev2\Downloads`
- On `dev3`’s machine/session, this returns: `C:\Users\dev3\Downloads\dati\`

---
</details>

<details>
<summary>Join a relative path to the resolved base folder</summary>

```PowerQueryM
JoinLocalPath("repo\sales.csv")
```
- On `dev1`’s machine/session, this returns: `C:\Users\dev1\Desktop\syncedFolder\repo\sales.csv`
- On `dev2`’s machine/session, this returns: `C:\Users\dev2\Downloads\repo\sales.csv`
- On `dev3`’s machine/session, this returns: `C:\Users\dev3\Downloads\dati\repo\sales.csv`

---
</details>

<details>
<summary>Use a custom prefix instead of the auto‑resolved one</summary>

```PowerQueryM
JoinLocalPath(
    "logs\events.json",
    "D:\SharedWorkspace"
)
```
Returns: `D:\SharedWorkspace\logs\events.json`

---
</details>

## 🔗 References
| Function | Descrioption |
|-|-|
| [File.Contents](https://learn.microsoft.com/en-us/powerquery-m/file-contents)  | Documentation |
| [List.Transform](https://learn.microsoft.com/en-us/powerquery-m/list-transform)  | Documentation |
| [Table.FromList](https://learn.microsoft.com/en-us/powerquery-m/table-fromlist)  | Documentation |
| [Text functions](https://learn.microsoft.com/en-us/powerquery-m/text-functions)  | Documentation |
