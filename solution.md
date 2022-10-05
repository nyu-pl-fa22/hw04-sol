# Solution

## Answer to Problem 1

The problem with the code is that the array read `grades[entry]` on
the last line of `get_transcript_entry` does not check whether `entry`
is within the bounds of the array `0 <= entry < size`. Since the array
`entry` is stored on the stack, providing values for `entry` that are
outside of the bounds of the array enables the attacker to read off
any value from the stack. Both the argument value `strudent_id` of
`get_transcript_entry` as well as the local variable `student_id` in
`main` are potentially stored on the stack. In particular, unless the
compiler decides to store the local variable `student_id` of `main` in
a register, it will be stored in the activation record of `main`
during the time when the call to `get_transcript_entry`
executes. Thus, the attacker can read off the student ID from that
activation record by choosing a sufficiently small negative value for
`entry` (or a sufficiently large value greater than `size` depending
on the direction in which the stack grows).

If you compile program `problem1.c` with `gcc` and all optimizations
disabled, then on an x86 architecture, running Linux, the value `28`
for entry will retrieve the student ID. It will be printed as `*`,
the character with ASCII code `42`.

## Answer to Problem 2

1. The program prints the value `0` twice. This is because the
   occurrence of `x` in the body of `print_x` always refers to the
   local variable of `x` within `foo`. Due to deep binding, this is
   the reference environment for both calls to `print_x`, the one
   within `foo` after the call to `s`, and the one via the return
   value `p` of `foo` on the last line. The occurrence of `x` in
   `set_x` always refers to the global variable `x` and hence any call
   to `set_x` will only change the global `x` but not the local `x` in
   `foo`.
   
2. The program prints `1` and `2`. This is because under dynamic
   scoping and shallow binding, the occurrences of
   `x` in `set_x` and `print_x` will always refer to the most recent
   binding of `x` in the reference environment where these functions
   are called. For the two calls within `foo`, this is the reference
   environment of `foo` which binds `x` to the local variable `x`
   within `foo`. Thus, the call `s(y)` in `foo` sets the local `x` to
   `1`, which is then printed by the subsequent call to `print_x`. The
   call `set_x(2)` on the second last line on the other hand is
   executed in the global reference environment, which binds `x` to
   the global variable `x` declared on the first line. Likewise the
   subsequent call to `print_x` via `p` will use the global `x`, whose
   value is now `2`.

## Answer to Problem 3

1. The program prints `4` and `2`.

2. The program prints `4` and `4`.

3. The program prints `5` and `3`.

4. The program prints `5` and `5`.

