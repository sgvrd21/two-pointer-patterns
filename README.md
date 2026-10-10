# two pointers interview questions: how to spot the pattern fast, drop O(n²) to O(n), and answer the follow-ups out loud

You write a nested loop. It works. Then the interviewer asks, "Can you do better?" and suddenly the room gets smaller.

Two pointers is the answer to that question more often than any other single technique. It shows up in array problems, string problems, linked lists, and half the "medium" problems that get asked at Google, Meta, Amazon and friends. The reason isn't that it's clever. It's that it tests something interviewers actually care about: whether you notice what the input gives you for free.

## What the interviewer is really measuring

There are four things a two-pointer question checks, and they're rarely stated out loud.

Can you reduce O(n²) to O(n)? Every pointer moves in one direction and never goes back, so total pointer movement is capped at roughly 2n. That's the whole trick.

Do you read the constraints? "Sorted array" is not decoration. If the array is sorted, you get a guarantee you can exploit. If it isn't sorted and you can't sort it, two pointers usually stops being the right answer.

Can you work in place? Many two-pointer problems are really about O(1) extra space. Slow/fast pointer patterns exist precisely because a hash set would use O(n) memory.

Can you justify pointer movement? The follow-up is never "what's the complexity." It's "why is it safe to move that pointer?" If you can't explain the exchange argument, the correct code doesn't save you.

## The three shapes you need to recognize

Almost every question in this family is one of three variants.

**Converging pointers.** One pointer at index 0, one at the last index. They move toward each other. Used when the array is sorted and you're hunting pairs, or when you need to compare from both ends. Two Sum II, Container With Most Water, Valid Palindrome, Trapping Rain Water.

**Same-direction pointers (slow/fast).** Both start at index 0, but the fast pointer scans everything while the slow one only advances when a condition holds. This is the shape for in-place modification: Remove Duplicates, Move Zeroes, partitioning. Sliding window is the same family, just with a maintained window invariant.

**Fast/slow on linked lists.** One pointer advances one node, the other two. Used for cycle detection and finding the middle. You can't index into a linked list, so this is the only way to get the "two pointers at different speeds" idea without random access.

| Shape | Setup | Typical use | Time / space |
| --- | --- | --- | --- |
| Converging | left = 0, right = n-1 | Pair sums on sorted input, palindromes, container problems | O(n) / O(1) |
| Slow + fast (same direction) | both at 0, slow only advances on a condition | In-place dedup, partitioning, move-zeroes | O(n) / O(1) |
| Fast + slow (linked list) | 1x and 2x steps | Cycle detection, middle node | O(n) / O(1) |

## The questions that actually get asked

This is the list worth grinding. The LeetCode numbers are included because that's how people search for them, and the "what they're checking" column is the part that decides your score.

| Question | Shape | Time / space | What the interviewer is actually testing |
| --- | --- | --- | --- |
| Two Sum II – Input Array Is Sorted (LC 167) | Converging | O(n) / O(1) | Whether you use the sorted guarantee instead of reaching for a hash map |
| 3Sum (LC 15) | Sort, then converging inside a loop | O(n²) / O(1) extra | Duplicate skipping, and why sorting is mandatory |
| 3Sum Closest (LC 16) | Same as 3Sum | O(n²) / O(1) | Tracking closest-so-far instead of an exact match |
| 4Sum (LC 18) | Nested loop + converging | O(n³) / O(1) | Generalizing k-sum without breaking dedup |
| Container With Most Water (LC 11) | Converging | O(n) / O(1) | The exchange argument for moving the shorter line |
| Trapping Rain Water (LC 42) | Converging with left_max / right_max | O(n) / O(1) | Understanding why the smaller side is safe to decide |
| Remove Duplicates from Sorted Array (LC 26) | Slow/fast | O(n) / O(1) | In-place writes and returning the new length |
| Move Zeroes (LC 283) | Slow/fast | O(n) / O(1) | Stable partition: relative order must survive |
| Sort Colors (LC 75) | Three pointers (Dutch flag) | O(n) / O(1) | Boundary maintenance, which is where people break it |
| Merge Sorted Array (LC 88) | Two pointers, back to front | O(n+m) / O(1) | Realizing a forward merge overwrites the buffer |
| Squares of a Sorted Array (LC 977) | Converging | O(n) / O(1) | Noticing the largest square sits at one of the ends |
| Valid Palindrome (LC 125) | Converging inward | O(n) / O(1) | Character-skipping rules and termination |
| Valid Palindrome II (LC 680) | Converging + one retry branch | O(n) / O(1) | Handling the single-deletion case without duplicating logic badly |
| Linked List Cycle (LC 141) | Fast/slow | O(n) / O(1) | Floyd's algorithm and *why* the pointers must meet |
| Middle of the Linked List (LC 876) | Fast/slow | O(n) / O(1) | Odd and even length edge cases |
| Minimum Size Subarray Sum (LC 209) | Same-direction window | O(n) / O(1) | Knowing that sliding window is a two-pointer shape |

