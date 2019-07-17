# Fetch next instruction

Now that our instructions are read correctly, let's implement a fetch loop.

Given an instruction pointer, the fetch is an operation that gives us the operation to implement. This is globally similar to a random access in array, but the fetch loop should be executed again and again.

Then, it's simpler to have a one-based index. We can just start a loop on this value that won't be null, and loop indefinitely on next task.
Access an element in this case is not complex either, the pointer value just need to be decreased by a given offset (1).

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_
* Cursor: on (initially null) instruction pointer
* Input: any

# Process

* Set pointer to 1
* While pointer is not null
  * Decrease pointer
  * Copy pointer (random array access at given index destroys the index value)
  * Use pointer copy to get the instruction
  * Copy target instruction outside array
  * If instruction is not null
    * well, execute it - but later :)
    * update instruction pointer according to the instruction (well, again, later)
  * If instruction is null
    * Break the loop : set instruction pointer to 0
* Loop

# Code
```
+                             Set pointer to 1
[                             While pointer is not null
  -                           Decrease pointer
  [->+>+<<]>>[-<<+>>]         Copy pointer (random array access at given index destroys the index value)
  <[-<<<[<]>[-<<+>>]>[>]>>]   Use pointer copy to get the instruction; part 1 : split instructions array in 2 parts
                              Note: elements are shifted by 2 cells to improve the "extract and copy value" part
  <<<[<]>[-<+<+>>]<           Part 2 : copy target instruction outside array (we have 3 empty cells instead of 2 so copy can be handled here)
  [->>[>]>>+<<<[<]<]          Part 3 : move the copied instruction
  <[[->>+<<]<]                Put instructions array at its original location
  >>>[>]>>                    Go to copied instruction

  do something

<]                            Go back to the instruction pointer and loop

```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0 
* Cursor: on instruction pointer
* Input: unchanged
* Output: unchanged


# Test program

This program reads the data (previous part), then do the fetch part, with "do something" = reconstruct instruction (+35) and print it

```
>>>,[>+++++[-<------->]+<[>,>]>[[-]>]<<]>               load all instructions (until end of input or #)
+[-[->+>+<<]>>[-<<+>>]<[-<<<[<]>[-<<+>>]>[>]>>]<<<[     Fetch part 1
<]>[-<+<+>>]<[->>[>]>>+<<<[<]<]<[[->>+<<]<]>>>[>]>>     ** part 2 **
                                                        the "do something"
  >+<[
     >++++++[-<+++++>]<.                                not null : print instruction
     <++                                                and increase pointer (by 1 plus the offset)
     >[-]                                               reset instruction
  ]>[
    <<[-]>>-                                            null: reset instruction pointer
  ]
<<]                                                     Fetch loop
```
