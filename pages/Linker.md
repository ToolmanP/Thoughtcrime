## Basics
- Linker script is an imperative scripting language containing multiple commands of manipulating sections of objects from various object files.
- By combining and redirecting the sections, the linker merge all the relocatable object files into one single executable, may it be a simple hello world program or a complex and dedicated operating system.
- ### Variables
- > .  = TEXT_OFFSET
- In linker script "." is a special program counter. This sets the current memory address to it points to. For example,  in this case, we set the current memory address to `TEXT_OFFSET`. If not explicitly set, the address starts at 0 and shifts based on the size of each section.
- In Linker Script EVERY user-defined variable is a symbol introduced to the program.
- For Example,
- ```Linker
  floating_point = 0;
  SECTIONS
  {
    .text :
      {
        *(.text)
        _etext = .;
      }
    _bdata = (. + 3) & ~ 3;
    .data : { *(.data) }
  }
  ```
- In this linker script, we define _etext at the end of .text which will introduce a symbol _etext into the symbol table which can instead be used in a kernel which helps determine the TEXT location when setting up the MMU
- ### Commands
- > ENTRY(symbol)
- This defines a entry point of the program basically the start symbol of the text section.
- T