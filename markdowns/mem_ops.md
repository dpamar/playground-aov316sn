# Memory manipulation operations

Now, let's implement inc, dec, print and read operations.

The method is quite the same for these 4 operations : reach target cell and do something.

To reach the target Nth memory cell, we will move the first Nth memory cells to the left, then reach the "last" one. Remember that memory cells are encoded using 2 values (to allow null inputs)

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_
* Cursor: just after instruction pointer, on instruction
* Input: any

# Process

* First, copy memory pointer to have the target index, and increment target index (one-based is easier to move Nth first blocks)
* _invariant: memory = 0, 0, 0, instructions, 0, instruction pointer, 0, 0, inactive flag, direction flag, memory pointer, target index, 0, 0, memoryA, 0, 0, memoryB with memory = memoryA + memoryB and length(memoryA) = memory pointer - target index + 1_
* While targetIndex is not null
  * Go through memoryA, reach memoryB
  * Move memoryB's first value to the left (after memoryA's last position)
  * Decrease target index
* Loop
* _memory = 0, 0, 0, instructions, 0, instruction pointer, 0, 0, inactive flag, direction flag, memory pointer, 0, 0, 0, memoryA, target cell flag, target cell value, 0, 0, memoryB_
* Reach target cell value
* Do the job (inc, dec, print, read)
* Move memoryA back to its originial position

# Code (generic)

```
>>>>[->+>+<<]>>[-<<+>>]<+            First copy memory pointer to have the target index and increment target index
[                                    While targetIndex is not null
  >>>[>>]+>>                         Go through memoryA and reach memoryB (note : the target cell flag is set on the fly)
  [-]>[-<<+>>]                       Reset target cell flag from the source, and move value from source to destination
  <<<[<<]<<-                         Decrease target index
]
>>>[>>]<                             Reach target cell value
Do the job
<[[->>+<<]>[->>+<<]<<<]              Move memoryA back to its originial position (2 cell long blocks)
<<<<<<                               Go back to instruction value cell
```

# Minified version (add)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]+   Reach target
>>[-]>[-<<+>>]<<<[<<]<<-]>>>[>>]<
  +                                  Do the job (add)
<[[->>+<<]>[->>+<<]<<<]<<<<<<        cleansing
```

# Minified version (sub)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]+   Reach target
>>[-]>[-<<+>>]<<<[<<]<<-]>>>[>>]<
  -                                  Do the job (sub)
<[[->>+<<]>[->>+<<]<<<]<<<<<<        cleansing
```

# Minified version (read)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]+   Reach target
>>[-]>[-<<+>>]<<<[<<]<<-]>>>[>>]<
  ,                                  Do the job (read)
<[[->>+<<]>[->>+<<]<<<]<<<<<<        cleansing
```


# Minified version (print)

```
>>>>[->+>+<<]>>[-<<+>>]<+[>>>[>>]+   Reach target
>>[-]>[-<<+>>]<<<[<<]<<-]>>>[>>]<
  .                                  Do the job (print)
<[[->>+<<]>[->>+<<]<<<]<<<<<<        cleansing
```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_* Memory:  
* Cursor: after instruction pointer 
* Input: unchanged, unless instruction was read
* Output: unchanged, unless instruction was print
