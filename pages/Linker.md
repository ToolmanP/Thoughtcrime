## Basics
- Linker script is an imperative scripting language containing multiple commands of manipulating sections of objects from various object files.
- By combining and redirecting the sections, the linker merge all the relocatable object files into one single executable, may it be a simple hello world program or a complex and dedicated operating system.
- ### Variables
- > .  = TEXT_OFFSET
- In linker script "." is a special location counter. This sets the current memory address to it points to. For example,  in this case, we set the current memory address to `TEXT_OFFSET`. If not explicitly set, the address starts at 0 and shifts based on the size of each section.
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
- Similarly, _bdata will be defined as the address following the text output section aligned upward to a 4 byte boundary
- > PROVIDE(symbol = expression)
- This is a weak definition of a symbol and can be overwritten by the symbol defined in the source object files. IF ONLY the object file refers to it and does not define it, it will be used as a default symbol definition.
- > HIDDEN(symbol = expression)
- This is a symbol hidden in the linker script and won't be exported to the global symbol table.
- > ENTRY(symbol)
- This defines a entry point of the program basically the start symbol of the text section.
-
- ## Alignment
- > ALIGN(4K)
- This is simple. We strictly align the location counter to 4K Page.
- ## Defining Sections And Other Stuffs
- ```linker
  .text [./ALIGN(4K)]: { *(.text*)}
  ```
- In this case, the linker will create a section `.text` and merge all the data in the text section defined by the source object files. Notice that, it's optional to write a "." or "ALIGN" to explicitly define the address it points to. If not provided, the linker will default it to the strictest alignment for the section.
- The part that is wrapped inside the curly braces is called the **Input Sections** and the keyword defined followed by  a colon is called a **output section**.
- #### Input Section
- ```linker
  *(.text*)
  ```
- The  input section consists of two parts the input file and the section in the file. In this case, this will include the section beginning with ".text" across all the input object files.
- We can fine-grain the input file by using the archive namespace access, just like things did in c++
- > (archive): file
- we can specify an archive file and choose only from that archive file.
- ### Output Sections
- id:: 655e2669-98ae-4048-942a-096486a8eb8e
  ```linker
  section [address] [(type)] :
    [AT(lma)]
    [ALIGN(section_align) | ALIGN_WITH_INPUT]
    [SUBALIGN(subsection_align)]
    [constraint]
    {
      output-section-command
      output-section-command
      …
    } [>region] [AT>lma_region] [:phdr :phdr …] [=fillexp]
  ```
- #### AT
- AT Specify the load address of the memory explicitly instead of being assigned the same address as the virtual address (directed by the location counter)