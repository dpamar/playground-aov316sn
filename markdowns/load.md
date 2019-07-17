# Load code

Let's now implement the first part of our interpreter: read code to execute - and code only. The part dedicated to execution's input should remain unread for now.

Here are the characters we want to read. First value is the corresponding ASCII code, second is 35 below those values (35 is ASCII code of #, our separator).
* Separator # 35/0
* Inc + 43/8
* Read , 44/9
* Dec - 45/10
* Print . 46/11
* Left < 60/25
* Right > 62/27
* While [ 91/56
* Loop ] 93/58

Now, let's
* Read char
* Subtract 35
* If null : stop reading instruction
* Otherwise : store instruction (with an offset of -35 that does not really matter)

# Let's start

* Memory: empty
* Cursor: first cell
* Input: any

# Process

* Read char
* While current char is not null (well, yes, there may be code only !)
   * Decrease by 35
   * If not null, read next char
* Loop

# Code
```
>>>,[                    leave some cells for future use and read char
  >+++++[-<------->]+    subtract 35 and set else flag
  <[>,>]                 if not null, read next char (we are after the else flag)
  >[[-]>]                move right and reset (either it was a # : reset else flag and move left
                         or it wasn't: do nothing
<<]>                     either loop for next char or break

```

# Final state

* Memory: 0, 0, 0, _instructions_, 0, _instruction pointer = 0_ 
* Cursor: on instruction pointer 
* Input: code is read, remaining chars are execution input only
* Output: unchanged

# Test program

This test program reads all data until it reaches the # separator, or end of input stream.

Then, all instuctions are printed to the screen; followed by five #; followed by all remaining chars from the input stream.

```
>>>,[>+++++[-<------->]+<[>,>]>[[-]>]<<]>    read all instructions
<<[<]>[                                      go back to first instruction and loop
  <+++++++[->+++++<]>                        add 35
.[-]>]                                       print; clean; loop
+++++++[->+++++<]>.....                      print five #
,[.,]                                        print all remaining chars from input stream
```