If you can solve all sixteen and explain the reasoning behind each, you've covered the bulk of what gets asked. Candidates who only memorize the solutions tend to stall on the follow-up.

## Say the reasoning before you type

One thing that separates strong candidates: they narrate the invariants before writing a loop.

Instead of "I'll use two pointers," say the version that carries information:

> "The array is sorted, so I can start one pointer at each end and move them inward based on whether the current sum is too high or too low. That avoids checking every pair, so it's O(n) time and O(1) space."

For the slow/fast shape:

> "I'll keep a slow pointer as the write position and a fast pointer as the scanner. Every time the fast pointer finds a value that should survive, I write it at slow and advance slow by one."

For Container With Most Water, the sentence that wins the follow-up:

> "The area is capped by the shorter line, so moving the taller one inward can never increase the area. Only moving the shorter one can, which is why that's the safe greedy choice."

Candidate walkthroughs of Two Sum II posted on LeetCode's discussion forum follow this exact skeleton: sortedness, then pointer rule, then complexity, then the unsorted variant. That's not coincidence. That's the script interviewers are listening for.

## The follow-ups that decide the outcome

Most of these questions come with a second act. Prepare for it.

**"What if the array isn't sorted?"** For pair-sum, the honest answer is a hash map: O(n) time, O(n) space. You can also sort first, which costs O(n log n) and destroys the original indices, so mention that trade-off explicitly instead of skating past it.

**"Can you return all unique pairs?"** Now duplicate handling is the problem. After finding a pair in 3Sum, advance both pointers past equal values. Interviewers watch for this.

**"Can you do it in O(1) space?"** This is a hint toward slow/fast. If your current approach copies the array, you've missed it.

**"What if the input arrives as a stream?"** Converging pointers die here, since you need both ends. Say so, and pivot to a different structure.

**"What about at most two occurrences of each value?"** The dedup variant of Remove Duplicates. The slow pointer stops being "next unique position" and becomes "next valid write position."

## When two pointers is the wrong answer

Being able to say no is as valuable as saying yes.

- Unsorted data you're not allowed to reorder. Sorting changes indices, which some problems depend on.
- No random access. Python lists and arrays are fine. Linked lists are not, unless you're doing the fast/slow special case.
- Problems that need counts, frequencies, or "have I seen this before." That's hashing.
- Interval problems where the ordering of the result matters.

If you propose two pointers on unsorted input and can't explain the sorting step, you've handed the interviewer an easy negative.

## Where the practice actually happens

The pattern is easy to understand and hard to execute under time pressure. Ten minutes of reading gets you the idea; recognizing it in a problem you've never seen takes repetition. That gap is what structured platforms like AlgoMonster are built around — it teaches two pointers as one of 48 explicit patterns, each with an explanation, a code template, and a set of problems attached to it, so you're drilling a pattern rather than accumulating solutions.

A few details worth knowing before you pay for anything:

- The content is text and diagrams. Around 700+ illustrations, no video walkthroughs at all. If you learn from watching someone code, this format will feel dry.
- The problem count is roughly 230, well below LeetCode's thousands. The bet is that fewer mapped problems beat more unmapped ones.
- There's a decision flowchart that routes a new problem to a likely pattern, with branches like "two pointers / partitioning" and a check for O(1) memory. That's exactly the skill you're trying to build.
- Speedrun mode gives you timed passes over problems. The pool is smaller than LeetCode's contest set, so treat it as pressure practice, not ranking practice.
- Solutions are available in Python, Java, JavaScript, C++, Go and Haskell.
- Created by ex-Google engineers, which reviewers consistently repeat as a trust signal. Take that as positioning, not proof.

