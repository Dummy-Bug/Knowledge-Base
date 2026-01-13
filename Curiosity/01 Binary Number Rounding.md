[[01 Binary Rounding.pdf]]
# Binary vs Decimal (why floats lie and why DynamoDB wants Decimal)

## What is a "number" to a computer
A computer does not store numbers like humans do.  
It stores bits: `0` and `1`. Everything is built from that.

---

## How humans write numbers (base-10)
Example:
```

123.45  
= 1 × 10^2 + 2 × 10^1 + 3 × 10^0 + 4 × 10^-1 + 5 × 10^-2

```
This system is base-10 and it represents decimal fractions exactly when the denominator's prime factors are 2 and/or 5.

---

## How computers store numbers (base-2 / binary)
Computers use base-2. Example:
```

101.01₂  
= 1 × 2^2 + 0 × 2^1 + 1 × 2^0 + 0 × 2^-1 + 1 × 2^-2

```

### Key problem
Some decimal fractions cannot be represented exactly in binary.  
Just like `1/3` is repeating in base-10 (`0.3333...`), numbers like `0.1` repeat in base-2.

Binary representation of `0.1`:
```

0.000110011001100110011...

```
It repeats forever.

---

## What floats do
Floats store a binary approximation. They cut the repeating binary expansion after a fixed number of bits and store that approximation. So `0.1` is actually stored as something like:
```

0.10000000000000000555...

```
You rarely see the raw value, but that tiny difference exists.

---

## The famous example
Mathematical:
```

0.1 + 0.2 = 0.3

````
In Python:
```py
>>> 0.1 + 0.2
0.30000000000000004
````

This is a consequence of binary rounding error.

---

## What "binary rounding" means

A decimal number is forced into binary, its infinite repeating representation is truncated, and the result is rounded.  
Consequences:

- Errors accumulate
    
- Equality checks break (`0.1 + 0.2 != 0.3`)
    
- Money and financial logic become unsafe with floats
    

---

## Why this is unacceptable for money

If you add `₹0.10` one million times using floats you may not get exactly `₹100,000`. Banks and financial systems require exactness. Small floating errors are not tolerated.

---

## What `Decimal` does differently

`Decimal` uses base-10 arithmetic (from Python's `decimal` module). Example:

```py
from decimal import Decimal
Decimal("0.1") + Decimal("0.2")  # Decimal('0.3')
```

No hidden rounding. Decimal stores and computes in base-10 so decimal fractions remain exact.

**"No binary rounding"** means numbers are stored and computed in base-10. No infinite binary expansion. No truncation. No hidden error.

---

## Why DynamoDB forces `Decimal`

DynamoDB represents numbers as exact decimals. Accepting native binary floats would risk precision loss and corrupt numeric data (especially money). AWS requires `Decimal` to avoid ambiguity. SDKs (e.g., `boto3`) reject Python `float` types.

---

## Simple mental model

```
float   = fast, approximate, unsafe for money
Decimal = slower, exact, safe
```

---

## Deeper: why 0.1 cannot be written exactly in binary (rule of bases)

### 1. The rule of bases

- Base-10 (10 = 2 × 5). Fractions whose denominators have only factors 2 and/or 5 terminate in base-10 (e.g., 1/2 = 0.5, 1/5 = 0.2).
    
- If the denominator includes other primes (e.g., 3), the decimal repeats (1/3 = 0.333...).
    
- Base-2 (2) only has prime factor 2. Any fraction whose denominator is not a power of 2 will repeat in binary.  
    Example: `1/10` (0.1) has factor 5, so it repeats in binary.
    

### 2. The binary "divide by 2" process (intuition)

To represent 0.1 in binary you try to express it as sums of 1/2, 1/4, 1/8, 1/16, ...

- 1/2 (0.5) too big
    
- 1/4 (0.25) too big
    
- 1/8 (0.125) too big
    
- 1/16 (0.0625) fits → remainder 0.0375
    
- 1/32 (0.03125) fits → remainder 0.00625
    
- and so on...  
    You never reach zero. The binary expansion repeats forever.
    

### 3. Visual pattern

Long division for `1 ÷ 10` in binary yields:

```
0.00011001100110011...
```

A repeating pattern. The computer stores a truncated version (e.g., 64 bits) and that truncation produces the tiny rounding error. Summing two such approximations gives results like `0.30000000000000004`.

---

## Quick experiments to remember

```py
print(0.1 + 0.2 == 0.3)                # False
from decimal import Decimal
print(Decimal("0.1") + Decimal("0.2") == Decimal("0.3"))  # True
```

---

## Practical rule for engineering

- Treat floats as approximate. Use them for things where tiny error is acceptable (graphics, physics, some scientific computing).
    
- For money, counts, IDs, or where exact equality matters, use `Decimal` or integer cents.
    
- When storing numbers in DynamoDB, always send `Decimal` (or serialized strings) — do not send Python `float`.
    