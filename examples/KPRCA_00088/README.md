# Sorter

## Author Information
Kaprica Security

### DARPA performer group
Kaprica Security (KPRCA)

## Description

The service allows a user to input an array of ints and sort the values.
It is meant to help students understand the differences in types of sort by showing
enumerating, roughly, the number of comparisons each sort does.

The service protects the core of it's functionality by using a simple xor encryption on
its core subroutines. When the program begins the subroutines are decrypted and placed in
executable memory on the heap.

### Feature List

Enter an Array
Multiply an array
Sort Array via
    -Insertion Sort
    -Selection Sort
    -Heap Sort


## Vulnerability

### Vuln 1
The service does an improper bounds check when there are `> 1024 and <= 1048` ints in the array to be sorted.
This bounds check error allows for a 24 byte overwrite from an allocated page into another allocated page
which is marked as executable. 

The vulnerability can only be triggered in arrays of length 1025 to 1048, inclusive. In order to exploit
the vulnerability the ints in the shellcode must be stored as little endian signed ints instead of a char array.

### Generic class of vulnerability
Heap-based Buffer Overflow

### CWE classification
CWE-122: Heap-based Buffer Overflow
CWE-119: Improper Restriction of Operations within the Bounds of a Memory Buffer

## Challenges
This short CB does a very simple obfuscation technique to its code, in order to protect its IP. 
All of the code's functions have been generated by a normal -O0 compilation process (see commented 
out code in sort.c). The bytes were then taken from the disassembled functions, xor'd with key
"CS10FUN!" and stored as arrays in packed.h. 

This CB tests a CRS's ability for dynamic analysis of a service. The code is xor'd on disk to prevent
naive code inspection of the data segment, and all of the subroutines are run via executable allocated
memory on the heap.

One minor difficultly of exploiting the vulnerability is making sure to convert the shell code into an array of
signed ints as opposed to a more typical byte stream. Those ints then need to be converted into strings in order for
the CB to process them.

## Difficulty

Discovering = Medium
Proving = Medium
Patching = Medium
