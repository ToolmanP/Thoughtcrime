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
- In this linker script, we define _etext at the end of .text which will introduce a symbol _etext into the symbol table which can instead be used in a kernel which helps determine the TEXT location when setting up the MMU unit in operating system. However, unlike the symbol defined in the source code, the symbol does not refer to any special or reserved memory space in the memory, which means, yes, it is  a merely a variable that only can be referenced from but cannot be dereferenced.
-
- Similarly, _bdata will be defined as the address following the text output section aligned upward to a 4 byte boundary
- > PROVIDE(symbol = expression)
- This is a weak definition of a symbol and can be overwritten by the symbol defined in the source object files. IF ONLY the object file refers to it and does not define it, it will be used as a default symbol definition.
- > HIDDEN(symbol = expression)
- This is a symbol hidden in the linker script and won't be exported to the global symbol table.
-
- ### Commands
- > ENTRY(symbol)
- This defines a entry point of the program basically the start symbol of the text section.