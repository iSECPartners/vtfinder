vtfinder
========

###pykd script to dynamically find vtables on heap###

This [pykd](https://pykd.codeplex.com) script helps find vtables on heaps where mostly static data is stored
(no obvious vtables, function pointers, etc.). It sets a breakpoint on free (ntdll!RtlFreeHeap) and checks
whether the first pointer-sized value on the chunk is an address in any of the loaded modules (this is usually
an indication of something interesting, often a vtable). The script handles x86, x64 and allows you to specify
which heap you are interested in. It also automatically adds dynamically loaded and delay-loaded modules to the
list of monitored address ranges. Syntax is as follows:

```
!py path/to/vtfinder.py [arch] [heap]
```
where [arch] is X86/X64 (defaults to x64) and [heap] is the hex address of your heap of interest
(!heap to list all heaps). Here is a sample output (full output attached) from running the script
against notepad x64:

```
Breakpoint 0 hit
================> FOUND MSCTF!CEditSessionObject::`vftable' (0x7fefe2030d0) ON HEAP CHUNK 0x371e60
Entry             User              Heap              Segment               Size  PrevSize  Unused    Flags
-------------------------------------------------------------------------------------------------------------
0000000000371e50  0000000000371e60  0000000000350000  0000000000350000        70        50        38  busy extra fill


Breakpoint 0 hit
================> FOUND MSCTF!CEditSessionObject::`vftable' (0x7fefe2030d0) ON HEAP CHUNK 0x371e60
Entry             User              Heap              Segment               Size  PrevSize  Unused    Flags
-------------------------------------------------------------------------------------------------------------
0000000000371e50  0000000000371e60  0000000000350000  0000000000350000        70        50        38  busy extra fill


Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
Breakpoint 0 hit
================> FOUND MSCTF!CBStoreHolderWin32::`vftable' (0x7fefe204e60) ON HEAP CHUNK 0x371da0
Entry             User              Heap              Segment               Size  PrevSize  Unused    Flags
-------------------------------------------------------------------------------------------------------------
0000000000371d90  0000000000371da0  0000000000350000  0000000000350000        70       340        38  busy extra fill


Breakpoint 0 hit
================> FOUND MSCTF!CBStoreHolderWin32::`vftable' (0x7fefe204e60) ON HEAP CHUNK 0x371da0
Entry             User              Heap              Segment               Size  PrevSize  Unused    Flags
-------------------------------------------------------------------------------------------------------------
0000000000371d90  0000000000371da0  0000000000350000  0000000000350000        70       340        38  busy extra fill
```
