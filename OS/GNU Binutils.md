二进制文件工具集
****
ld - the GNU linker        通常用于编译程序的最后一步
as - the GNU assembler
****
addr2line - Converts addresses into filenames and line numbers.
ar - A utility for creating, modifiying and extracting from archives.
c++filt - Filters to demangle encoded C++ symbols.
dlltool - Creates files for  building and using DLLs.
gold - A new, faster, ELF only linker, still in beta test.
gprof - Displays profiling information.
nlmconv - Converts object code into an ALM.
nm - Lists symbols from object files.                                           **看符号**（类型、初始值、名称）
objcopy - Copies and translates object files.                      
objdump - Displays information from object files.                      **看信息**
ranlib - Generates an index to the contents of an archive.
readelf - Displays information from any ELF format object file.   **看ELF信息** 比objdump更详细，独立于BFD库
size - Lists the section sizes of an object or archive file.              **看大小**
strings - Lists printable strings from files.
strip - Discards symbols.
windmc - A Windows compatible message compiler.
windres - A compiler for Windows resource files.

Mosts of these programs use BFD,the Binary File Discriptor library, to do low-level manipulation. Many of them also use the opcodes library to assemble and disassemble machine instructions.