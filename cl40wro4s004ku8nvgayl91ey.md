## BrainFuck - An Introduction

Ever since I first started programming back in middle school, I was intrigued by the infamous, ingenious, and infuriating **BrainFuck**. And no, I don't mean the regular brainfuck you experience when you're trying to figure out why your code doesn't work.

For the unfamiliar, BrainFuck is an esoteric language developed in 1993 by Urban Dominik Müller, consisting of only 8 characters. As ludicrous as it sounds, these 8 characters can be used to write any program you can think of, making BrainFuck Turing-complete.

# BrainFuck's Syntax

Since we only have 8 characters, there isn't a lot to cover.

| Character | Meaning |
| --- | --- |
| `<` | move the pointer to the left |
| `>` | move the pointer to the right |
| `+` | increment the current byte |
| `-` | decrement the current bye |
| `.` | print the current byte |
| `,` | read the byte to the current location |
| `[` | The start of a loop, exits if the byte is 0 |
| `]` | The end of a loop, repeats if the byte is non-zero |

In BrainFuck, you begin with a one-dimensional array of 30,000 bytes, all initialised to zero. A pointer is initialised to the left-most (first) byte in the array.


![Screen Shot 2022-06-04 at 4.13.17 pm.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654339477958/jw7q6_m6R.png align="left")

A sequence of the characters detailed above represents a BrainFuck program, instructing the movement of the data pointer and the modification of the bytes. There are also two streams of bytes for input (connected to the keyboard) and output (connected to the monitor) that use the ASCII character encoding.

# Hello, World!

To get a taste of what BrainFuck programs look like, here's a simple script that prints the all-too-familiar "Hello, World!":

```
>+++++++++[<++++++++>-]<.>+++++++[<++++>-]<+.+++++++..+++.[-]>+++++++++++[<++++>-]<.>++++[<--->-]<.>+++++++++++[<+++++>-]<.>++++++++[<+++>-]<.+++.------.--------.[-]>++++++++[<++++>-]<+.
```

That's a bit intimidating, so let's break it down a bit. We know that each `.` outputs the byte, so that seems to be a good place to split the code.

## Deciphering "Hello"

`>+++++++++[<++++++++>-]<.` Is a long string of characters to represent the letter 'H'. Well, actually, it represents the number **72**, which is the ASCII value of 'H'. The output stream converts this number into the character H when it displays it to the user.

Let's break this down further:
- `>` moves the pointer to the second byte (the first byte is still 0)
- `+++++++++` increments to make the second (and current) byte 9
- The `[` in `[<++++++++>-]` is the start of a loop, since the current byte is 9, we enter the loop:
    - `<` moves the pointer back to the first byte
    - `++++++++` increments to make the first (and current) byte 8
    - `>` moves the pointer to the second byte
    - `-` decrements to make the second (and current) byte 8
    - `]` end of the loop, since the current byte is 8, we keep going.
    This loop runs 8 more times, ending when the second byte is 0 and the first byte is 72 (8*9).
- `<` moves the pointer back to the first byte
- `.` prints the current byte i.e. 'H'

Similarly, `>+++++++[<++++>-]<+.` prints 'e':
- `>` moves the pointer to the second byte (the first byte is still 72)
- `+++++++` increments to make the second (and current) byte 7
- `[<++++>-]` is a loop similar to the above, that increments the first byte by 4 until the second byte becomes 0. At the end of this loop, the pointer is at the second byte and the value of the first byte is 100 (72 + 4*7).
- `<` moves the pointer back to the first byte
- `+` increments to make the first (and current) byte 101
- `.` prints the current byte i.e. 'e'

Next, `+++++++..` increments the current (first) byte by 7 to make it 108 and then prints it twice to give us `ll`.

`+++.` increments the current (first) bite by 3 to make it 111 and then prints it as 'o'.

At this point, we've achieved the "Hello" in "Hello, World!" (yay!). Take a moment to pat yourself on the back before we continue. 

## The Rest of the World

`[-]>+++++++++++[<++++>-]<.` prints a comma (','):
- `[-]` decrements the current (first) byte until it becomes 0
- `>` moves the pointer to the second byte
- `+++++++++++` increments to make the second (and current) byte 11
- `[<++++>-]` is a loop to increment the first byte by 4 until the second byte becomes 0. At the end of the loop, the pointer is at the second byte and the value of the first byte is 44 (4*11).
- `<` moves the pointer back to the first byte
- `.` prints the current byte i.e. ','

Likewise, `>++++[<--->-]<.` prints a space(' ') using a loop to decrement the first byte a total of 12 times to make it 32 (44 - 4*3), the ASCII value of ' '.

`>+++++++++++[<+++++>-]<.` increments the value of the second byte to 11, then uses that to loop until the first byte's value becomes 87 (32 + 5*11), to print 'W'.

`>++++++++[<+++>-]<.` increments the value of the second byte to 8, using it as a loop to make the first byte 111 (87 + 3*8). We know that's printed as 'o'.

