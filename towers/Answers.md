#  MCON 264 — Recursion Assignment: Towers of Hanoi × 3


##  Overview

In this lab, you implemented three recursive versions of the Towers of Hanoi problem and (optionally) one iterative version.  
Each part reinforces a key concept of recursion — base case, recursive case, and growth pattern — and illustrates how recursion can be refactored or replaced by iteration.

---

## Part 1 — Classic Towers of Hanoi (`TowersOfHanoi.java`)

### 1. Base Case
_Describe the base condition that stops recursion (for example, what happens when `n == 0`?)._

> ✎ Your answer here
> When n equals 0, there are no disks to move, so the method immediately returns without making any moves or recursive calls.
> This prevents infinite recursion and handles the simplest case where no work needs to be done.

### 2. Recursive Case
_Explain the sequence of recursive calls and what each represents._

> ✎ Your answer here
> > The recursive case breaks down the problem into three steps:
1-Move n-1 disks from the source peg to the auxiliary peg (using destination as temporary)
2-Move the largest disk (disk n) directly from source to destination
3-Move n-1 disks from the auxiliary peg to the destination (using source as temporary)

### 3. Sample Trace (for n = 3)

| Move # | From | To |
|:------:|:----:|:--:|
|   1    |  A   | C  |
|   2    |  A   | B  |
|   3    |  C   | B  |
|   4    |  A   | C  |
|   5    |  B   | A  |
|   6    |  B   | C  |
|   7    |  A   | C  |

_Total moves = 2ⁿ − 1 = 7 (for n = 3)_

---

## Part 2 — Counting Moves (`TowersExercise21.java`)

### 1. Approach
_How did you modify the standard recursion to count rather than print moves?_

>  Your answer here
> Instead of printing each move, I used a static int count that gets incremented by 1 each time a disk is moved.
> The rest of the structure stayed the same

### 2. Verification of Formula
_Complete the table and verify that count = 2ⁿ − 1._

| n | Expected (2ⁿ − 1) | Program Output | Matches? (Y/N) |
|:--:|:--:|:--------------:|:--------------:|
| 1 | 1 |       1        |       Y        |
| 2 | 3 |       3        |       Y        |
| 3 | 7 |       7        |       Y        |
| 4 | 15 |       15       |       Y        |
| 5 | 31 |       31       |       Y        |

### 3. Reflection
_What changes when you replace printed moves with a counter? What are the pros and cons?_

> ✎ Your answer here
pro- counting is more efficient (no I/O overhead)
> con- you lose visibility into the actual move sequence, making it harder to debug or understand what the algorithm is doing step-by-step.
---

## Part 3 — Hanoi Variation (`TowersVariations.java`)

### 1. New Rule
_Every move must pass through the middle peg. How does this alter the recursion?_

> ✎ Your answer here
> The constraint that every move must use the middle peg means we cannot move directly between pegs 1 and 3. This changes the recursive structure.
> To move n disks from source to destination, we must:
Recursively move n-1 disks from peg 1 to peg 3
Move disk n from peg 1 to peg 2 (1 move)
Recursively move n-1 disks from peg 3 back to peg 1
Move disk n from peg 2 to peg 3 (1 move)
Recursively move n-1 disks from peg 1 to peg 3

This creates three recursive calls instead of two, and each large disk requires 2 moves instead of 1.

### 2. Observed Move Counts

| n | Expected ≈ 3ⁿ − 1 | Program Output | Matches? (Y/N) |
|:-:|:--:|:--------------:|:--------------:|
| 1 | 2 |       2        |       Y        |
| 2 | 8 |       8        |       Y        |
| 3 | 26 |       26       |       Y        |
| 4 | 80 |       80       |       Y        |

### 3. Analysis
_Why does this variation grow faster than the standard version? How do additional move constraints affect complexity?_

> ✎ Your answer here
> This variation grows faster because the recurrence relation is T(n) = 3·T(n-1) + 2 instead of T(n) = 2·T(n-1) + 1. 
> The middle-peg constraint forces us to make three recursive calls (moving the smaller disks three times) 
> plus two moves for the large disk. This results in 3ⁿ − 1 total moves compared to 2ⁿ − 1 for the standard version.
> The added constraint significantly increases complexity—for n=10, standard Hanoi needs 1,023 moves
> while this variation needs 59,048 moves. Additional constraints generally increase complexity by
> limiting available options and requiring more steps to satisfy the restrictions.

---

## Comparative Analysis

### 1. Growth Patterns

| Version | Approx. Formula | Growth Type |
|:--|:--|:--|
| Standard | 2ⁿ − 1 | Exponential |
| Variation | 3ⁿ − 1 | Exponential (faster) |
| Iterative (Optional) | 2ⁿ − 1 | Same as recursive but loop based |

### 2. Stack Depth and Memory
_Estimate the maximum recursion depth before StackOverflowError and discuss how stack size (–Xss flag) affects this._

> ✎ Your answer here
> The maximum recursion depth equals n (the number of disks) because at most n recursive calls are on the stack simultaneously.
> With Java's default stack size (~1MB), you can typically handle n=10,000-20,000 before StackOverflowError occurs.
> The -Xss flag (e.g., -Xss2m for 2MB) increases stack size and allows deeper recursion.
> However, stack depth isn't the real problem—even small values like n=30 require over a billion moves
> (2³⁰ - 1 = 1,073,741,823), making execution time the actual limiting factor, not memory.

---

## Part 4 — Beyond Recursion (Extra Credit)

### Iterative / Explicit-Stack Version (`TowersIterative.java`)

1. How does your iterative version simulate recursion?
2. How did you track pending calls or frames?
3. Which version (r vs iterative) is clearer? Why?

> ✎ Your answer here
> I did not do the extra credit

---

## Discussion &amp; Reflection

1. What three questions can you use to verify a recursive solution works?
2. How do the base case and recursive case cooperate to guarantee termination?
3. What is the trade-off between elegance and efficiency in recursion?

> ✎ Your answers here
1- Does the base case handle the simplest input correctly and stop recursion?
Does each recursive call move toward the base case (progress toward termination)?
Assuming the recursive call works for smaller inputs, does the recursive case correctly solve the current input?
> 2-The recursive case breaks the problem into smaller subproblems that move toward the base case (e.g., n → n-1).
> The base case provides the termination condition when the input becomes trivially simple (n = 0).
> Together, they form a "ladder". the recursive case steps down toward simpler problems,
> and the base case stops the descent, ensuring the recursion eventually terminates. - I knew this, but got the ladder
> idea from AI.
> 3- Recursion often produces elegant, concise code that closely matches the problem's mathematical structure 
> (For example - Towers of Hanoi in 6 lines). However, it incurs overhead from function calls, stack memory usage, and potential stack overflow for deep recursion.
> Iteration may be less elegant but more efficient as there is no call overhead and better cache locality.

---

## References (APA or MLA format)

- Dale, N., Joyce, D., &amp; Weems, C. (2016). *Object-Oriented Data Structures Using Java* (Ch. 3 Recursion). Jones &amp; Bartlett.
- Additional videos or websites you consulted (YouTube CS50 Recursion, GeeksForGeeks Hanoi, etc.)

---

 **Submission Checklist**

- [ ] `TowersOfHanoi.java` — implemented and tested
- [ ] `TowersExercise21.java` — counts moves correctly
- [ ] `TowersVariations.java` — middle-peg constraint implemented
- [ ] (Extra Credit) `TowersIterative.java` — loop or explicit stack solution
- [ ] All JUnit tests pass (green on GitHub Actions)
- [ ] This `ANSWERS.md` file completed and committed to repo  
