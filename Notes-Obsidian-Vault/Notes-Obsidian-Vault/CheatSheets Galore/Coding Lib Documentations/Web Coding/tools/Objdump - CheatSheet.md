## Table of Contents

- [Objdump Cheatsheet](#objdump\cheatsheet)
  - [Basic Usage](#Basic\Usage)
  - [Key Options](#Key\Options)
    - [Display Options](#Display\Options)
    - [Filter Options](#Filter\Options)
    - [Formatting Options](#Formatting\Options)
    - [Miscellaneous Options](#Miscellaneous\Options)
    - [Disassembler Options (i386/x86-64)](#Disassembler\Options\(i386/x86-64))
  - [Example Usage](#Example\Usage)

Certainly! I can help you create a master cheatsheet for `objdump` in Markdown format. This cheatsheet will be a quick reference guide, summarizing the most commonly used options and their purposes. 


# Objdump Cheatsheet

`objdump` is a powerful tool for disassembling and displaying information about binary files. This cheatsheet provides a summary of its most commonly used options.

## Basic Usage
```
objdump [option(s)] [file(s)]
```

## Key Options

### Display Options
- `-a, --archive-headers`  
  Display archive header information.

- `-f, --file-headers`  
  Display the contents of the overall file header.

- `-p, --private-headers`  
  Display object format specific file header contents.

- `-h, --headers`  
  Display the contents of the section headers.

- `-x, --all-headers`  
  Display the contents of all headers.

- `-d, --disassemble`  
  Display assembler contents of executable sections.

- `-D, --disassemble-all`  
  Display assembler contents of all sections.

- `-S, --source`  
  Intermix source code with disassembly.

- `-s, --full-contents`  
  Display the full contents of all sections requested.

- `-g, --debugging`  
  Display debug information in object file.

- `-W, --dwarf`  
  Display the contents of DWARF debug sections.

- `-t, --syms`  
  Display the contents of the symbol table(s).

- `-r, --reloc`  
  Display the relocation entries in the file.

### Filter Options
- `-j, --section=NAME`  
  Only display information for section NAME.

- `-b, --target=BFDNAME`  
  Specify the target object format as BFDNAME.

- `-m, --architecture=MACHINE`  
  Specify the target architecture as MACHINE.

### Formatting Options
- `-C, --demangle[=STYLE]`  
  Decode mangled/processed symbol names.

- `-M, --disassembler-options=OPT`  
  Pass text OPT on to the disassembler.

- `-l, --line-numbers`  
  Include line numbers and filenames in output.

- `-F, --file-offsets`  
  Include file offsets when displaying information.

- `-w, --wide`  
  Format output for more than 80 columns.

### Miscellaneous Options
- `-v, --version`  
  Display this program's version number.

- `-H, --help`  
  Display help information.

### Disassembler Options (i386/x86-64)
- `x86-64`  
  Disassemble in 64bit mode.

- `i386`  
  Disassemble in 32bit mode.

- `att`  
  Display instruction in AT&T syntax.

- `intel`  
  Display instruction in Intel syntax.

## Example Usage
To disassemble all sections of a binary in Intel syntax:
`objdump -D -M intel mybinary`
To view the file headers of a binary:
`objdump -f mybinary`

To display the contents of the section headers:
`objdump -h mybinary`



