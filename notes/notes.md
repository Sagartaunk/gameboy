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


## JUMPS

Rhw CPU has a special-purpose register `PC` which stores the address of the instruction 
currently being executed. Jump allows us to arbitraly modify this `PC` register. 
(Kind of the thing flow-control statements do in other languages).

Instruction	  Mnemonic	  Effect
Jump	          jp	      Jump execution to a location
Jump Relative	  jr	      Jump to a location close by
Call	          call	    Call a subroutine
Return	        ret	      Return from a subroutine


### Difference between `jp` and `jr` 
`jr` jumps relative to the current value of the `PC` register 
and can only go forward/backward by `128` bytes. The advantage of `jr` is that 
it takes `2 bytes` of storage instead of `3` taken by `jp` and also costs one less 
cpu cycle.

### NOTE: Conditional Jumps can be achieved by using a label as in the following example. 

` ; Copy the tile data
  ld de, Tiles
  ld hl, $9000
  ld bc, Tiles.End - Tiles
  CopyTiles:
  ld a, [de]
  ld [hli], a
  inc de
  dec bc
  ld a, b
  or a, c
  jr nz, CopyTiles
`
## Memory:
There are two types of `Ram` in a gameboy, `High Ram` and `Work Ram`

### High Ram: 
It is a small memory region which is extremely fast and convienet to access
from the cpu. It is used to store small amounts of data. 

### Work Ram:
Normal ram.


## Registers:
These are small places of memory that the CPU uses as a workspace. 
There are registers for everything and also a few `General Purpose Registers`.
These differ from the countless registers a cpu has for performing calculations 
as they do no specialise in one task and can be used for anything as the name implies. 

The gameboy has 7 of these General Purpose registers which are `8-bit` in memory.
