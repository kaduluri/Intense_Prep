"Mini-Problem" for each pattern and see how the code actually looks in both languages.

* * * *

### 1\. Frequency Counter Example

**Problem:** Count how many times each character appears in a string.

**Java**

Java

```
String s = "apple";
Map<Character, Integer> counts = new HashMap<>();

for (char c : s.toCharArray()) {
    // getOrDefault is the "Safety Check"
    counts.put(c, counts.getOrDefault(c, 0) + 1);
}
// Access: counts.get('p') -> 2

```

**JavaScript**

JavaScript

```
const s = "apple";
const counts = {}; // Using a plain object

for (let c of s) {
    // Logical OR || is the "Safety Check"
    counts[c] = (counts[c] || 0) + 1;
}
// Access: counts['p'] -> 2

```

* * * *

### 2\. Fast & Slow Pointer (Cycle Detection)

**Problem:** Check if a Linked List has a loop.

**Java**

Java

```
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;         // 1 step
        fast = fast.next.next;    // 2 steps
        if (slow == fast) return true; // Collision!
    }
    return false;
}

```

**JavaScript**

JavaScript

```
function hasCycle(head) {
    let slow = head, fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) return true;
    }
    return false;
}

```

* * * *

### 3\. In-Place Reversal (Full List)

**Problem:** Flip the direction of a Linked List.

**Java**

Java

```
ListNode prev = null;
ListNode curr = head;

while (curr != null) {
    ListNode next = curr.next; // Secure the future
    curr.next = prev;          // Reverse the link
    prev = curr;               // Move prev forward
    curr = next;               // Move curr forward
}
return prev; // New head

```

**JavaScript**

JavaScript

```
let prev = null;
let curr = head;

while (curr) {
    let next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
return prev;

```

* * * *

### 4\. Sliding Window (Fixed Size)

**Problem:** Find the maximum sum of $k$ consecutive elements.

**Java**

Java

```
int maxVal = 0, windowSum = 0;
for (int i = 0; i < nums.length; i++) {
    windowSum += nums[i]; // Add next person to train
    if (i >= k - 1) { // Train is full
        maxVal = Math.max(maxVal, windowSum);
        windowSum -= nums[i - k + 1]; // First person leaves train
    }
}

```

**JavaScript**

JavaScript

```
let maxVal = 0, windowSum = 0;
for (let i = 0; i < nums.length; i++) {
    windowSum += nums[i];
    if (i >= k - 1) {
        maxVal = Math.max(maxVal, windowSum);
        windowSum -= nums[i - (k - 1)];
    }
}

```

* * * *

### Summary Cheat Sheet for Logic

| **Concept** | **Java Key Syntax** | **JS Key Syntax** | **Mental Model** |
| --- |  --- |  --- |  --- |
| **Safety Check** | `.getOrDefault(k, 0)` | `map[k] \|\| 0` | Check if cubby is empty before adding. |
| --- |  --- |  --- |  --- |
| **Middle/Cycle** | `fast.next.next` | `fast.next.next` | The Hare runs twice as fast. |
| **Reversal** | `curr.next = prev` | `curr.next = prev` | Turning to the past. |
| **Sliding** | `windowSum -= nums[i-k+1]` | `sum -= nums[i-k+1]` | One enters front, one exits back. |

**Important Java Note:** Notice in the Java Reversal example, we don't use `list.get()`. In Linked List problems, you are usually given a `ListNode` object. You interact with it via `.next` and `.val`, which is very similar to how you use objects in JavaScript.