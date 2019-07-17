# Welcome!

This playground is my third one about the BrainFuck language. See 
* [Getting started with BrainFuck](https://tech.io/playgrounds/50426/getting-started-with-brainfuck/welcome)
* [BrainFuck part 2 - Working with arrays](https://tech.io/playgrounds/50443/brainfuck-part-2---working-with-arrays/welcome)
playgrounds if you didn't already !

The goal of this playground is to write a BF interpreter... in BF !

TL/DR : The cover image is the final source code of this interpreter. _Sound: Nelson Muntz's "Ha! Ha!"_

# Let's start

Let's create a BrainFuck interpreter in BrainFuck. The goal of this first step is to understand how we can do that, and then next ones will be about implementation.
BrainFuck execution needs
* Instructions (+ - . , < > [ ])
* Instruction pointer
* Input stream (to read chars using ,)
* Output stream (to write chars using .)
* Memory
* Memory pointer

# Instructions and input stream
For this interpreter, the _code_ to execute will be provided by the input stream. However, the _data_ read by code execution will also be provided by the same input stream.

We need to maje a distinction between _code_ and _input data_, that's why we will introduce a separator (a char that is not part of the BF's 8 instructions) that will separate code and data.

This also means the separator cannot be used in comments within the source code part.

For this playground, we will consider # as our code/data separator char

# Memory

The memory needs to be a subset of global execution memory. But let's have it as big as possible: we'll use some space at the beginning of the memory array for instructions to execute (fixed size), and some variables / swap memory for our interpreter.

Another issue with the memory is that it's an array. As we saw in previous playground, to work with arrays that can contains null values, we need to have 2-cell-long blocks.

# Instruction pointer

The core of our interpreter will be
* Read code to interpret
* While instruction pointer points at an instruction
  * execute it
  * move to the next instruction (except for [ and ] )

To handle [ and ], we need to define something more complex
* either a stack of opening "[" addresses : on "]" we just get the return pointer from the stack
* or on "]" read code backwards until we met as many "[" than "]" (including the first one)

Both are possible : the stack allows a virtually infinite number nested loops, but it implies a dedicated part of the memory. The counter is limited to 255 nested loops : that's enough for our playground :)

Hence, we will have 2 flags / registers : _inactive_ flag and _direction_ flag
* inactive indicates if code can be run or not
* direction indicates whether instructions are read from left to right or right to left
Then
* if inactive is not null
  * execution of [ : increase inactive flag by 1
  * execution of ] : decrease inactive flag by 1
  * execution of other instructions : nothing
  * if inactive is now null : reset direction flag
* if inactive is null
  * execution of [ :
    * if current memory value is null : increase inactive flag by 1
    * if current memory value is not null : do nothing
  * execution of ] :
    * if current memory value is null : do nothing
    * if current memory value is not null : 
      * set direction flag to 1 (backwards)
      * decrease inactive flag by 1
  * execution of other instructions : do it

Hence, if we met an opening loop, we either just go into the loop or stop execution instructions until we met the corresponding closing loop instructions.

And when we reach the end of loop instructions, we either exit the loop or read backwards code wihout executing it until we met the corresponding opening loop instruction.

# Global memory map and execution flow

Memory map: 0, 0, 0, instructions, 0, instruction pointer, some free space, memory pointer, some free space, 0, memory

Execution flow
* Read instructions, i.e. read only instructions, and stop when # separator is read
* Fetch instruction given by instruction pointer
* Execute instruction
  * For + and - : use memory pointer to reach current cell in memory array and update value
  * For < and >: update memory pointer (+/- 1)
  * For , : read in input stream the next char and store in memory cell pointed by memory pointer
  * For . : write current memory value to output stream
  * For [ : read current memory value
    * if 0 : increase instruction pointer until it meets as many '[' than ']'
    * if not: do nothing
  * For ] : read current memory value
    * if 0: do nothing
    * if not: decrease instruction pointer until it meets as many '[' than ']'