👉 [open the AlgoMonster plan page and try the free tier first](https://algo.monster?fpr=li69)

Reviewers who compare it against LeetCode tend to land in the same place: AlgoMonster for the framework, LeetCode for volume. The two-pointer section teaches you the movement rule. LeetCode gives you 200 variations to prove you didn't just memorize one.

## Plans and current pricing

There's no monthly option. It's free, annual, or lifetime.

| Plan | What you get | Price | Billing | Get it |
| --- | --- | --- | --- | --- |
| Free | Limited access to sample patterns and problems. Enough to judge whether the text-and-diagram format works for you | $0 | — | [start with the free tier](https://algo.monster?fpr=li69) |
| Pro – Yearly | All patterns, lessons and problems; unlimited AI assistant; unlimited Speedrun; company questions updated monthly; 500+ lessons across 6 courses; 1,000+ illustrations; 3,000+ editorials; member discount on live bootcamps | Promotional pricing has shown **$69.99–$99/year** (~$5.84–$8.25/mo) | Billed annually, cancel or upgrade anytime | [see the current annual price](https://algo.monster?fpr=li69) |
| Pro – Lifetime | Everything in the annual plan, plus lifetime updates and permanent access | Promotional pricing has shown **$89–$189** one-time | One-time payment | [check the lifetime price](https://algo.monster?fpr=li69) |

Two honest notes.

First, the price moves. The plan page has displayed different promotional figures at different times, and third-party coupon blogs quote numbers as high as $459 for lifetime, which does not match what the page itself shows. If a number matters to your decision, read it on the page at checkout, not in a review from another month.

Second, not everything is bundled. Courses built with external partners, like Quant Interview Prep, are sold separately. The free tier is a preview, not a viable interview-prep plan on its own.

## Annual or lifetime?

Do the arithmetic against your own timeline.

If your interview loop is in six to eight weeks and you land the job, yearly access covers it. You'd be paying somewhere under $100 for the window that matters.

Lifetime makes sense if you expect to come back. Engineers who change jobs every two to three years re-prep each time, and the two-pointer refresh is part of that. If you think you'll use it twice, the lifetime plan stops being a splurge. If you're certain this is a one-shot and you'll never touch the material again, it's money sitting idle.

The free tier is the tiebreaker. Use it to check whether the diagram-heavy format clicks for you before you decide anything.

## A two-week two-pointer drill

If you want something concrete to run:

1. **Days 1–2:** converging pointers on sorted arrays. Two Sum II, Valid Palindrome, Squares of a Sorted Array. Force yourself to state the movement rule out loud before coding.
2. **Days 3–4:** slow/fast in-place work. Remove Duplicates, Move Zeroes, then the Dutch flag. Track your off-by-one errors.
3. **Days 5–6:** 3Sum, 4Sum, 3Sum Closest. The entire difficulty here is duplicate handling. Write the skip logic twice from scratch.
4. **Days 7–8:** Container With Most Water and Trapping Rain Water. Write out the exchange argument in two sentences before you write code.
5. **Days 9–10:** linked lists. Cycle detection and middle node. Be ready to explain why the fast pointer can't skip past the slow one without meeting.
6. **Days 11–12:** sliding window as a two-pointer family. Minimum Size Subarray Sum, then a variable-window string problem.
7. **Days 13–14:** timed passes. Mix everything. If you solve a problem and can't explain the invariant in one sentence, it counts as unsolved.

That schedule is roughly 1 to 2 hours a day, which is the same window most reviewers say the full AlgoMonster curriculum takes to complete.

## Common questions

**Is two pointers the same as sliding window?**
Sliding window is a two-pointer shape where both pointers move in the same direction and the region between them is the answer. It's a specialized case with a maintained invariant, not a separate technique.

**Does it work on unsorted arrays?**
Rarely for pair problems. If you're allowed to sort, you can sort first and pay O(n log n), but you lose the original indices. If the problem needs indices, use a hash map instead.

**Two pointers or hashing for pair sum?**
Two pointers on sorted input: O(n) time, O(1) space. Hashing on unsorted input: O(n) time, O(n) space. Interviewers like hearing you name the trade-off rather than picking one silently.

**How often does this actually come up?**
AlgoMonster publishes pattern-frequency charts by company, and reviewers reporting those numbers put two pointers near the top of the list, in the region of 12% of asked patterns, roughly level with DFS and dynamic programming. Treat the exact percentage as directional, but the ranking holds up across interview reports.

**Do I need to memorize the code?**
Memorize the templates. The movement rules for converging and slow/fast cover most variants. What you can't memorize is the follow-up, which is why practicing the explanation matters as much as the implementation.

**Do I need the paid plan to learn two pointers?**
No. The concept is free in a dozen places, and 3Sum solution walkthroughs are everywhere. What a structured course buys you is the ordering and the problem set attached to each pattern, which saves the hours you'd spend deciding what to study next. 👉 [compare the plans and decide for yourself](https://algo.monster?fpr=li69)

The pattern itself is small: two indices, one direction of movement each, one invariant you can state in a sentence. The interview difficulty lives in spotting it fast and explaining why it's legal. Practice the explanation as deliberately as you practice the code, and the "can you do better?" question stops being a threat.
