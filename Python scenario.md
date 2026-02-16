### 1. Log File Analyzer

Your system generates a log file:

```
2026-01-10 10:00:21 INFO User 101 logged in
2026-01-10 10:02:11 ERROR Payment failed for User 203
2026-01-10 10:03:55 INFO User 101 logged out
```

**Task**

* Count number of INFO, ERROR, WARNING
* Extract unique user IDs
* Print top 3 most frequent users


### 2. Data Cleanup Pipeline

You receive:

```
["  Apple ", "banana", None, "APPLE", "Banana", ""]
```

**Task**

* Remove null/empty
* Normalize case
* Remove duplicates
* Output sorted list


### 3. Transaction Validator

You receive:

```
[1200, -300, 500, -2000, 700]
```

Rules:

* Negative values > 1000 → suspicious
* Calculate final balance
* Print suspicious count


### 4. Student Marks Processor

Input:

```
{
 "A": [78, 92, 88],
 "B": [45, 67, 55],
 "C": [95, 91, 89]
}
```

**Task**

* Compute average per student
* Rank students
* Return topper


### 5. Inventory Tracker

Products:

```
("Laptop", 5)
("Mouse", 10)
("Laptop", -2)
("Keyboard", 4)
```

**Task**
Maintain stock dynamically and print final inventory.


### 6. Duplicate Detection (Performance Based)

You receive 1 million IDs.

**Task**
Find duplicates efficiently.

Constraint:

* Time complexity < O(n²)


### 7. Bank Account System

Design classes:

* Account
* SavingsAccount
* CurrentAccount

Features:

* deposit()
* withdraw()
* transaction history
* savings interest calculation


### 8. E-Commerce Cart

Design:

* Product
* Cart
* User

Features:

* add/remove items
* apply discount
* calculate total
* generate invoice


### 9. Library Management Mini System

Rules:

* Max 3 books per user
* Late return fine
* Track issued books

**Task**
Implement using classes and dictionaries.


### 10. Browser History

Implement:

* visit(url)
* back()
* forward()

Constraint:
Use stacks only.


### 11. LRU Cache

Implement:

* get(key)
* put(key, value)
  Evict least recently used.


### 12. Task Scheduler

Given tasks with priority:

```
("Deploy", 3)
("Test", 1)
("Build", 2)
```

**Task**
Execute highest priority first.


### 13. Employee Attendance Analyzer

Input: CSV

```
emp_id, date, hours
```

**Task**

* Total hours per employee
* Overtime (>40)
* Top 3 employees


### 14. API Rate Limiter

Rule:

* Max 5 requests/min per user

**Task**
Block extra requests


### 15. Mini Chat System

Design:

* User
* Message
* ChatRoom

Features:

* Send messages
* Store history
* Fetch last N messages


### 16. Resume Parser

Input: text file with mixed data

**Task**
Extract:

* emails
* phone numbers
* skills


### 17. Sales Analytics Dashboard

Input:

```
[
 {"region":"North","sales":1000},
 {"region":"South","sales":1500}
]
```

**Task**

* Region-wise total
* Best region
* Percentage contribution


### 18. File Backup Utility

Folder with files.

**Task**

* Copy only files modified today
* Rename with timestamp
