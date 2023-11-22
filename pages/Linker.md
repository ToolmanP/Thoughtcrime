## Basics
- Linker script is an imperative scripting language containing multiple commands of manipulating sections of objects from various object files.
- By combining and redirecting the sections, the linker merge all the relocatable object files into one single executable, may it be a simple hello world program or a complex and dedicated operating system.
- ### Variables
- > .  = TEXT_OFFSET
- In linker script "." is a special program counter. This sets the current memory address to it points to. For example,  in this case, we set the current memory address to `TEXT_OFFSET`. If not explicitly set, the address starts at 0 and shifts based on the size of each section.
- ### Commands
- > ENTRY(symbol)
- This defines a entry point of the program basically the start symbol of the text section