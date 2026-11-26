# Code Challenges - Python Solutions

Complete, runnable Python 3 solutions for a set of coding challenges and scenarios (placement / interview practice).
Every program is self-contained: copy it into a file and run `python3 <file>.py`.

## Contents

| No. | Challenge / Scenario | File |
|-----|----------------------|------|
| 1 | [Find the Longest Palindromic Substring](#challenge-1-find-the-longest-palindromic-substring) | `programs/longest_palindrome.py` |
| 2 | [Squares in a Matrix](#challenge-2-squares-in-a-matrix) | `programs/squares_in_matrix.py` |
| 3 | [Implement Queue using two stacks](#challenge-3-implement-queue-using-two-stacks) | `programs/queue_two_stacks.py` |
| 4 | [Six Consecutive No Outcomes in Customer Calls](#challenge-4-six-consecutive-no-outcomes-in-customer-calls) | `programs/six_consecutive_no.py` |
| 5 | [Filter List Against Dictionary Values](#challenge-5-filter-list-against-dictionary-values) | `programs/filter_list_by_dict_values.py` |
| 6 | [Find words with both letters and numbers](#challenge-6-find-words-with-both-letters-and-numbers) | `programs/words_letters_and_numbers.py` |
| S1 | [Scenario 1: Log without dates](#scenario-1-log-without-dates) | `programs/log_without_dates.py` |
| S2 | [Scenario 2: Drop Caps](#scenario-2-drop-caps) | `programs/drop_caps.py` |
| S3 | [Scenario 3: Two numbers are positive](#scenario-3-two-numbers-are-positive) | `programs/two_numbers_positive.py` |
| S5 | [Scenario 5: Consonant value](#scenario-5-consonant-value) | `programs/consonant_value.py` |
| S6 | [Scenario 6: Generating Generators](#scenario-6-generating-generators) | `programs/generating_generators.py` |
| S7 | [Scenario 7: Zonk Game](#scenario-7-zonk-game) | `programs/zonk_game.py` |
| S8 | [Scenario 8: Walking in the NASA Facility](#scenario-8-walking-in-the-nasa-facility) | `programs/walking_nasa_facility.py` |
| S4 | Chaser's Schedule | _statement not available, not included_ |
| S9 | Same as Challenge 4 | `programs/six_consecutive_no.py` |

## How to run

```bash
git clone <your-repo-url>
cd code-challenges-python/programs
python3 longest_palindrome.py
```

- Requires Python 3.8 or newer. No external libraries are used.
- Each file has sample input inside the `if __name__ == "__main__":` block. Replace it with your own input; the functions above it stay the same.
- The original problems were given as titles only, so each solution states its assumption. If a statement differs, only the small part that differs needs changing.

---

## Challenge 1: Find the Longest Palindromic Substring

File: `programs/longest_palindrome.py`

```python
def longest_palindrome(s):
    if not s:
        return ""
    start = end = 0

    def expand(l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
        return l + 1, r - 1

    for i in range(len(s)):
        for l, r in (expand(i, i), expand(i, i + 1)):   # odd and even centres
            if r - l > end - start:
                start, end = l, r
    return s[start:end + 1]


if __name__ == "__main__":
    tests = ["babad", "cbbd", "a", "forgeeksskeegfor", "racecar"]   # change these
    for s in tests:
        print(f"Input: {s!r:22} Longest palindrome: {longest_palindrome(s)!r}")
```

**Output**

```text
Input: 'babad'                Longest palindrome: 'bab'
Input: 'cbbd'                 Longest palindrome: 'bb'
Input: 'a'                    Longest palindrome: 'a'
Input: 'forgeeksskeegfor'     Longest palindrome: 'geeksskeeg'
Input: 'racecar'              Longest palindrome: 'racecar'
```

---

## Challenge 2: Squares in a Matrix

File: `programs/squares_in_matrix.py`

```python
# Part A: count square submatrices that contain only 1s
def count_squares(m):
    R, C = len(m), len(m[0])
    dp = [[0] * C for _ in range(R)]
    total = 0
    for i in range(R):
        for j in range(C):
            if m[i][j] == 1:
                if i == 0 or j == 0:
                    dp[i][j] = 1
                else:
                    dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
                total += dp[i][j]
    return total


# Part B: count ALL squares of every size in an n x m grid
def squares_in_grid(n, m):
    return sum((n - k + 1) * (m - k + 1) for k in range(1, min(n, m) + 1))


if __name__ == "__main__":
    matrix = [
        [0, 1, 1, 1],
        [1, 1, 1, 1],
        [0, 1, 1, 1],
    ]                                                  # change this
    print("Matrix:")
    for row in matrix:
        print(" ", row)
    print("Number of all-1 squares:", count_squares(matrix))
    print("Squares in a 4x5 grid  :", squares_in_grid(4, 5))
```

**Output**

```text
Matrix:
  [0, 1, 1, 1]
  [1, 1, 1, 1]
  [0, 1, 1, 1]
Number of all-1 squares: 15
Squares in a 4x5 grid  : 40
```

---

## Challenge 3: Implement Queue using two stacks

File: `programs/queue_two_stacks.py`

```python
class MyQueue:
    def __init__(self):
        self.inp = []      # stack for pushes
        self.out = []      # stack for pops

    def push(self, x):
        self.inp.append(x)

    def _move(self):
        if not self.out:
            while self.inp:
                self.out.append(self.inp.pop())

    def pop(self):
        if self.empty():
            raise IndexError("pop from empty queue")
        self._move()
        return self.out.pop()

    def peek(self):
        if self.empty():
            raise IndexError("peek from empty queue")
        self._move()
        return self.out[-1]

    def empty(self):
        return not self.inp and not self.out

    def size(self):
        return len(self.inp) + len(self.out)


if __name__ == "__main__":
    q = MyQueue()
    for x in (10, 20, 30):
        q.push(x)
        print("push", x)
    print("peek ->", q.peek())
    print("pop  ->", q.pop())
    q.push(40)
    print("push 40")
    while not q.empty():
        print("pop  ->", q.pop())
    print("empty ->", q.empty())
```

**Output**

```text
push 10
push 20
push 30
peek -> 10
pop  -> 10
push 40
pop  -> 20
pop  -> 30
pop  -> 40
empty -> True
```

---

## Challenge 4: Six Consecutive No Outcomes in Customer Calls

File: `programs/six_consecutive_no.py`

```python
# Finds customers who have 6 or more "No" outcomes in a row (calls in time order).
#
# SQL version (if the exam asks for SQL, table calls(customer_id, call_time, outcome)):
#   SELECT DISTINCT customer_id FROM (
#     SELECT customer_id,
#            SUM(outcome = 'No') OVER (PARTITION BY customer_id ORDER BY call_time
#                ROWS BETWEEN 5 PRECEDING AND CURRENT ROW) AS c
#     FROM calls) t
#   WHERE c = 6;
from itertools import groupby


def has_six_consecutive_no(outcomes):
    return any(k == "No" and len(list(g)) >= 6 for k, g in groupby(outcomes))


def customers_with_six_no(calls):
    """calls: list of (customer_id, call_time, outcome)"""
    by_customer = {}
    for cid, t, outcome in sorted(calls, key=lambda r: (r[0], r[1])):
        by_customer.setdefault(cid, []).append(outcome)
    return [cid for cid, outs in by_customer.items() if has_six_consecutive_no(outs)]


if __name__ == "__main__":
    calls = []
    data = {
        101: "No No No No No No Yes",       # six in a row -> qualifies
        102: "No No No Yes No No No No",    # max run is 4
        103: "Yes No No No No No No No",    # seven in a row -> qualifies
        104: "No Yes No Yes No Yes",
    }
    for cid, seq in data.items():
        for i, outcome in enumerate(seq.split()):
            calls.append((cid, i, outcome))
    result = customers_with_six_no(calls)
    print("Customers with 6+ consecutive No:", result)
```

**Output**

```text
Customers with 6+ consecutive No: [101, 103]
```

---

## Challenge 5: Filter List Against Dictionary Values

File: `programs/filter_list_by_dict_values.py`

```python
def filter_list(items, d, keep=True):
    """keep=True  -> keep items that appear in the dictionary values
       keep=False -> keep items that do NOT appear in the dictionary values"""
    values = set(d.values())
    return [x for x in items if (x in values) == keep]


if __name__ == "__main__":
    items = ["apple", "banana", "cherry", "date", "fig"]            # change these
    d = {1: "banana", 2: "fig", 3: "grape"}
    print("List              :", items)
    print("Dictionary        :", d)
    print("In dict values    :", filter_list(items, d, keep=True))
    print("Not in dict values:", filter_list(items, d, keep=False))
```

**Output**

```text
List              : ['apple', 'banana', 'cherry', 'date', 'fig']
Dictionary        : {1: 'banana', 2: 'fig', 3: 'grape'}
In dict values    : ['banana', 'fig']
Not in dict values: ['apple', 'cherry', 'date']
```

---

## Challenge 6: Find words with both letters and numbers

File: `programs/words_letters_and_numbers.py`

```python
import re


def mixed_words(text):
    words = re.findall(r"[A-Za-z0-9]+", text)          # split into alphanumeric words
    return [w for w in words
            if any(c.isalpha() for c in w) and any(c.isdigit() for c in w)]


if __name__ == "__main__":
    text = "Order a12 shipped on 2026, ticket B7 and code x9y8z are ready; hello 123 world"
    print("Text  :", text)
    print("Result:", mixed_words(text))
```

**Output**

```text
Text  : Order a12 shipped on 2026, ticket B7 and code x9y8z are ready; hello 123 world
Result: ['a12', 'B7', 'x9y8z']
```

---

## Scenario 1: Log without dates

File: `programs/log_without_dates.py`

```python
import re

DATE_PATTERN = re.compile(
    r"^\[?\d{4}-\d{2}-\d{2}([ T]\d{2}:\d{2}:\d{2}(\.\d+)?)?\]?\s*"
)


def strip_date(line):
    return DATE_PATTERN.sub("", line)


if __name__ == "__main__":
    logs = [
        "2026-11-26 09:00:01 INFO Server started",
        "[2026-11-26T09:05:10] ERROR Disk almost full",
        "2026-11-26 WARN Low memory",
        "No date on this line",
    ]
    print("Log lines without dates:")
    for line in logs:
        print(" ", strip_date(line))
```

**Output**

```text
Log lines without dates:
  INFO Server started
  ERROR Disk almost full
  WARN Low memory
  No date on this line
```

---

## Scenario 2: Drop Caps

File: `programs/drop_caps.py`

```python
def drop_caps(text):
    """Capitalise the first letter of every word, keep the rest unchanged."""
    return " ".join(w[:1].upper() + w[1:] for w in text.split(" "))


if __name__ == "__main__":
    for s in ["hello big world", "python is fun", "mIxEd cAsE wOrDs", "a b c"]:
        print(f"{s!r:22} -> {drop_caps(s)!r}")
```

**Output**

```text
'hello big world'      -> 'Hello Big World'
'python is fun'        -> 'Python Is Fun'
'mIxEd cAsE wOrDs'     -> 'MIxEd CAsE WOrDs'
'a b c'                -> 'A B C'
```

---

## Scenario 3: Two numbers are positive

File: `programs/two_numbers_positive.py`

```python
def exactly_two_positive(a, b, c):
    return sum(n > 0 for n in (a, b, c)) == 2      # use >= 2 for "at least two"


if __name__ == "__main__":
    tests = [(1, 2, -3), (1, 2, 3), (-1, -2, 3), (0, 5, 6), (-1, -2, -3)]
    for a, b, c in tests:
        print(f"{(a, b, c)!s:15} -> {exactly_two_positive(a, b, c)}")
```

**Output**

```text
(1, 2, -3)      -> True
(1, 2, 3)       -> False
(-1, -2, 3)     -> False
(0, 5, 6)       -> True
(-1, -2, -3)    -> False
```

---

## Scenario 5: Consonant value

File: `programs/consonant_value.py`

```python
import re


def consonant_value(s):
    """Vowels (a, e, i, o, u) split the word into consonant chunks.
    Letter value: a=1, b=2 ... z=26. Return the highest chunk value."""
    return max(sum(ord(c) - 96 for c in chunk) for chunk in re.split("[aeiou]", s))


if __name__ == "__main__":
    for w in ["zodiac", "chruschtschov", "khrushchev", "strength", "catchphrase", "aeiou"]:
        print(f"{w:15} -> {consonant_value(w)}")
```

**Output**

```text
zodiac          -> 26
chruschtschov   -> 80
khrushchev      -> 38
strength        -> 57
catchphrase     -> 73
aeiou           -> 0
```

---

## Scenario 6: Generating Generators

File: `programs/generating_generators.py`

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b


def multiples_of(k):
    """A function that returns a generator."""
    n = 0
    while True:
        n += k
        yield n


def take(n, gen):
    return [next(gen) for _ in range(n)]


def squares_below(limit):
    n = 1
    while n * n < limit:
        yield n * n
        n += 1


if __name__ == "__main__":
    print("First 10 Fibonacci :", take(10, fibonacci()))
    print("First 5 multiples 7:", take(5, multiples_of(7)))
    print("Squares below 50   :", list(squares_below(50)))
    gen = (x * x for x in range(5))               # generator expression
    print("Generator expression:", list(gen))
```

**Output**

```text
First 10 Fibonacci : [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
First 5 multiples 7: [7, 14, 21, 28, 35]
Squares below 50   : [1, 4, 9, 16, 25, 36, 49]
Generator expression: [0, 1, 4, 9, 16]
```

---

## Scenario 7: Zonk Game

File: `programs/zonk_game.py`

```python
import random
from collections import Counter


def zonk_score(dice):
    """Scoring: 1 = 100, 5 = 50, three of a kind = face x 100 (three 1s = 1000),
    each extra die doubles it, straight 1-6 = 1500. Score 0 means ZONK."""
    if len(dice) == 6 and sorted(dice) == [1, 2, 3, 4, 5, 6]:
        return 1500
    score = 0
    for face, n in Counter(dice).items():
        if n >= 3:
            score += (1000 if face == 1 else face * 100) * (n - 2)
            n = 0
        if face == 1:
            score += 100 * n
        elif face == 5:
            score += 50 * n
    return score


def play_round(rng):
    dice = [rng.randint(1, 6) for _ in range(6)]
    score = zonk_score(dice)
    print(f"Rolled {dice} -> {'ZONK!' if score == 0 else score}")
    return score


if __name__ == "__main__":
    print("Fixed examples:")
    for d in ([1, 5, 2, 3, 4, 6], [2, 3, 4, 6, 6, 3], [1, 1, 1, 2, 3, 4],
              [2, 2, 2, 5, 3, 4], [5, 5, 5, 5, 1, 2]):
        s = zonk_score(d)
        print(f"  {d} -> {'ZONK!' if s == 0 else s}")
    print("Three random rounds:")
    rng = random.Random(7)          # remove the seed (7) for truly random rolls
    total = sum(play_round(rng) for _ in range(3))
    print("Total score:", total)
```

**Output**

```text
Fixed examples:
  [1, 5, 2, 3, 4, 6] -> 1500
  [2, 3, 4, 6, 6, 3] -> ZONK!
  [1, 1, 1, 2, 3, 4] -> 1000
  [2, 2, 2, 5, 3, 4] -> 250
  [5, 5, 5, 5, 1, 2] -> 1100
Three random rounds:
Rolled [3, 2, 4, 6, 1, 1] -> 200
Rolled [5, 1, 3, 5, 1, 5] -> 700
Rolled [2, 1, 1, 4, 4, 1] -> 1000
Total score: 1900
```

---

## Scenario 8: Walking in the NASA Facility

File: `programs/walking_nasa_facility.py`

```python
from collections import deque


def shortest_path(grid, start, goal):
    """BFS on a grid. '#' = wall, anything else is walkable. Returns (steps, path)."""
    R, C = len(grid), len(grid[0])
    q = deque([start])
    prev = {start: None}
    while q:
        cur = q.popleft()
        if cur == goal:
            path = []
            while cur is not None:
                path.append(cur)
                cur = prev[cur]
            return len(path) - 1, path[::-1]
        r, c = cur
        for nr, nc in ((r + 1, c), (r - 1, c), (r, c + 1), (r, c - 1)):
            if 0 <= nr < R and 0 <= nc < C and grid[nr][nc] != "#" and (nr, nc) not in prev:
                prev[(nr, nc)] = cur
                q.append((nr, nc))
    return -1, []


if __name__ == "__main__":
    facility = [
        "S..#....",
        ".#.#.##.",
        ".#...#..",
        ".####.#.",
        "......#G",
    ]                                              # S = start, G = goal, # = wall
    start = goal = None
    for r, row in enumerate(facility):
        for c, ch in enumerate(row):
            if ch == "S": start = (r, c)
            if ch == "G": goal = (r, c)
    steps, path = shortest_path(facility, start, goal)
    print("Start:", start, "Goal:", goal)
    print("Shortest steps:", steps)
    print("Path:", path)
    grid = [list(row) for row in facility]
    for r, c in path[1:-1]:
        grid[r][c] = "*"
    print("\n".join("".join(row) for row in grid))
```

**Output**

```text
Start: (0, 0) Goal: (4, 7)
Shortest steps: 15
Path: [(0, 0), (0, 1), (0, 2), (1, 2), (2, 2), (2, 3), (2, 4), (1, 4), (0, 4), (0, 5), (0, 6), (0, 7), (1, 7), (2, 7), (3, 7), (4, 7)]
S**#****
.#*#*##*
.#***#.*
.####.#*
......#G
```

---

## Notes

- Complexities: palindrome O(n^2); matrix squares O(R*C); queue operations amortised O(1); six-consecutive-No O(n); dictionary filter O(n + m); grid BFS O(R*C).
