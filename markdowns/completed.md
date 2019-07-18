# Completed interpreter

We just have to puts all these parts together now :)

# Code (minified version)

```
>>>,[>+++++[-<------->]+<[>,>]>[[-]>]<<]>+[-[->+>+<<]>>[-<<+
>>]<[-<<<[<]>[-<<+>>]>[>]>>]<<<[<]>[-<+<+>>]<[->>[>]>>+<<<[<
]<]<[->>+<<]<[[->>+<<]<]>>>[>]>>>+<[>->+>[<<++++++++[-<-----
-->]+<[--[>-<[-]]>[->>-<<]<]>[->>+[<-]<[>>[-]<<-<]>+<]<+>>-]
<[-<]+<[--------[-[-[-[>+[-<------->]+<[--[>+++[-<------->]+
<-[--[>-<[-]]>[->>>>[->+>+<<]>>[-<<+>>]<+[->>>>[>>]+>>[-]>[-
<<+>>]<<<[<<]<<]>>>>[>>]<[->>+<<]>>[-<<+<[<<]<<+>>>>[>>]>]<<
<[[->>+<<]>[->>+<<]<<<]<<[[-]<<<->[-]+>>]<<<<<]<]>[->>>>[->+
>+<<]>>[-<<+>>]<+[->>>>[>>]+>>[-]>[-<<+>>]<<<[<<]<<]>>>>[>>]
<[->>+<<]>>[-<<+<[<<]<<+>>>>[>>]>]<<<[[->>+<<]>[->>+<<]<<<]<
+<[>-<[-]]>[-<<<<+>>>>]<<<<<<]<]>[->>>>+<<<<]<]>[->>>>-<<<<]
<]>[->>>>[->+>+<<]>>[-<<+>>]<+[->>>>[>>]+>>[-]>[-<<+>>]<<<[<
<]<<]>>>>[>>]<.<[[->>+<<]>[->>+<<]<<<]<<<<<<<]<]>[->>>>[->+>
+<<]>>[-<<+>>]<+[->>>>[>>]+>>[-]>[-<<+>>]<<<[<<]<<]>>>>[>>]<
-<[[->>+<<]>[->>+<<]<<<]<<<<<<<]<]>[->>>>[->+>+<<]>>[-<<+>>]
<+[->>>>[>>]+>>[-]>[-<<+>>]<<<[<<]<<]>>>>[>>]<,<[[->>+<<]>[-
>>+<<]<<<]<<<<<<<]<]>[->>>>[->+>+<<]>>[-<<+>>]<+[->>>>[>>]+>
>[-]>[-<<+>>]<<<[<<]<<]>>>>[>>]<+<[[->>+<<]>[->>+<<]<<<]<<<<
<<<]>>>[-<<+<<<-->>>>>]<<[->>+<<]<<<++>]]>[[-]<<[-]>>]<<]
```

# Code with comments

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


0 0 0 {instructions} 0 IP current bit_else 0 inactive_flag direction_flag memory_pointer MPcopy/read_value 0 0 0 0 0 {memory}

