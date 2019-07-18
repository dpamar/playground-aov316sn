# Improve our interpreter

So, we have a working interpreter, coded using 1137 instructions, and that performs about 80.000 operations to execute a single instruction. Let's do better !

Moreover, our code size is limited to 255 instructions, so is our memory accesses. We also need to fix this !

Fortunately, these problems all have the same solution.
* The most expensive part while interpreting BF using our tool consists in all the operations done to access a value in memory (shift all the array, then shift it back). 4N shifts in total to get Nth item
* The second most expensive part is exactly the same, while accessing instructions (shift all the array, then shift it back)

The trick is that we don't really need *random* accesses on these 2 arrays. We need to access *current* value, and sometimes move along array to change what *current* means.

Instruction pointer and memory pointer are useful to get any value from those 2 arrays, but we can instead
* replace a single array by 2 _beforeArray_ and _afterArray_
* say that the *current* item is the first of the _afterArray_

We actually never access an element by its index. We have the current element (in memory, or current instruction) and we just move along memory or instruction arrays, 1 cell after the other

Pros:
* Memory accesses needed for inc, dec, print, read, while and loop are now straightforward : just go through the first part of the array and get the first item of the second array
* Instructions accesses are also simplified the same way
* No need to move 2N cells to get the Nth
* No index, so no index limit

Cons:
* Instructions left and right, and moving along instruction arrays, is now a bit longer than just incrementing the index

Anyway, even if left and right were faster in the initial implementation, they were useless as long as memory is not accessed, and memory accesses are definitely faster so this drawback is highly mitigated.

One last improvement : for inc/dec/read/print, we work directly in memory; but for while and loop we copied the value outside memory. It was easier to implement instructions then, but it's more efficient to handle this value where it is, instead of copying it.

# Code (minified : only 695 instructions)

```
>>>,[>+++++[-<------->]+<[>,>]>[->]<<]>+[-<<[<]>[-<+<+>>]<[->+<]<[->>[
>]>+<<[<]<]>>[>]+>>[<<++++++[->--------<]+>[--[<->[-]]<[->>-<<]>]<[->>
+<<]>+>[<->[->>+<<]]<[->>-<<]+>]>>[-<<+>>]<<<[--------[-[-[-[<+[->----
---<]+>[--[<+++[->-------<]+>-[--[<->[-]]<[->>>>>>[>>]>>>[<<<<<[<<]<+<
->>>>[>>]>]<[<<]<<[<<]>[-<<<+>>>]<<<<<]>]<[->>>>>+>[>>]>>>[<<<<<[<<]>-
>[>>]>]<[<<]<<[<<]>[-<<<+>>>]<<<<<]>]<[->>>>>>[>>]+>>[-]>[-<<+>>]>[-]+
<<<<[<<]<<<<]>]<[->>>>>>[>>]+<[->>+<<]<-<<[<<]<<<<]>]<[->>>>>>[>>]>>>.
<<<<<[<<]<<<<]>]<[->>>>>>[>>]>>>-<<<<<[<<]<<<<]>]<[->>>>>>[>>]>>>,<<<<
<[<<]<<<<]>]<[->>>>>>[>>]>>>+<<<<<[<<]<<<<]>[-]<]>>>>+<[<<<<[<]<<[->>+
<<]>>[>]>>>>-]>[-<<<<<[<]>[-<<+>>]> [>]>>>>>]<<<<<<[<]>[[>]>+<]>]
```

Yes, you can have fun if you want and recreate the PNG cover with this code and 2 big letters "B" and "F" inside. Actually, this may be the most complex part :)

# Code (commented)

```
separator 35/0
inc 43/8
read 44/9
dec 45/10
print 46/11
left 60/25
right 62/27
while 91/56
loop 93/58

0 {instructions_before} 0 0 {instructions_after} 0 1/instruction_copy inactive_counter direction_flag 0 0 {memory_before} 0 0 {memory_after}

>>>,[>+++++[-<------->]+<[>,>]>[->]<<]>+
[-
  fetch
  <<[<]>[-<+<+>>]<[->+<]<[->>[>]>+<<[<]<]>>[>]
  inactive check processing
  +>>[
    check instruction
    <<++++++[->--------<]+>
    [
      --
      [
        not while nor loop
        reset else bit
        <->[-]
      ]<[-
        instruction loop
        decrease inactive_counter
        >>-<<
      ]>
    ]<[-
      instruction while
      increase inactive_counter
      >>+<<
    ]>
    reset instruction (but not null)
    +
    >[
      if counter is not null
      copy inactive_counter (to break loop)
      <->[->>+<<]
    ]<[
      else reset direction_flag
    ->>-<<
    ]+>
  ]
  reload inactive_counter
  >>[-<<+>>]<<
  <[
    Execute instructions
    --------
    [
      -
      [
        -
        [
          -
          [
            <+[->-------<]+>
            [
              --
              [
                <+++[->-------<]+>-
                [
                  --
                  [
                    not an instruction
                    <->[-]
                  ]<[-
                    instruction loop
                    >>>>>>[>>]>>>[<<<<<[<<]<+<->>>>[>>]>]<[<<]<<[<<]>[-<<<+>>>]<<<<<
                  ]>
                ]<[-
                  instruction while
                  >>>>>+>[>>]>>>[<<<<<[<<]>->[>>]>]<[<<]<<[<<]>[-<<<+>>>]<<<<<
                ]>
              ]<[-
                instruction right (a bit longer now : move the first cell of 2nd array to then end of 1st one)
                >>>>>>[>>]+>>[-]>[-<<+>>]>[-]+<<<<[<<]<<<<
              ]>
            ]<[-
              instruction left (the exact opposite)
              >>>>>>[>>]+<[->>+<<]<[-]<<[<<]<<<<
            ]>
          ]<[-
            instruction print (really faster : go and print)
            >>>>>>[>>]>>>.<<<<<[<<]<<<<
          ]>
        ]<[-
          instruction dec (idem)
          >>>>>>[>>]>>>-<<<<<[<<]<<<<
        ]>
      ]<[-
        instruction read (idem)
        >>>>>>[>>]>>>,<<<<<[<<]<<<<
      ]>
    ]<[-
      instruction inc (idem)
      >>>>>>[>>]>>>+<<<<<[<<]<<<<
    ]>
    clear instruction
    [-]
    <
  ]
  >>>>+<
  check direction
  [
    <<<<[<]<<[->>+<<]>>[>]>>>>-
  ]>
  [
    -<<<<<[<]>[-<<+>>]>[>]>>>>>
  ]
  <<<<<
  <[<]>[[>]>+<]>
]
```

# Test program

With the same "Hello world" input

```
,.---.>+++[-<++>]<+..+++.>++++[->++[->++++<]<]>>.<<<[->+>+<<]++[->++++<]>.>.+++.<<++[->-----<]>-.<++[->----<]>.#H
```

We now need 13.675.534 operations for the same input, instead of the initial 65.622.379 ones (4.8x faster !!!)
