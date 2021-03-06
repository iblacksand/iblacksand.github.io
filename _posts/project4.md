+++
date = "2017-02-01T08:52:46-07:00"
draft = false
title = "Project 4"

+++

Project 4 has two files, `Mult.asm` and `Fill.asm`. 

## Mult
`Mult.asm` is multiplication. You have to do `R0 * R1` and put that in `R2`. It is important to note to get the value at `R0` you would do
```
@0
D = M //stores R0 at D
```

The memory address is not `R0` but `0`.  Remember that you have to put an infinite loop at the end to say that the program is finished. It shouldn't be too difficult.

## Fill
Fill is supposed to fill the screen with black when a key is pressed.  A block of 16 pixels is consider a **word**. `@SCREEN` is the position of the first word of the screen. If a pixel holds a value of `1` this will make the pixel black. Otherwise it is white.If you assign a word to have a value of `-1`, which is `1111111111111111` in binary, it will turn the first 16 pixels black.

```
@SCREEN
M = -1 // this is referencing the first word
(END)
@END
0;JMP
```

This will make the first 16 pixels black and is a good test script to make sure you that you have the write steps to run your program.

The screen has 32 words in each row and has a total of 256 rows meaning that there are 8192 words in total. The easiest way to go at this is assigning each of these words with a value of `-1`. Start with `@SCREEN`  

To tell if you need to fill the screen, you have to tell if key is pressed. This is as simple as getting the value at the keyboard address which is `@24576`. If the value doesn't equal zero at that memory position then a key is currently pressed.

```
@24576 // memory address of the keyboard.
D = M
```
D has the value of the key being pressed. It is 0 if no key is pressed.

*All of this is given in the book. I wanted to make it easier for people to get what it is saying.*

## Testing and Compiling

To compile your `*.asm` file, run `assembler.bat` or `.sh` and then click load source file on the very left. If you press the blue arrows it will compile it slowly so to get it done quickly press the while paper with numbers on it(directly left of the equals sign). The save icon next to the open file should turn blue and you can click on it and save the `.hack` file.

Next run `CPUEmulator.bat` or `.sh`. For `Mult` you can click on the script button, the same as in HardwareSimulator, and run the test file and you got your result. For `Fill` you will have to press the open file on the very left and open `Fill.hack` in the `/Fill` folder. Next on the right, set animations to no animations. Then set view to screen. Click on the keyboard button on the bottom and then you can press any key, other than ctrl and some modifier buttons, and see the results. An error of `At line 32XXX: Can't continue past last line` usually means that you forgot  to end the program with an infinite loop.

*It is important to note that sometimes the assembler might bug out with overwriting so you might want to delete the `.hack` file and then save*