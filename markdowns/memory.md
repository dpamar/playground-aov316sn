# Memory, left and right

Let's design our memory area now, and see how we can implement the 2 first insructions over 8, left and right.

Our new memory map can be : 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, _some free space_ 0, _memory_ 

Note that we need 2 cells after instruction pointer to
* copy the instruction pointer (2 cells)
* copy the current instruction code (just one cell, after instruction)
* switch on instruction (2 cells : one for instruction, one for else flag)

Inactive and direction flags have been quickly introduced at the very first step of this playground, though they have not benn implemented yet.

Memory access will be performed like instructions : with a pointer on an array. This will introduce the same "255 max" limitation, but this will be addressed later.


Now, let's implement < and > instructions

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, _some free space_ 0, _memory_
* Cursor: on current instruction, stored after instruction pointer (note: instruction is either > or <)
* Input: any

# Process

* Increase (or decrease) memory pointer 

# Code
```
>>>>+<<<<
```

or

```
>>>>-<<<<
```

# Final state

* Memory: unchanged but mem pointer value 
* Cursor: unchanged
* Input: unchanged
* Output: unchanged
