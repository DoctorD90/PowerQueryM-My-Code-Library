# 📂 Time Table
A Power Query (M) function that generates a datetimes table between two boundaries, using a configurable step duration and optional additional entries.

This module is provided as <ins>Single monolithic file</ins> implementation.


## 📘 Description
The module creates a sequential list of datetime values starting from a given datetime and ending at another. If the end datetime is omitted, the function automatically uses the current system datetime. You can also let it append extra elements (default: 0) after the end and customize the step duration (default: 1 day).

Additionally, if the specified end datetime does not align with the defined step duration, the sequence is automatically extended to the next valid step boundary, ensuring full coverage before any extra elements are appended.

This function is useful for generating **time tables**, **sampling sequences**, **rolling windows**, or **custom datetime scaffolding**.


## 📦 Module Structure
| File | Description | Output |
|-|-|-|
| **FnDateTimes.powerquerym** | Main function generating the datetime table. | table (datetimes) |


## ⚙️ Function & Parameters
| Function | Parameter | Type | Required | Description |
|-|-|-|-|-|
| `FnDateTimes` | `TheStartDate` | datetime | Yes | Starting datetime of the sequence. |
| | `TheEndDate` | datetime | No | Ending datetime; if `null`, the current system datetime is used. |
| | `Interval` | duration | No | Time increment between elements; defaults to 1 day. | 
| | `ElementsToAdd` | number | No | Additional elements to append after the `TheEndDate`; if `0`, no additional dates will appended. |


## 💡 Usage Example
<details>
<summary>Basic usage</summary>

```PowerQueryM
FnDateTimes(
  #datetime(2023,1,20, 0,0,0)
)
```
or
```PowerQueryM
FnDateTimes(
  #datetime(2023,1,20, 0,0,0),
  null,
  null,
  null
)
```
Return a datetime table with:
- **StartDate:** 2023/01/20 00:00:00
- **EndDate:** default (current system datetime)
- **Interval:** default (1 day)
- **Additional steps:** default (0)

| Dates |
|-|
| 2023/01/20 00:00:00 |
| 2023/01/21 00:00:00 |
| 2023/01/22 00:00:00 |
| ... |
| ... |

---
</details>

<details>
<summary>Explicit end datetime</summary>

```PowerQueryM
FnDateTimes(
  #datetime(2023,1,20, 0,0,0),
  #datetime(2023,1,26, 5,30,10)
)
```
Return a datetime table with:
- **StartDate:** 2023/01/20 00:00:00
- **EndDate:** 2023/01/27 00:00:00
- **Interval:** default (1 day)
- **Additional steps:** default (0)

| Dates |
|-|
| 2023/01/20 00:00:00 |
| 2023/01/21 00:00:00 |
| ... |
| 2023/01/26 00:00:00 |
| 2023/01/27 00:00:00 |

---
</details>

<details>
<summary>Custom interval of 1 hour</summary>

```PowerQueryM
FnDateTimes(
  #datetime(2023,1,20, 0,0,0),
  #datetime(2023,1,26, 5,30,10),
  #duration(0, 1, 0, 0)
)
```
Return a datetime table with:
- **StartDate:** 2023/01/20 00:00:00
- **EndDate:** 2023/01/26 06:00:00
- **Interval:** 1 hour
- **Additional steps:** default (0)

| Dates |
|-|
| 2023/01/20 00:00:00 |
| 2023/01/20 01:00:00 |
| ... |
| 2023/01/26 05:00:00 |
| 2023/01/26 06:00:00 |

---
</details>

<details>
<summary>Full Example</summary>

```PowerQueryM
FnDateTimes(
  #datetime(2023,1,20, 0,0,0),
  #datetime(2023,1,26, 5,30,10),
  #duration(0, 1, 0, 0),
  10
)
```
Return a datetime table with:
- **StartDate:** 2023/01/20 00:00:00
- **EndDate:** 2023/01/26 16:00:00
- **Interval:** 1 hour
- **Additional steps:** + 10 steps after `TheEndDate` (10 hours)

| Dates |
|-|
| 2023/01/20 00:00:00 |
| 2023/01/20 01:00:00 |
| ... |
| 2023/01/26 15:00:00 |
| 2023/01/26 16:00:00 |

---
</details>


## 🔗 References
| Function | Descrioption |
|-|-|
| [List.DateTimes](https://learn.microsoft.com/en-us/powerquery-m/list-datetimes)  | Documentation |
| [#duration](https://learn.microsoft.com/en-us/powerquery-m/sharpduration) | Documentation |
