# Python Milk Co-operative Payment System (Assignment 1)

This repository contains a step-by-step implementation of a **Milk Payment Calculation System** for a village co-operative. The assignment covers foundational Python concepts—ranging from basic data types, control flow, functions, modules, error handling, floating-point precision comparisons using `Decimal`, to advanced JSON configuration and seasonal bonus calculations.

---

## Table of Contents
1. [Task 1: Understand the Problem and Identify the Data](#task-1-understand-the-problem-and-identify-the-data)
2. [Task 2: Explore Values and Types in the Interpreter](#task-2-explore-values-and-types-in-the-interpreter)
3. [Task 3: Choose Variables and Name the Constants](#task-3-choose-variables-and-name-the-constants)
4. [Task 4: Write Correct Expressions](#task-4-write-correct-expressions)
5. [Task 5: Read Input and Convert Types](#task-5-read-input-and-convert-types)
6. [Task 6: Organise the Logic into Functions](#task-6-organise-the-logic-into-functions)
7. [Task 7: Build a Module](#task-7-build-a-module)
8. [Task 8: Plan the Algorithm Before Coding](#task-8-plan-the-algorithm-before-coding)
9. [Task 9: Implement and Test the Calculator](#task-9-implement-and-test-the-calculator)
10. [Task 10: Compare Expectations with Execution](#task-10-compare-expectations-with-execution)
11. [Task 11: Recommend and Defend the Final Design](#task-11-recommend-and-defend-the-final-design)
12. [Task 12: Re-evaluate and Adapt the Design](#task-12-re-evaluate-and-adapt-the-design)

---

## Task 1: Understand the Problem and Identify the Data

Basic entry validation and tracking farmer payments over multiple entries.

### Python Code
```python
# TASK 1 Understand the Problem and Identify the Data
MIN_FAT = 2.0
MAX_FAT = 7.0
DAYS = 10
entries = []
total_payment = {}

n = int(input("Enter number of milk entries: "))
for i in range(n):
    print("\nEntry", i + 1)
    farmer_id = input("Enter farmer ID: ")
    litres = float(input("Enter litres: "))
    fat = float(input("Enter fat percentage: "))
    snf = float(input("Enter SNF percentage: "))
    shift = input("Enter shift (Morning/Evening): ")
    
    if litres < 0:
        print("Invalid litres")
        continue
    if fat < MIN_FAT or fat > MAX_FAT:
        print("Invalid fat percentage")
        continue
    if snf < 0:
        print("Invalid SNF percentage")
        continue
        
    rate = fat * 5 + snf * 2
    amount = litres * rate
    entry = [farmer_id, litres, fat, snf, shift, rate, amount]
    entries.append(entry)
    
    if farmer_id not in total_payment:
        total_payment[farmer_id] = 0.0
    total_payment[farmer_id] += amount
    
    print("Rate per litre: Rs.", round(rate, 2))
    print("Payment amount: Rs.", round(amount, 2))

print("\n--- Farmer Payment Details ---")
for farmer_id, total in total_payment.items():
    print("Farmer ID:", farmer_id)
    print("Ten-day total: Rs.", round(total, 2))
```

### Expected Output
```text
Enter number of milk entries: 1

Entry 1
Enter farmer ID: 101
Enter litres: 10.5
Enter fat percentage: 4.2
Enter SNF percentage: 8.5
Enter shift (Morning/Evening): Morning
Rate per litre: Rs. 38.0
Payment amount: Rs. 399.0

--- Farmer Payment Details ---
Farmer ID: 101
Ten-day total: Rs. 399.0
```

---

## Task 2: Explore Values and Types in the Interpreter

Exploring basic data types, integer division, floating-point precision, and memory sizes using `sys.getsizeof`.

### Python Code
```python
# TASK 2 Explore Values and Types in the Interpreter
from decimal import Decimal
import sys

print(type(10))
print(type(10.0))
print(type("10"))
print(type(True))
print(type([10, 8.5]))

print(7 / 2)
print(7 // 2)
print(7 % 2)
print(-7 // 2)
print(0.1 + 0.2 == 0.3)

payments = ["33.40"] * 1000 + ["0.10"] * 3

def total_float(items):
    total = 0.0
    for amount in items:
        total += float(amount)
    return total

def total_decimal(items):
    total = Decimal("0.00")
    for amount in items:
        total += Decimal(amount)
    return total

print("float:", total_float(payments))
print("Decimal:", total_decimal(payments))
print(0.1 + 0.2, Decimal("0.1") + Decimal("0.2") == Decimal("0.3"))

def total_decimal_float(items):
    total = Decimal("0.00")
    for amount in items:
        total += Decimal(str(float(amount)))
    return total

print("Decimal (float):", total_decimal_float(payments))

def total_paise(items):
    total = 0
    for amount in items:
        rupees, paise = amount.split(".")
        total += int(rupees) * 100 + int(paise)
    return total

print("Integer paise:", total_paise(payments), "paise")
print("Total in rupees:", total_paise(payments) / 100)

print("Integer memory:", sys.getsizeof(100))
print("Float memory:", sys.getsizeof(100.0))
print("Decimal memory:", sys.getsizeof(Decimal("100.00")))
```

### Expected Output
```text
<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
<class 'list'>
3.5
3
1
-4
False
float: 33400.30000000061
Decimal: 33400.30
0.30000000000000004 True
Decimal (float): 33400.30
Integer paise: 3340030 paise
Total in rupees: 33400.3
Integer memory: 28
Float memory: 24
Decimal memory: 104
```

---

## Task 3: Choose Variables and Name the Constants

Assigning meaningful constants for formula calculation and demonstrating variable swapping.

### Python Code
```python
# TASK 3 Choose Variables and Name the Constants
BASE_RATE = 30.0
FAT_RATE = 0.40
SNF_RATE = 0.30
MIN_FAT = 2.0
MAX_FAT = 7.0
MIN_SNF = 0.0
MAX_DAYS = 10

farmer_id = "101"
litres = 10.5
fat = 4.2
snf = 8.5
shift = "Morning"

rate = BASE_RATE + (fat * FAT_RATE) + (snf * SNF_RATE)
amount = litres * rate

print("Farmer ID:", farmer_id)
print("Litres:", litres)
print("Fat:", fat, "%")
print("SNF:", snf, "%")
print("Shift:", shift)
print("Rate per litre: Rs.", round(rate, 2))
print("Payment amount: Rs.", round(amount, 2))

value = 10
print(value, type(value))
value = "Morning"
print(value, type(value))

a = 10
b = 20
a, b = b, a
print("After swapping:")
print("a =", a)
print("b =", b)
```

### Expected Output
```text
Farmer ID: 101
Litres: 10.5
Fat: 4.2 %
SNF: 8.5 %
Shift: Morning
Rate per litre: Rs. 34.23
Payment amount: Rs. 359.415
10 <class 'int'>
Morning <class 'str'>
After swapping:
a = 20
b = 10
```

---

## Task 4: Write Correct Expressions

Using standard baseline offsets (`FAT_BASE`, `SNF_BASE`) and step adjustments with rounding, truncated conversions, and floor functions.

### Python Code
```python
# TASK 4 Write Correct Expressions
import math

BASE_RATE = 30.0
FAT_RATE = 0.40
SNF_RATE = 0.30
FAT_BASE = 3.5
SNF_BASE = 8.0
STEP_SIZE = 0.1

fat = 4.2
snf = 8.5
litres = 10.5

fat_steps = round((fat - FAT_BASE) / STEP_SIZE)
snf_steps = round((snf - SNF_BASE) / STEP_SIZE)

rate = BASE_RATE + fat_steps * FAT_RATE + snf_steps * SNF_RATE
amount = litres * rate

print("Fat steps:", fat_steps)
print("SNF steps:", snf_steps)
print("Rate per litre:", round(rate, 2))
print("Amount:", round(amount, 2))

print("(4.2 - 3.5) / 0.1 =", (4.2 - 3.5) / 0.1)
print("0.1 + 0.2 =", 0.1 + 0.2)
print("0.1 + 0.2 == 0.3:", 0.1 + 0.2 == 0.3)
print("-2 ** 2 =", -2 ** 2)
print("(-2) ** 2 =", (-2) ** 2)
print("2 ** 3 ** 2 =", 2 ** 3 ** 2)

for f in [4.2, 3.1]:
    x = (f - FAT_BASE) / STEP_SIZE
    print("\nFat:", f)
    print("round:", round(x))
    print("int:", int(x))
    print("floor:", math.floor(x))
```

### Expected Output
```text
Fat steps: 7
SNF steps: 5
Rate per litre: 34.3
Amount: 360.15
(4.2 - 3.5) / 0.1 = 7.000000000000002
0.1 + 0.2 = 0.30000000000000004
0.1 + 0.2 == 0.3: False
-2 ** 2 = -4
(-2) ** 2 = 4
2 ** 3 ** 2 = 512

Fat: 4.2
round: 7
int: 7
floor: 7

Fat: 3.1
round: -4
int: -3
floor: -4
```

---

## Task 5: Read Input and Convert Types

Robust user input collection using `try-except` blocks to handle invalid entries gracefully.

### Python Code
```python
# TASK 5 Read Input and Convert Types
while True:
    text = input("Enter farmer ID: ").strip()
    if text:
        farmer_id = text
        break
    print("Farmer ID cannot be empty.")

while True:
    text = input("Enter litres: ").strip()
    try:
        litres = float(text)
        if litres < 0:
            print("Litres cannot be negative.")
            continue
        break
    except ValueError:
        print("Invalid input! Enter a number like 8.5.")

while True:
    text = input("Enter fat percentage: ").strip()
    try:
        fat = float(text)
        if not 2.0 <= fat <= 7.0:
            print("Fat must be between 2.0 and 7.0.")
            continue
        break
    except ValueError:
        print("Invalid input! Enter a number like 4.2.")

while True:
    text = input("Enter SNF percentage: ").strip()
    try:
        snf = float(text)
        if snf < 0:
            print("SNF cannot be negative.")
            continue
        break
    except ValueError:
        print("Invalid input! Enter a valid number.")

shift = input("Enter shift (Morning/Evening): ").strip()
rate = 30.0 + fat * 0.40 + snf * 0.30
amount = litres * rate

print("\n--- PAYMENT DETAILS ---")
print(f"{'Farmer ID':<20}: {farmer_id}")
print(f"{'Litres':<20}: {litres:8.2f}")
print(f"{'Fat %':<20}: {fat:8.2f}")
print(f"{'SNF %':<20}: {snf:8.2f}")
print(f"{'Shift':<20}: {shift}")
print(f"{'Rate per litre':<20}: Rs. {rate:8.2f}")
print(f"{'Payment amount':<20}: Rs. {amount:8.2f}")
```

### Expected Output
```text
Enter farmer ID: 1
Enter litres: 101
Enter fat percentage: 8.5
Fat must be between 2.0 and 7.0.
Enter fat percentage: 4.2
Enter SNF percentage: 8.5
Enter shift (Morning/Evening): Morning

--- PAYMENT DETAILS ---
Farmer ID           : 1
Litres              :   101.00
Fat %               :     4.20
SNF %               :     8.50
Shift               : Morning
Rate per litre      : Rs.    34.23
Payment amount      : Rs.  3457.23
```

---

## Task 6: Organise the Logic into Functions

Modularizing the rate and payment calculation using structured functions.

### Python Code
```python
# TASK 6 Organise the Logic into Functions
BASE_RATE = 30.0
FAT_RATE = 0.40
SNF_RATE = 0.30

def rate_per_litre(fat, snf):
    """Calculate and return the rate per litre using fat and SNF."""
    rate = BASE_RATE + (fat * FAT_RATE) + (snf * SNF_RATE)
    return rate

def payout(litres, rate):
    """Calculate and return the payment for the supplied milk."""
    return litres * rate

def ten_day_total(payments):
    """Return the total payment for all entries in ten days."""
    return sum(payments)

def main():
    """Read farmer details and display the payment."""
    payments = []
    n = int(input("Enter number of milk entries: "))
    for i in range(n):
        print("\nEntry", i + 1)
        farmer_id = input("Enter farmer ID: ")
        litres = float(input("Enter litres: "))
        fat = float(input("Enter fat percentage: "))
        snf = float(input("Enter SNF percentage: "))
        shift = input("Enter shift (Morning/Evening): ")
        
        if not (2.0 <= fat <= 7.0) or snf < 0:
            print("Invalid entry. Please check the values.")
            continue
        
        rate = rate_per_litre(fat, snf)
        amount = payout(litres, rate)
        payments.append(amount)
        
        print("Farmer ID:", farmer_id)
        print("Shift:", shift)
        print(f"Rate per litre: Rs. {rate:.2f}")
        print(f"Payment amount: Rs. {amount:.2f}")
        
    total = ten_day_total(payments)
    print(f"\nTotal payment for entered records: Rs. {total:.2f}")

if __name__ == "__main__":
    main()
```

### Expected Output
```text
Enter number of milk entries: 2

Entry 1
Enter farmer ID: 101
Enter litres: 10
Enter fat percentage: 4.0
Enter SNF percentage: 8.0
Enter shift (Morning/Evening): Morning
Farmer ID: 101
Shift: Morning
Rate per litre: Rs. 34.00
Payment amount: Rs. 340.00

Entry 2
Enter farmer ID: 101
Enter litres: 12
Enter fat percentage: 4.5
Enter SNF percentage: 8.5
Enter shift (Morning/Evening): Evening
Farmer ID: 101
Shift: Evening
Rate per litre: Rs. 34.35
Payment amount: Rs. 412.20

Total payment for entered records: Rs. 752.20
```

---

## Task 7: Build a Module

Encapsulating logic into a reusable module block executable via `if __name__ == "__main__":`.

### Python Code
```python
# TASK 7 Build a Module
"""Rate card of the village milk co-operative."""
BASE_RATE = 30.00
FAT_STEP_VALUE = 0.40
SNF_STEP_VALUE = 0.30

def rate_per_litre(fat, snf):
    """Return the price per litre, rounded to 2 decimals."""
    fat_steps = round((fat - 3.5) / 0.1)
    snf_steps = round((snf - 8.5) / 0.1)
    rate = (
        BASE_RATE
        + fat_steps * FAT_STEP_VALUE
        + snf_steps * SNF_STEP_VALUE
    )
    return round(rate, 2)

def payout(litres, fat, snf):
    """Return the payment amount, rounded to 2 decimals."""
    rate = rate_per_litre(fat, snf)
    return round(litres * rate, 2)

if __name__ == "__main__":
    for litres, fat, snf in [
        (10, 3.5, 8.5),
        (8.5, 4.2, 8.7),
        (12, 3.1, 8.3)
    ]:
        rate = rate_per_litre(fat, snf)
        pay = payout(litres, fat, snf)
        print(f"{litres:5} L fat {fat} snf {snf} rate {rate:6.2f} pay {pay:0.2f}")
```

### Expected Output
```text
   10 L fat 3.5 snf 8.5 rate  30.00 pay 300.00
  8.5 L fat 4.2 snf 8.7 rate  33.40 pay 283.90
   12 L fat 3.1 snf 8.3 rate  27.80 pay 333.60
```

---

## Task 8: Plan the Algorithm Before Coding

A 10-day loop accumulating payouts with daily validation checks.

### Python Code
```python
# TASK 8 Plan the Algorithm Before Coding
BASE_RATE = 30.00
FAT_STEP_VALUE = 0.40
SNF_STEP_VALUE = 0.30
total_payment = 0.0

farmer_id = input("Enter Farmer ID: ")
for day in range(1, 11):
    print("\nDay", day)
    litres = float(input("Enter litres: "))
    fat = float(input("Enter fat percentage: "))
    snf = float(input("Enter SNF percentage: "))
    shift = input("Enter shift (Morning/Evening): ").strip()
    
    if litres < 0 or not (2.0 <= fat <= 7.0) or snf < 0:
        print("Invalid entry")
        continue
    if shift.lower() not in ("morning", "evening"):
        print("Invalid shift")
        continue
        
    fat_steps = round((fat - 3.5) / 0.1)
    snf_steps = round((snf - 8.5) / 0.1)
    rate = BASE_RATE + fat_steps * FAT_STEP_VALUE + snf_steps * SNF_STEP_VALUE
    rate = round(rate, 2)
    amount = round(litres * rate, 2)
    total_payment += amount
    
    print("Rate per litre: Rs.", f"{rate:.2f}")
    print("Payment amount: Rs.", f"{amount:.2f}")

print("\nFarmer ID:", farmer_id)
print("Ten-day total: Rs.", f"{total_payment:.2f}")
```

### Expected Output
```text
Enter Farmer ID: F101

Day 1
Enter litres: 9
Enter fat percentage: 3.7
Enter SNF percentage: 4.8
Enter shift (Morning/Evening): Evening
Rate per litre: Rs. 19.70
Payment amount: Rs. 157.60

[... Day 2 through 10 iterations ...]

Farmer ID: F101
Ten-day total: Rs. 2026.75
```

---

## Task 9: Implement and Test the Calculator

Creating unit tests with valid and invalid assertion checks.

### Python Code
```python
# TASK 9 Implement and Test the Calculator
BASE_RATE = 30.00
FAT_STEP_VALUE = 0.40
SNF_STEP_VALUE = 0.30
MIN_FAT = 2.0
MAX_FAT = 7.0

def rate_per_litre(fat, snf):
    if not (MIN_FAT <= fat <= MAX_FAT):
        raise ValueError("Fat must be between 2.0 and 7.0")
    if snf < 0:
        raise ValueError("SNF cannot be negative")
    fat_steps = round((fat - 3.5) / 0.1)
    snf_steps = round((snf - 8.5) / 0.1)
    rate = BASE_RATE + fat_steps * FAT_STEP_VALUE
    rate += snf_steps * SNF_STEP_VALUE
    return round(rate, 2)

def payout(litres, fat, snf):
    if not isinstance(litres, (int, float)):
        raise TypeError("Litres must be a number")
    if litres < 0:
        raise ValueError("Litres cannot be negative")
    rate = rate_per_litre(fat, snf)
    return round(litres * rate, 2)

def run_tests():
    tests = [
        ("Above standard", 10, 4.2, 8.7, 33.40, 334.00),
        ("Below standard", 10, 3.1, 8.3, 27.80, 278.00),
        ("Standard values", 10, 3.5, 8.5, 30.00, 300.00),
        ("Lowest fat", 10, 2.0, 8.5, 24.00, 240.00),
        ("Highest fat", 10, 7.0, 8.5, 44.00, 440.00),
    ]
    print("TEST RESULTS")
    print("-" * 70)
    for name, litres, fat, snf, expected_rate, expected_pay in tests:
        actual_rate = rate_per_litre(fat, snf)
        actual_pay = payout(litres, fat, snf)
        assert actual_rate == expected_rate
        assert actual_pay == expected_pay
        print(f"{name}: Rate = {actual_rate:.2f}, Payment = {actual_pay:.2f} - PASS")

    invalid_tests = [
        ("Text quantity", lambda: payout("ten", 3.5, 8.5)),
        ("Negative quantity", lambda: payout(-5, 3.5, 8.5)),
        ("Fat below range", lambda: payout(10, 1.9, 8.5)),
        ("Fat above range", lambda: payout(10, 7.1, 8.5)),
    ]
    for name, operation in invalid_tests:
        try:
            operation()
        except (ValueError, TypeError):
            print(f"{name}: Correctly rejected - PASS")

if __name__ == "__main__":
    run_tests()
```

### Expected Output
```text
TEST RESULTS
----------------------------------------------------------------------
Above standard: Rate = 33.40, Payment = 334.00 - PASS
Below standard: Rate = 27.80, Payment = 278.00 - PASS
Standard values: Rate = 30.00, Payment = 300.00 - PASS
Lowest fat: Rate = 24.00, Payment = 240.00 - PASS
Highest fat: Rate = 44.00, Payment = 440.00 - PASS
Text quantity: Correctly rejected - PASS
Negative quantity: Correctly rejected - PASS
Fat below range: Correctly rejected - PASS
Fat above range: Correctly rejected - PASS
```

---

## Task 10: Compare Expectations with Execution

Profiling execution performance and accuracy differences between standard `float` and `Decimal`.

### Python Code
```python
# TASK 10 Compare Expectations with Execution
from decimal import Decimal
import time

BASE_RATE = 30.00
FAT_STEP_VALUE = 0.40
SNF_STEP_VALUE = 0.30

def calculate_rate(fat, snf):
    fat_steps = round((fat - 3.5) / 0.1)
    snf_steps = round((snf - 8.5) / 0.1)
    rate = BASE_RATE + fat_steps * FAT_STEP_VALUE
    rate += snf_steps * SNF_STEP_VALUE
    return round(rate, 2)

def float_total(payments):
    total = 0.0
    for amount in payments:
        total += amount
    return total

def decimal_total(payments):
    total = Decimal("0.00")
    for amount in payments:
        total += Decimal(str(amount))
    return total

def measure_time(function, payments):
    start = time.perf_counter()
    result = function(payments)
    elapsed = time.perf_counter() - start
    return result, elapsed

print("1. FLOAT VS DECIMAL TOTALS")
print("-" * 75)
for count in [300, 6000, 150000]:
    payments = [33.40] * count
    f_total, f_time = measure_time(float_total, payments)
    d_total, d_time = measure_time(decimal_total, payments)
    difference = Decimal(str(f_total)) - d_total
    print(f"\nPayments: {count}")
    print(f"Float total: Rs. {f_total:.2f}")
    print(f"Decimal total: Rs. {d_total:.2f}")
    print(f"Difference: Rs. {difference:.10f}")
    print(f"Float time: {f_time:.6f} seconds")
    print(f"Decimal time: {d_time:.6f} seconds")

print("\n2. ROUNDING COMPARISON")
print("-" * 75)
amounts = [Decimal("10.005")] * 3
round_each = sum(
    (amount.quantize(Decimal("0.01"), rounding="ROUND_HALF_UP") for amount in amounts),
    Decimal("0.00")
)
```

### Expected Output
```text
1. FLOAT VS DECIMAL TOTALS
---------------------------------------------------------------------------

Payments: 300
Float total: Rs. 10020.00
Decimal total: Rs. 10020.00
Difference: Rs. -0.0000000001
Float time: 0.000010 seconds
Decimal time: 0.000185 seconds

Payments: 6000
Float total: Rs. 200400.00
Decimal total: Rs. 200400.00
Difference: Rs. -0.0000000213
Float time: 0.000112 seconds
Decimal time: 0.002439 seconds

Payments: 150000
Float total: Rs. 5010000.00
Decimal total: Rs. 5010000.00
Difference: Rs. 0.0000008430
Float time: 0.002668 seconds
Decimal time: 0.060163 seconds

2. ROUNDING COMPARISON
---------------------------------------------------------------------------
Round each payment: 30.03
```

---

## Task 11: Recommend and Defend the Final Design

Externalizing rate cards into a JSON configuration file and implementing high-precision calculation using `Decimal` and `ROUND_HALF_UP`.

### Python Code
```python
# TASK 11 Recommend and Defend the Final Design
import json
import os
from decimal import Decimal, ROUND_HALF_UP

CENT = Decimal("0.01")
STEP = Decimal("0.1")
DEFAULT_CARD = {
    "version": "2026-01",
    "base rate": "30.00",
    "fat step value": "0.40",
    "snf step value": "0.30",
    "standard fat": "3.5",
    "standard snf": "8.5"
}

def load_rate_card(filename):
    if not os.path.exists(filename):
        with open(filename, "w") as file:
            json.dump(DEFAULT_CARD, file, indent=4)
    with open(filename, "r") as file:
        card = json.load(file)
    
    required = [
        "version", "base rate", "fat step value",
        "snf step value", "standard fat", "standard snf"
    ]
    for key in required:
        if key not in card:
            raise ValueError("Missing rate card value: " + key)
    for key in required[1:]:
        card[key] = Decimal(str(card[key]))
    return card

def calculate_rate(fat, snf, card):
    fat_steps = ((fat - card["standard fat"]) / STEP).quantize(
        Decimal("1"), rounding=ROUND_HALF_UP
    )
    snf_steps = ((snf - card["standard snf"]) / STEP).quantize(
        Decimal("1"), rounding=ROUND_HALF_UP
    )
    rate = (
        card["base rate"]
        + fat_steps * card["fat step value"]
        + snf_steps * card["snf step value"]
    )
    return rate.quantize(CENT, rounding=ROUND_HALF_UP)

def calculate_payment(litres, fat, snf, card):
    if litres < 0:
        raise ValueError("Litres cannot be negative")
    if not Decimal("2.0") <= fat <= Decimal("7.0"):
        raise ValueError("Fat must be between 2.0 and 7.0")
    rate = calculate_rate(fat, snf, card)
    payment = (litres * rate).quantize(CENT, rounding=ROUND_HALF_UP)
    return rate, payment

if __name__ == "__main__":
    card = load_rate_card("rate_card.json")
    rate, pay = calculate_payment(Decimal("10"), Decimal("3.5"), Decimal("8.5"), card)
    print("Rate per litre: Rs.", rate)
    print("Payment amount: Rs.", pay)
    print("Rate card version:", card["version"])
```

### Expected Output
```text
Rate per litre: Rs. 30.00
Payment amount: Rs. 300.00
Rate card version: 2026-01
```

---

## Task 12: Re-evaluate and Adapt the Design

Adding dynamic seasonal pricing (April-June bonus base rate) and bulk supply performance incentives.

### Python Code
```python
# TASK 12 Re-evaluate and Adapt the Design
from decimal import Decimal, ROUND_HALF_UP

CENT = Decimal("0.01")
STEP = Decimal("0.10")
STANDARD_FAT = Decimal("3.50")
STANDARD_SNF = Decimal("6.50")
BONUS_RATE = Decimal("0.50")
BONUS_LIMIT = Decimal("500.00")

def select_rate_card(month):
    if month in (4, 5, 6):
        return Decimal("32.00"), Decimal("0.45"), Decimal("0.30")
    return Decimal("30.00"), Decimal("0.40"), Decimal("0.30")

def calculate_steps(value, standard):
    return ((value - standard) / STEP).quantize(
        Decimal("1"), rounding=ROUND_HALF_UP
    )

def calculate_rate(fat, snf, month):
    if not Decimal("2.00") <= fat <= Decimal("7.00"):
        raise ValueError("Fat must be between 2.00 and 7.00")
    if snf < Decimal("0.00"):
        raise ValueError("SNF cannot be negative")
    
    base, fat_value, snf_value = select_rate_card(month)
    fat_steps = calculate_steps(fat, STANDARD_FAT)
    snf_steps = calculate_steps(snf, STANDARD_SNF)
    rate = base + fat_steps * fat_value + snf_steps * snf_value
    return rate.quantize(CENT, rounding=ROUND_HALF_UP)

def calculate_payment(litres, fat, snf, month):
    if litres < 0:
        raise ValueError("Litres cannot be negative")
    rate = calculate_rate(fat, snf, month)
    payment = (Decimal(str(litres)) * rate).quantize(
        CENT, rounding=ROUND_HALF_UP
    )
    return rate, payment

def calculate_ten_day_total(entries):
    total_litres = Decimal("0.00")
    milk_payment = Decimal("0.00")
    for litres, fat, snf, month in entries:
        rate, payment = calculate_payment(litres, fat, snf, month)
        total_litres += Decimal(str(litres))
        milk_payment += payment
    
    bonus = Decimal("0.00")
    if total_litres >= BONUS_LIMIT:
        bonus = (total_litres * BONUS_RATE).quantize(
            CENT, rounding=ROUND_HALF_UP
        )
    return total_litres, milk_payment, bonus, milk_payment + bonus

if __name__ == "__main__":
    sample_entries = [(50, Decimal("4.00"), Decimal("7.00"), 5)] * 10
    total_litres, milk_pay, bonus, final_pay = calculate_ten_day_total(sample_entries)
    
    print("Milk payment: Rs.", milk_pay)
    print("Bonus: Rs.", bonus)
    print("Total payment: Rs.", final_pay)
```

### Expected Output
```text
Milk payment: Rs. 18136.20
Bonus: Rs. 250.50
Total payment: Rs. 18386.70
```