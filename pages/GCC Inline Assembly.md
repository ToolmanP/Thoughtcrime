- GCC provides a decent assembly extension to write a templated assembly for non-trivial but 
  title:: GCC Inline Assembly
  important purposes. e.g.
	- Spin locks
	- Memory Order
	- Memory Clobbers
	- Change And Store
	- setjmp/longjmp impl
- Here's a snippet of exemplified inline assembly codes performing a simple addition.
- ```c
  int64_t asm_add(int64_t a, int64_t b) {
    int64_t c = 0;
    asm volatile(
        "mov %1, %0\n\t" // c = a
        "add %2, %0" // c = c + b
        : "=r"(c)
        : "r"(a), "r"(b));
    return c;
  }
  ```
- To sum up, a single inline assembly can be annotated as follows ways.
- > warning: some clobbers or constraints are not platform-independent and shall not be overused.
- ```c
  asm <volatile?> ("<assembly template>" : 
                 "<output-constraint>"(<output operands>):
                 "<input-constraint>"(<input operands>):
                 <clobbers>?)
                 
  // volatile means that this line of code, even if it's nop, can not be optimized by the compiler.
  ```
- ### rw-constraints
- id:: 65225a73-d96b-415d-98da-31647c32e361
  > Output constraints must begin with either = (a variable overwriting an
  existing value) or + (when reading and writing). When using
  =, do not assume the location contains the existing value
  on entry to the asm, except when the operand is tied to an input; see [Input Operands](https://gcc.gnu.org/onlinedocs/gcc/extensions-to-the-c-language-family/how-to-use-inline-assembly-language-in-c-code.html#inputoperands).
- & -- means early clobbering indicating that a input register cannot be overshadowed by an output register
- m -- means that this variable should be backed up by memory access
- r -- means that this variable should be backed up by certain register access
- ### data-constraints
- ### register assignments
- Notice that in x86_64, memory indexing and some logical arthimatical operands can only be assigned to some certain registers. To mitigate wrong compiler assignment, one must use asm() to assign the correct registers.
- ```c
  uintptr_t i asm("rcx") = 0
  ```
- This line of code shows that i should only resides in %rcx and uses only 64-bit register, preventing any rewrite of ecx, cl registers
- ### clobbers
- **"memory"**: means that this segment of assembly must perform memory interaction. Useful when doing some multithreading works or provide memory barrier for any out of order instructions.
-
-