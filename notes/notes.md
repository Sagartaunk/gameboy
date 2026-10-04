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
