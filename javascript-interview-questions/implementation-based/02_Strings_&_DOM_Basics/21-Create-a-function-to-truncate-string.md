
---

> [!quote] Metadata  
> **Posted on:** January 1, 2024  
> **Author:** Mustafoski  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #string

---

# Create a Function to Truncate String

If the length of the string is less than or equal to the given number, just return the string without truncating it. Otherwise, truncate the string — keep the beginning of the string up until the given number and discard the rest. Append "..." if truncating.

---

## Example

```javascript
let string = 'Orange';
truncateString(string, 1);  // "O..."
truncateString(string, 3);  // "Ora..."
truncateString(string, 9);  // "Orange" (no truncation needed)
```

---

## Implementation

```javascript
function truncateString(str, num) {
  if (str.length > num && num > 3) {
    return str.slice(0, num - 3) + '...';
  } else if (str.length > num && num <= 3) {
    return str.slice(0, num) + '...';
  } else {
    return str;
  }
}
```

---

## How It Works

1. If string is **longer** than num and num > 3: slice to `num - 3` and append "..." (total length = num).
2. If string is **longer** than num and num <= 3: slice to `num` and append "..." (dots may exceed num, but that's the spec).
3. If string is **shorter or equal**: return as-is, no truncation needed.

---

> [!tip] Key Points
> - When num > 3, reserve 3 characters for the "..." suffix.
> - When num <= 3, just slice and append dots regardless.
> - If the string is already short enough, return it unchanged.
> - The total output length should ideally be `num` characters.

---

> [!summary] Takeaway
> - Truncate strings that exceed a given length.
> - Account for the "..." suffix when calculating slice length.
> - Return original string if no truncation is needed.
> - Handle edge cases where the limit is very small (<= 3).

---

### 📎 Reference

[Truncate String - Interview Problems](https://mustafoski.github.io/javascript/Truncate-String/)

---
