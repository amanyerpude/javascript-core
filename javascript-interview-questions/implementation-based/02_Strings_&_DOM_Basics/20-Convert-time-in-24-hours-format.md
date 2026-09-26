
---

> [!quote] Metadata  
> **Posted on:** February 21, 2022  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript  
> **Tags:** #interview #javascript #string #time

---

# Convert Time in 24 Hours Format

Given a time in 12 hours format, convert it into 24 hours format.

---

## Example

```javascript
formatTime("12:10AM"); // "00:10"
formatTime("12:33PM"); // "12:33"
formatTime("01:15PM"); // "13:15"
formatTime("11:45AM"); // "11:45"
```

---

## Implementation

```javascript
const formatTime = (time) => {
  // convert the input to lowercase
  const timeLowerCased = time.toLowerCase();

  // split the hours and mins
  let [hours, mins] = timeLowerCased.split(":");

  // Special case: 12 has to be handled for both AM and PM
  if (timeLowerCased.endsWith("am")) {
    hours = hours == 12 ? "0" : hours;
  } else if (timeLowerCased.endsWith("pm")) {
    hours = hours == 12 ? hours : String(+hours + 12);
  }

  return `${hours.padStart(2, 0)}:${mins.slice(0, -2).padStart(2, 0)}`;
};
```

---

## How It Works

1. Convert input to **lowercase** for consistent comparison.
2. **Split** hours and minutes from the colon separator.
3. Handle **AM**: if hours is 12, set to 0 (midnight case).
4. Handle **PM**: if hours is not 12, add 12 to convert.
5. **Pad** single digits with leading zero using `padStart`.
6. **Slice** off the AM/PM suffix from minutes.

---

> [!tip] Key Points
> - Special case: **12 AM** = 00:00, **12 PM** = 12:00.
> - Use `padStart(2, 0)` for zero-padding single digits.
> - `endsWith("am"/"pm")` checks the time period.
> - `slice(0, -2)` removes the last 2 characters (AM/PM).

---

> [!summary] Takeaway
> - Convert 12-hour format to 24-hour by handling **AM/PM** logic.
> - Special case for **12** (midday vs midnight).
> - Use string methods: `split`, `endsWith`, `padStart`, `slice`.

---

### 📎 Reference

[Convert time in 24 hours format - LearnersBucket](https://learnersbucket.com/examples/interview/convert-format-time-in-24-hours-format/)

---
