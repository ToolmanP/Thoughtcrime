- Rust Macros are invoked **after** the construction of the abstract syntax tree  in the first pass instead of in the first phasing of lexical analysis.
- Rust Macros happens after the syntactic analysis and before the real semantic analysis kicks in, this is a huge difference or enhancement than C/C++ macros.
- C++ template instantiation also happens before semantic analysis but unlike Rust, this happens after the lexical analysis and before constructing the real abstract syntax tree. But it's rather the same and more powerful, since C++ special template expansion and `constexpr` supports make s it easier to manipulate every token we want.
- The input to every macro is a single non-leaf token tree.
- After the invocation of the macro, the abstract syntax tree is reformatted.
- Since macros are parsed and merged into the original syntax tree,  so they can always appear in
	- Patterns
	- Statements
	- Expressions
	- Items
	- `Impl` Items
- Rust Macro Engine will try to treat every single macro as one of the syntactic items you place them in. e.g. in the following
- ```rust
  let a = foo!(1);
  ```
- `foo!` will be treated as `$expr`