>>>,[>+++++[-<------->]+<[>,>]>[[-]>]<<]>
+[
  -[->+>+<<]>>[-<<+>>]<[-<<<[<]>[-<<+>>]>[>]>>]<<<[<]>[-<+<+>>]<[->>[>]>>+<<<[<]<]<[->>+<<]<[[->>+<<]<]>>>[>]>>

  parse current instruction
  >+<[>->+>
  [
    check if instruction is while (increase flag) or loop (decrease flag)
    <<++++++++[-<------->]+<
    [
      --
      [>-<
        clear instruction read
        [-]
      ]>[-
        instruction: loop
        decrease inactive_flag
        >>-<<
      ]<
    ]>[-
      instruction: while
      increase inactive_flag
      >>+
    
      if inactive_flag = 0 then reset direction_flag
      [<-]<[>>[-]<<-<]>+
      <
    ]<
    set instruction read to 1 to consider it as a non instruction
    +>>-
  ]
  <[-<]
  +<
  [
    --------
    [
      -
      [
        -
        [
          -
          [
            >+[-<------->]+<
            [
              --
              [
                >+++[-<------->]+<-
                [
                  --
                  [
                    not an instruction
                    >-<[-]
                  ]>[-
                    instruction: loop
                  duplicate memory pointer and move to copy
                  >>>>[->+>+<<]>>[-<<+>>]<+
                  [
                    -
                    go to next memory cell
                    >>>>[>>]+>>
                    (re)init/copy memory indicator
                    [-]>[-<<+>>]<<<[<<]<<
                  ]
                  go to current memory cell and copy value
                  >>>>[>>]<[->>+<<]>>[-<<+<[<<]<<+>>>>[>>]>]<<
                  move array back to initial position
                  <[[->>+<<]>[->>+<<]<<<]
                  <<
                  check current memory value: do nothing if null and move to corresponding while otherwise
                  [
                    if not null (current position: next to memory_pointer)
                    reset value
                    [-]
                    decrease inactive_flag and change direction_flag
                    <<<->[-]+>>
                  ]
                  <<<<<
                  ]<
                ]>[-
                  instruction: while
                  duplicate memory pointer and move to copy
                  >>>>[->+>+<<]>>[-<<+>>]<+
                  [
                    -
                    go to next memory cell
                    >>>>[>>]+>>
                    (re)init/copy memory indicator
                    [-]>[-<<+>>]<<<[<<]<<
                  ]
                  go to current memory cell and copy value
                  >>>>[>>]<[->>+<<]>>[-<<+<[<<]<<+>>>>[>>]>]<<
                  move array back to initial position
                  <[[->>+<<]>[->>+<<]<<<]
                  check current memory value: do nothing if not null and move to corresponding loop otherwise
                  <+<[>-<[-]]>[-<
                    if null (current position: next to memory_pointer)
                    increase inactive_flag
                    <<<+>>>
                  >]<
                  <<<<<
                ]<
              ]>[-
                instruction: right
                >>>>+<<<<
              ]<
            ]>[-
              instruction: left
              >>>>-<<<<
            ]<
          ]>[-
            instruction: print
            duplicate memory pointer and move to copy
            >>>>[->+>+<<]>>[-<<+>>]<+
            [
              -
              go to next memory cell
              >>>>[>>]+>>
              (re)init/copy memory indicator
              [-]>[-<<+>>]<<<[<<]<<
            ]
            go to current memory cell and print value
            >>>>[>>]<.
            move array back to initial position
            <[[->>+<<]>[->>+<<]<<<]
            <<<<<<<
          ]<
        ]>[-
          instruction: dec
          duplicate memory pointer and move to copy
          >>>>[->+>+<<]>>[-<<+>>]<+
          [
            -
            go to next memory cell
            >>>>[>>]+>>
            (re)init/copy memory indicator
            [-]>[-<<+>>]<<<[<<]<<
          ]
          go to current memory cell and decrease value
          >>>>[>>]<-
          move array back to initial position
          <[[->>+<<]>[->>+<<]<<<]
          <<<<<<<
        ]<
      ]>[-
        instruction: read

        duplicate memory pointer and move to copy
        >>>>[->+>+<<]>>[-<<+>>]<+
        [
          -
          go to next memory cell
          >>>>[>>]+>>
          (re)init/copy memory indicator
          [-]>[-<<+>>]<<<[<<]<<
        ]
        go to current memory cell and read value
        >>>>[>>]<,
        move array back to initial position
        <[[->>+<<]>[->>+<<]<<<]
        <<<<<<<
      ]<
    ]>[-
      instruction: inc
      duplicate memory pointer and move to copy
      >>>>[->+>+<<]>>[-<<+>>]<+
      [
        -
        go to next memory cell
        >>>>[>>]+>>
        (re)init/copy memory indicator
        [-]>[-<<+>>]<<<[<<]<<
      ]
      go to current memory cell and increase value
      >>>>[>>]<+
      move array back to initial position
      <[[->>+<<]>[->>+<<]<<<]
      <<<<<<<
    ]<
    increase or decrease instruction counter based on direction_flag
    >>>>[-<<+<<<-->>>>>]<<[->>+<<]<<<++>
  ]]>[[-]<<[-]>>]<
<]
```

# Test program

It is possible to test the interpreter with this input for example

```
,.---.>+++[-<++>]<+..+++.>++++[->++[->++++<]<]>>.<<<[->+>+<<]++[->++++<]>.>.+++.<<++[->-----<]>-.<++[->----<]>.#H
```

It prints some chars calculated from the first one given in input (H) to end up with HELLO WORLD

Note: the original code executes in less than 850 operations (849 exactly)

The interpreter runs this code in 65.622.379 operations, so an average ratio of 77.924 operations to process one single instruction !!

Executing BF using a BF-written BF interpreter is, currently, 80.000 times longer...
