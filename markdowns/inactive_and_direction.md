# Handle inactive and direction flags

All our instructions are correctly implemented. We just need one last step : handle the inactive and direction flags.
* When inactive is set, instructions are fetched but ignored, except
  * While : it increase the inactive flag, and if inactive flag becomes null then it also clears direction flag
  * Loop : it decrease the inactive flag
* When direction flag is set, instructions are fetched backwards (i.e. the instruction pointer is decreased instead of increased)

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_
* Cursor: just after instruction pointer, on instruction
* Input: any

# Process

* if inactive flag is not null
  * check if instruction is loop and decrease inactive flag
  * or check if instruction is while and increase inactive flag
  * or ignore instruction
* else : process instruction

This process replaces parts of the actual fetch mechanism.

Instead of doing "if direction is set then decrease instruction pointer otherwise increase it", it's easier to
* check if direction flag is set and decrease instruction pointer by 2
* in all cases, increase instruction pointer by 1

Eventually, it will be either +1 or -2+1=-1

# Code to replace fetch

```
[                              if instruction is not null
  >>[                          if inactive flag is not null
  +++++++[-<------->]+<[       not while
    --[>-<[-]                  not loop either : reset else flag and instruction
  ]>[->>-<<                    it's a loop : decrease inactive flag and reset else flag
  ]<]>[->+>+                   reset else flag and increase intactive flag and another else flag just before
  [<-]<                        if inactive is not null reset else flag and go after instruction pointer (null cell)
                               but if inactive is null then it just moves to the else flag (not null cell)
  [>>[-]<<-<]                  This parts only apply if inactive is null, it resets direction flag then else flag and go after instruction pointer
  >+<]<+>>-]<[-<]+<            This part is a bit tricky : it sets the instruction (whatever it was) to 1; and as 1 is not a valid code it will be ignored by our previous switch/case block
  [                            Now process the instruction as usual (if inactive is set then we already processed it and set the value to 1
    continue
```

# Code to replace instruction pointer increment

```
  >>>[-<<+<<-->>>>]      If direction flag is set then set reset it; set copy flag before inactive one; and subtract 2 to instruction pointer
  <<[->>+<<]<<<++>       Move flag copy back; then increment instruction pointer as expected
```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0, 0, _inactive flag_, _direction flag_, _memory pointer_, 0, 0, 0, 0, 0, _memory_
* Cursor: just after instruction pointer, on instruction
* Input: unchanged
* Output: unchanged
