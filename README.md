# cpp-utilities
Miscellaneous C++11 utility classes and functions

### Arena Allocator

Found in `arena.hpp`. This is an implementation of a very efficient fixed block size arena allocator. It allows allocating and freeing back to the arena (if you want to, it isn't necessary), and will use one of two strategies depending on the size of blocks you need. If the blocks are smaller than the size of a pointer, and the arena is relatively small, then it will use a bitmap along with compiler intrinsics to find free blocks. If the the blocks are at least as large as a pointer, it will use a freelist implementation. Template deduction will choose the best backend for you.

Usage is **very** simple:

    // create an arena of 1024 64-bit blocks 
    auto arena = arena::make_arena<uint64_t, 1024>();
    auto p1 = arena.allocate(); // get a single uint64_t sized block
    arena.release(p1);          // free that block back into the system 
                                // (once again not necessary, the arena will clean itself up)
    
On my system , the allocate function when using the freelist strategy, allocate was **as few a 6 instructions**. Some of which were simple `nullptr` checks.

### Bitset Utility Functions

Found in `bitset.hpp`. This header provides a nice utility function to find the first set bit in a bitset. When possible using GCC intrinsics to do it in O(1) time, but falling back on an iterative implementation when this is not possible.

    std::bitset<32> bs;
    bs[4] = true;
    bs[10] = true;
    int bit = bitset::find_first(bs); // returns 4
    
The function is defined to return `bitset.size()` when no bits are set, this is similar to containers returning `.end()` for find operations.

### Bitwise Operations

### String Utility Functions
