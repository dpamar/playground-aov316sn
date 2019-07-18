# Loops

Now, let's implement loops. We already described what we will do with inactive and direction flags for while and loop instructions, it's time to implement it

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_
* Cursor: just after instruction pointer, on instruction
* Input: any

# Process

* First, access and copy current memory value (see previous step)
* Then, if it's while instruction
  * if copy is null, increase the inactive flag
  * if not null : do nothing (we will move to the first instruction within the loop and that's all)
* And if it's loop instruction
  * if copy is null : do nothing
  * if not null
    * decrease inactive flag
    * set direction flag to 1

# Code (while)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]      Access target memory value                   
+>>[-]>[-<<+>>]<<<[<<]<]>>>[>>]<       ** part 2 **
[->>+<<]>>[-<<+<[<<]<+>>>[>>]>]        Move target value; then move it back and copy after memory pointer
<<<[[->>+<<]>[-<<+>>]<<<]              Move mem array back to its original position
+<[[-]>-<]>[-                          Set else bit; if value is not null clear else flag and do nothing; otherwise                              
   <<<<+>>>>                           Increase inactive flag by one
]<<<<<<                                Go back to the instruction value cell
```

# Code (loop)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]      Access target memory value                   
+>>[-]>[-<<+>>]<<<[<<]<]>>>[>>]<       ** part 2 **
[->>+<<]>>[-<<+<[<<]<+>>>[>>]>]        Move target value; then move it back and copy after memory pointer
<<<[[->>+<<]>[-<<+>>]<<<]              Move mem array back to its original position
<[[-]                                  If value is not null (reset value)
   <<<->[-]+>>                         Decrease inactive flag by one and set direction flag to 1
]<<<<<                                 Go back to the instruction value cell
```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_
* Cursor: after instruction pointer 
* Input: unchanged
* Output: unchanged
