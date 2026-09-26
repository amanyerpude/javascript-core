
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** FrontendChallenges  
> **Posted in:** Interview, JavaScript, Algorithm  
> **Tags:** #interview #javascript #algorithm #search

---

# Find Highest Commodity Price Under a Timestamp

You are given a list of commodity price records, where each record contains:
- `commodity` (string) → name of the commodity
- `price` (number) → price of the commodity
- `timestamp` (number) → UNIX timestamp when the price was recorded

Implement a function that finds the **highest price of a given commodity up to a specified timestamp**.

---

## Example

```javascript
const records = [
  { commodity: "gold", price: 100, timestamp: 1 },
  { commodity: "gold", price: 120, timestamp: 3 },
  { commodity: "silver", price: 50, timestamp: 2 },
  { commodity: "gold", price: 90, timestamp: 5 },
];

findHighestPrice(records, "gold", 3);
// Expected output: 120

findHighestPrice(records, "gold", 1);
// Expected output: 100

findHighestPrice(records, "silver", 3);
// Expected output: 50

findHighestPrice(records, "silver", 1);
// Expected output: null (no records available before timestamp 1)
```

---

## Implementation

```javascript
function findHighestPrice(records, commodity, timestamp) {
  let highest = null;

  for (const record of records) {
    if (
      record.commodity === commodity &&
      record.timestamp <= timestamp
    ) {
      if (highest === null || record.price > highest) {
        highest = record.price;
      }
    }
  }

  return highest;
}
```

---

## Requirements

- If no record exists for the commodity before or at the given timestamp, return `null`.
- The function should handle **multiple commodities**.
- Timestamps are guaranteed to be **positive integers**.
- Records may **not be sorted** by timestamp.

---

> [!tip] Key Points
> - Filter records by **commodity name** and **timestamp ≤ target**.
> - Track the **maximum price** among matching records.
> - Return `null` if no matching records found.
> - Simple **linear scan** works for unsorted data.

---

> [!summary] Takeaway
> - Filter by commodity and timestamp constraint.
> - Track **maximum price** among matching records.
> - Return `null` when no records match.
> - Can be optimized with **sorting** or **indexing** for large datasets.

---

### 📎 Reference

[find-highest-commodity-price - FrontendChallenges](https://frontend-challenges.com/challenges/403-find-highest-commodity-price)

---
