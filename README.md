# Linux Kernel Analyzer

This is a static kernel analyzer forked from [KINT](https://github.com/CRYPTOlab/kint).

## generate bitcode for Linux kernel
build LLVM with [this patch](https://github.com/Markakd/LLVM-O0-BitcodeWriter), then build kernel with the compiled clang.

## build the analyzer
set the LLVM_BUILD in the Makefile.inc to your local LLVM BUILD directory before running make.

## run the analyzer
```bash
./analyzer `find your_bitcode_folder -name "*.c.bc"` 
```


## Erin's note:

The code has been modified to output a csv list of `struct_name,cache_name` to indicate the cache where each struct is allocated.
It doesn't work well for generic `kmalloc` calls but it works well with `kmem_cache_alloc` that use a global variable as their cache.
For the `kmalloc` calls we simply use the max size of the object and indicate its potential kmalloc cache, which may notbe precise.
This method can be fixed later to be more accurate by going down the use chain and find the hardcoded kmalloc caches.

run it with: ```./analyzer `find <ROOT OF KERNEL SOURCE> -name "*.c.bc" ` --debug-verbose=0 2> struct_cache_res.txt```
