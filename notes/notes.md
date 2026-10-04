## What is a header and why is it important?

A header is a region of memory that contains metadata  
about the ROM, such as it's title, compatibilty, size 
and two checksums.


## Why reserve space for the header ?

RGB Link is not aware of the header nor does it generate 
them, for RGBFIX does them. This means that RGBLINK will 
happily write code into the section which is reserved for 
the header causing our GameBoy to crash. 

Thus, while writing code we reserve space for header by assigning
a starting place for the section to be stored at.

This can be done via `Section "HEADER_NAME", ROM0[addr]` where `addr`
corresponds to an adress in memory obtained after reserving space for 
the header.

## What are flags? 

Flags also represented by `f` are `4-bit` registers that store the 
information that is dependant on an operation. Some simple registers  
are as follows.

`z` - Zero Flag: Get's set if the result of an operation is `0`. 
`c` - Carry: Get's set if the result of an operation overflowed.
i.e: It's value exceeded the register's `8-bit` capacitly and is malformed. 
`n` - addition/subtraction: Get's set when the last operation was a subtraction
and get's reset when it is an addition. 
`h` - half-carry: Carry (overflow) accored from the lower 4 bits to the higher 4 bits.
The value is not malformed. 

## NOTE: `h` and `n` flags are invisible in normal program unless we inspect them via 
## `daa` flag.


## `cp` or Comparison 
`cp` subtracts it's operand from `a` and discards the value. However it set's the respective
flag which can then be used for other operations.