In the same manner, `+++.`, `------.` and `--------.` print 'r' (111 + 3 = 114), 'l' (114 -  6 = 108) and 'd' (108 - 8 = 100), respectively.

A `[-]>++++++++[<++++>-]<+.` at the end makes the first byte 0 and the second byte 8, then loops until the first byte is 33 (4*8 + 1) to print the final '!'.

# Surviving BrainFuck

To survive BrainFuck, we can add some comments so we know WTF is going on (BrainFuck compilers will ignore them).

## Writing Comments

```

At this point:
i           0   1   2   3   4   and so on 
value       0   0   0   0   0   and so on
pointer     ^

>+++++++++          Move to i1 | add 9 to make it 9
[
    <++++++++       Move to i0 | add 8 to make it 8
    >-              Move to i1 | subtract 1 to make it 8
]                   Repeat 8 more times until i0 and i1 are 72 and 0 respectively
<.                  Move to i0 | print as 'H'

At this point:
i           0   1   2   3   4   and so on
value       72  0   0   0   0   and so on
pointer     ^

>+++++++            Move to i1 | add 7 to make it 7
[
    <++++           Move to i0 | add 4 to make it 76
    >-              Move to i1 | subtract 1 to make it 6
]                   Repeat 6 more times until i0 and i1 are 100 and 0 respectively
<+.                 Move to i0 | add 1 to make it 101 | print as 'e'

At this point:
i           0   1   2   3   4   and so on
value       101 0   0   0   0   and so on
pointer     ^

+++++++..           Add 7 to make it 108 | print as 'l' twice

At this point:
i           0   1   2   3   4   and so on
value       108 0   0   0   0   and so on
pointer     ^

+++.                Add 3 to make it 111 | print as 'o'

At this point:
i           0   1   2   3   4   and so on
value       111 0   0   0   0   and so on
pointer     ^


[-]                 Subtract until the value becomes 0
>+++++++++++        Move to i1 | add 11 to make it 11
[
    <++++           Move to i0 | add 4 to make it 4
    >-              Move to i1 | subtract 1 to make it 10
]                   Repeat 10 more times until i0 and i1 are 44 and 0 respectively
<.                  Move to i0 | print as a comma

At this point:
i           0   1   2   3   4   and so on
value       44  0   0   0   0   and so on
pointer     ^

>++++               Move to i1 | add 4 to make it 4
[
    <---            Move to i0 | subtract 3 to make it 41
    >-              Move to i1 | subtract 1 to make it 3
]                   Repeat 3 more times until i0 and i1 are 32 and 0 respectively
<.                  Move to i0 | print as ' '

At this point:
i           0   1   2   3   4   and so on
value       32   0   0   0   0  and so on
pointer     ^

>+++++++++++        Move to i1 | add 11 to make it 11
[
    <+++++          Move to i0 | add 5 to make it 37
    >-              Move to i1 | subtract 1 to make it 10
]                   Repeat 10 more times until i0 and i1 are 87 and 0 respectively
<.                  Move to i0 | print as 'W'

At this point:
i           0   1   2   3   4   and so on
value       87  0   0   0   0   and so on
pointer     ^

>++++++++           Move to i1 | add 8 to make it 8
[
    <+++            Move to i0 | add 3 to make it 90
    >-              Move to i1 | subtract 1 to make it 7
]                   Repeat 7 more times until i0 and i1 are 111 and 0 respectively
<.                  Move to i0 | print as 'o'

At this point:
i           0   1   2   3   4   and so on
value       111 0   0   0   0   and so on
pointer     ^

+++.                Add 3 to make it 114 | print as 'r'

At this point:
i           0   1   2   3   4   and so on
value       114 0   0   0   0   and so on
pointer     ^

------.             Subtract 6 to make it 108 | print as 'l'

At this point:
i           0   1   2   3   4   and so on
value       108 0   0   0   0   and so on
pointer     ^

--------.           Subtract 8 to make it 100 | print as 'd'

At this point:
i           0   1   2   3   4   and so on
value       100 0   0   0   0   and so on
pointer     ^

[-]                 Subtract until the value becomes 0
>++++++++           Move to i1 | add 8 to make it 8
[
    <++++           Move to i0 | add 4 to make it 4
    >-              Move to i1 | subtract 1 to make it 7
]                   Repeat 7 more times until i0 and i1 are 32 and 0 respectively
<+.                 Move to i0 | add 1 to make it 33 | print as '!'

At this point:
i           0   1   2   3   4   and so on
value       33  0   0   0   0   and so on
pointer     ^

```

## Delving Deeper

The above script (without comments) is **186** characters long. [The shortest "Hello, World!" code](https://codegolf.stackexchange.com/questions/55422/hello-world/163590#163590) at the time of writing this article comprises **72** characters.

If you're looking for an online compiler to run these programs, and others you may write, you can [Try it Online](https://tio.run/#brainfuck)!




