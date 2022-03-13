Two pointers are often used to solve problems involving some iterable data structures, such as arrays. Two (or more) pointers are used to traverse through the structure. It doesnt have to be two logical pointers, one of the pointers can be calculated based on the other pointer.

A two pointer algorithm generally has:
1. Two moving pointers
2. A function that uses the entries the two pointers are pointing at to solve the question
3. An easy way of deciding which pointer to move
4. Optional: A way to process the array when the pointers are moved

#### Same directions
The two pointers move in the same directions, one moving faster than the other (also referred to as fast/slow pointers).

[Remove Duplicates](Remove_Duplicates.md)

#### Opposite Directions
The pointers are moving in the opposite directions

[Two Sum Sorted](Two_Sum_Sorted.md)

#### Sliding Window
Similar to same direction problems but the function executes on the entire interval between two pointers instead of just the two entries the pointers are pointing at.

[Longest Substring without Repeating Characters](tp/Longest_Substring_wo_Repeats)
[Longest Substring with at most K Distinct Characters](tp/Longest_Substring_w_K_Distinct)

