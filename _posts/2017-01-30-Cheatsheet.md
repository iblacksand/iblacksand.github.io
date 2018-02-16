---
layout: post
title: "Assembly Cheatsheet"

---

Every assembly action must start with an `@` which is to followed by a memory address or [label](#labels) and then followed by code to modify something. For example, `@1` references the address `1` and you can access the address with `A` or the value stored at the memory slot  with `M` and use `D` to keep track of values that you want to use later on.

```
@100
D = A // A is address which is 100
```

What this does is stores the address to `D`, and since we called the address of `100`, `D` has a value of `100`. If we want to assign a spot in memory, `i`, with a value of `2220` we can simply do this
```
@2220
D = A // first we have to assign D with the value of 2220
@i 
M = D // M is the value held at that memory point.
```

`i` is some random address in memory but for some things, like the keyboard, you will have to use a numerical value for the address. 

Address names don't have to be capitalized, but to be safe, capitalize `M`, `A` and `D`.

## Labels

Labels are spots in code that are used to tell where to jump to. [Jumps](#Jumps) are mentioned later on but here is an example
```
(startLoop) //this is the label
@startLoop
0;JMP // this is a jump that will go to the label startLoop
```

As you can see labels are formatted like `(labelName)`. They will be useful later. Note that this is something that you don't have to put an `@` before.

## Jumps

Jumps are used to go to a label as shown before. They can go back or forward in code. There are two types of jumps, unconditional and conditional.

### Unconditional Jumps

As the name says, these will jump to the label no matter what. 
They are declared with `JMP`. Here is an example
```
(toJumpTo)
@toJumpTo //we put the label instead of a memory address and this shows where to jump
0;JMP// put a 0 followed by ;JMP to jump

```

The basic format is:

```
@labelToJumpTo
0;JMP
```

These are useful to go back to the start of a loop.

### Conditional Jumps

Conditional jumps will only jump to the label if a condition is met. Instead of `JMP` you would use different letters for different conditions. The chart below shows the different types of conditions.

| Instruction(code) | Jump Condition           | Jumps when              |
|-------------|--------------------------|-------------------------|
| JEQ          | Jump if Equal            | Equals 0                |
| JNE         | Jump if Not Equal        | Doesn't Equal Zero      |
| JGT          | Jump if Greater          | Greater than 0          |
| JGE         | Jump if Greater or Equal | Greater or equal to 0   |
| JLT          | Jump if Less             | Less than 0             |
| JLE         | Jump if Less or Equal    | Less than or equal to 0 |

There are more but this is the basics that you should need.

Notice that these all compare to zero. The value we compare with is the value before the semicolon. Let's say `D` has a value of `12`. 

```
@someLabel
D; JLE
```
this won't jump as `D`, `12`, is not less than or equal to `0`. If we want to check if some address `i` is less than `100` we can do this:

```
@i
D = M //stores the value of i at D
@100
D = D - A //subtracts 100 from D
@jumpIfLess
D;JLT // Sees if D is less than 0
```

If `i` is less than `100` and we subtract `100` away, it would be negative. So we want to jump if the `D` is negative which is the same as less than `0`. We can just change `JLT` to `JGT` if we want to jump if `i` is greater than `100`.

If the condition is not met it continues down the code as normal.

You can use any of these jumps and if you want to learn more jumps you can see them [here](https://courses.engr.illinois.edu/ece390/books/labmanual/assembly.html) or [here](http://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm).

## Loops

To build loops, you will have a series of jumps and labels that will keep it looping till a condition is met. 

```
int sum = 0;
for(int i = 1; i <= 100; i++){
	sum += i;
}
return sum;
```
This adds all of the numbers from 1 to 100

The equivalent of this in assembly is as follows

```
@i
M = 1 //Sets i to 1
@sum
M = 0 //sets our sum to 0

(LOOP) // label for the start of the loop
	@i
	D = M //stores the value of i to D
	@100
	D = D - A // subtracts off 100 from D
	@END // Label of the end of the loop
	D;JGT // if i - 100 is positive we need to end the loop
	@i
	D = M //restores the value of i to D
	@sum
	M = M + D // adds the value of i to D
	@i
	M = M + 1 //increases the value of i by 1
	@LOOP
	0;JMP //jumps back to the start of the loop
(END) // Our label for the end
@END
0;JMP //Infinite loop to end the program
```

We have to store the value of `i` to `D` whenever we want to add to `sum` because we can't just do 
```
@sum
M = M + i
```
because `i` is an address not a value so you have to store it in `D`. You have to do this whenever you want to interact with two different addresses.

Note the infinite loop at the end. This means that the program is done.

```
(ENDPROGRAM)
@ENDPROGRAM
0;JMP
```

Just copy this to the last line of your code and the CPU will stop running.

## Cheatsheet

I found that editing the while loop helped a lot for Mult so here it is without the comments.

```

@i
M = 1
@sum
M = 0

(LOOP)
	@i
	D = M
	@100
	D = D - A
	@END
	D;JGT
	@i
	D = M
	@sum
	M = M + D
	@i
	M = M + 1
	@LOOP
	0;JMP
(END)
@End
0;JMP

```