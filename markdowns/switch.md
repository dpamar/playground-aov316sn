# Instruction switch

Now, let's switch on the instructions. A switch/case in BF can be implemented like the IF/THEN/ELSE, with an ELSE flag. Entering a case resets the ELSE flag.

We will go through each possible instruction values from lowest to highest by decreasing the instruction value again and again
* it it's null : then we found the correct instruction and enter a specific case
* if not : then continue decreasing to the next case

If Else flag is still set after all possible cases, then it'll be the "default" case (meaning, it's not a valid instruction, so it's a comment)

# Let's start

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, _current instruction_
* Cursor: on current instruction
* Input: any

# Process

* Set else bit to 1
* Subtract 8
  * if null it's Inc (8+35 = 43, '+' ASCII code) - remember : we already subtracted 35 to check if it was a #
  * otherwise Subtract 1, if null it's Read (8+1+35 = 44, ',' ASCII code)
  * otherwise Subtract 1, if null it's Dec (8+1+1+35 = 45, '-' ASCII code)
  * otherwise Subtract 1, if null it's Print (8+1+1+1+35 = 46, '.' ASCII code)
  * otherwise Subtract 14, if null it's Left (8+1+1+1+14+35 = 60, '<' ASCII code)
  * otherwise Subtract 2, if null it's Right (8+1+1+1+14+2+35 = 62, '>' ASCII code)
  * otherwise Subtract 29, if null it's While (8+1+1+1+14+2+29+35 = 91, '[' ASCII code)
  * otherwise Subtract 2, if null it's Loop (8+1+1+1+14+2+29+2+35 = 93, ']' ASCII code)
  * otherwise it's not an instruction, it's a comment, ignore

_Note_: this has been written like this to ease readability, but actually we cannot say "if null", rather "if not null", so it'll be coded with comments and ']' cases first to finish with '+'

# Code
```
>+<--------                        set else bit; is it Inc ?
[-                                 no; is it Read ?
  [-                               no; is it Dec ?
    [-                             no; is it Print ?
      [>+[-<------->]+<            no; is it Left ? (14 = 7x2)
        [--                        no; is it Right ?
	  [>+++[-<------->]+<-     no; is it While ? (29 = 4x28 plus 1)
	    [--                    no; is it Loop ?
	      [>-<[-]]             no; it's a comment: reset else flag and reset instruction
	      >[-                  yes it is Loop
	        do loop
              ]<
	    ]>[-                   yes it is While
              do while
	    ]<
          ]>[-                     yes it is Right
	    do right
          ]<
        ]>[-                       yes it is Left
          do left
        ]<
      ]>[-                         yes it is Print
        do print
      ]<
    ]>[-                           yes it is Dec
      do Dec
    ]<
  ]>[-                             yes is is Read
    do Read
  ]<
]>[-                               yes it is Inc
  do Inc
]<
```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer_, 0 
* Cursor: on last 0
* Input: unchanged
* Output: unchanged

# Test program

This program reads the code and fetch instructions (previous parts), then print inscrutctions (without comments).

Note: this is a very handy "BF code minifier", written in BF :)

```
>>>,[>+++++[-<------->]+<[>,>]>[[-]>]<<]>               load all instructions (until end of input or 35)
+[-[->+>+<<]>>[-<<+>>]<[-<<<[<]>[-<<+>>]>[>]>>]<<<[     Fetch part 1
<]>[-<+<+>>]<[->>[>]>>+<<<[<]<]<[[->>+<<]<]>>>[>]>>     ** part 2 **
                                                        the "do something"
  >+<[                                                  set else bit; is there an instruction ?
  --------[-[-[-[>+[-<------->]                         find instruction type
  +<[--[>+++[-<------->]+<-[--
    [>-<[-]]                                            comment : clear 
  >[-
    +++++++++++++[-<+++++++>]<++.[-]>                   loop: print 93
  ]<]>[-
    +++++++++++++[-<+++++++>]<.[-]>                     while: print 91
  ]<]>[-
    ++++++++[-<++++++++>]<--.[-]>                       right: print 62
  ]<]>[-
    ++++++[-<++++++++++>]<.[-]>                         left: print 60
  ]<]>[-
    +++++++++[-<+++++>]<+.[-]>                          print: print 46 
  ]<]>[-
    +++++++++[-<+++++>]<.[-]>                           dec: print 45
  ]<]>[-
    +++++++++[-<+++++>]<-.[-]>                          read: print 44
  ]<]>[-
    +++++++[-<++++++>]<+.[-]>                           inc: print 43
  ]<
     <++>                                               and in all cases increase instruction pointer (by 1 plus the offset)
  ]>[
    <<[-]>>-                                            null: reset instruction pointer
  ]
<<]                                                     Fetch loop
```

