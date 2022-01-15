# ICS-53

## Struct


### <stdio.h>


### Strings & <string.h>

- Copy
  - strcpy
- strlen
- strchr ≈ python find()

- loop
  - `for(char* i=line;*i;++i)`
    - 当`*i==\EOF`时，即末尾时，停止
- Formatted strings
  - int sscanf(char *string, char *format, ...)
  - int sprinf(char *buffer, char *format, ...)
  - all return the number of successful characters
  
### Structs
- types can be defined
```C
    typedef struct {int x; int y} Point; 
    Point p;
    p.x = 0; p.y =0;
```
- Overall size is sum of elements and paddings in between
  - ```C
    struct {
        char x;
        int y;
        char z;
    } s1;
    sizeof(s1) != 1+4+1 // == 12 = 4(char+padding=3)+4(int)+4(char+padding=3)
    struct {
        char x,z;
        int y;
    } s2;
    sizeof(s2) != 1+4+1 // == 8 = 4(char+char+padding=2)+4(int)
    ```
    - word alignment here
      - It benefits array
- can model real memory layout
```C
    typedef struct {
        int a:2; //size in bits
        int b:1;
    } Complex
```
- bit fields
- Deferencing pointers to struct elements
  - `(*structPtr).element`
  - `structPtr->element`

#### Union

```C
  union u-tag {
      int iv
  }
```
- like structs
- occpy same memory space
    - 无论读哪个element，都按头地址读
- can hold different types at different times
- overall size is largest of elements
  
#### Memory Pitfalls 常见错误

- Dereferencing bad pointers
```C
int val;
scanf("%d",val); //should be &val
```
- Reading Uninitialized Memory
```C
int *y = malloc(...)
y[0] += 1; // 需要初始化 y[0]
```  
- Overwriting Memory
```C
int **p;
p = malloc(N*sizeof(int));
for (i=0;i<N;++i)
    p[i] = malloc(M*sizeof(int))
```
- referencing a pointer instend of value
  - should it be `*p` or just `p`?
- referencing Nonexistent variables
```C
int helper(){
    int val;
    return &val; // 该内存返回时已经去掉
}
```
- free one block multiple times
- referencing freed blocks
- failling to free allocated blocks
- free only parts of data structure
  - 比如链表只free head

##### DEALING with bugs
- gdb
- Valgrind
- glibc mallloc contains checking code
  - `setenv MALLOC_CHECK_ 3`


### The Lost Art of Structure Packing 笔记
- [Link to the Article](http://www.catb.org/esr/structure-packing/)
- ASSUME x86 or ARM or any with *self-alignment*

#### 1. Who should read this

#### 2. Why I wrote it

#### 3. Alignment requirement

-  the way your compiler lays out basic datatypes in memory is constrained in order **to make memory accesses faster**
-  each type except char has an ***alignment requirement***; 
   -  任何type的地址都必须被`sizeof(type)`整除
   -  Ex: 2-byte shorts must start on an even address, 
   -  4-byte ints or floats must start on an address divisible by 4,
   -  *Signed or unsigned makes no difference*
   -  The jargon for this is that basic C types on x86 and ARM are ***self-aligned***. 
   -  Pointers, whether 32-bit (4-byte) or 64-bit (8-byte) are *self-aligned* too.
- Self-alignment makes access faster because it facilitates generating ***single-instruction fetches*** and puts of the typed data. 
- on some ***older ones*** forcing your C program to violate alignment rules didn’t just slow your code down, it caused **an illegal instruction fault**. 
- Also, self-alignment is **not the only** possible rule.
- NTP默认有alignment
  - platforms with padding rules other than self-alignment are 
    - either nonexistent or 
    - confined to such specialized niches that they’re never either NTP servers or clients.

#### 4. Padding
In fact, the hidden assumption that the ***allocated order of static variables is their source order*** is not necessarily valid
```C
  struct s{
    char *p;
    char c;
    int x;
  }
```
- Here’s what actually happens. The storage for p starts on a self-aligned 4- or 8-byte boundary depending on the machine word size. This is ***pointer alignment*** - the strictest possible.
```C
char *p;      /* 4 or 8 bytes */
char c;       /* 1 byte */
char pad[3];  /* 3 bytes */
int x;        /* 4 bytes */
```
- The `pad[3]` character array represents the fact that there are three bytes of **waste space** in the structure.  
  - The old-school term for this was "slop". 
  - The value of the padding bits is **undefined**; 
    - in particular ***it is not guaranteed that they will be zeroed***.
```C
char c;
char pad1[M];
char *p;
char pad2[N];
int x;
```
- what can we say about M and N?
  - First, in this case `N` will be zero
  - The value of `M` is **less predictable**.
    - *If the compiler happened to map c to the last byte of a machine word, the next byte (the first of p) would be the first byte of the next one and properly pointer-aligned. M would be zero.*
    - It is more likely that c will be mapped to the **first byte of a machine word**. In that case M will be whatever padding is needed to ensure that **p has pointer alignment** - 3 on a 32-bit machine, 7 on a 64-bit machine.
- On a platform with self-aligned types, **arrays** of char/short/int/long/pointer have **no internal padding**;
  - each member is automatically self-aligned at the end of the next one
- same to **Go structs**, and to **Rust structs with the "repr(C)" attribute**, with only syntactic changes.
  
#### 5. Structure alignment and padding
- In general, a struct instance will have ***the alignment of its widest scalar member***. 
  - Compilers do this as the easiest way to **ensure that all the members are self-aligned for fast access**.
- in C (and Go, and Rust) **there is no leading padding**.  
  - In C++ this may not be true; 
```C
struct foo1 {
    char *p;
    char c;
    long x;
};
/* On 64-bit sizeof(void *)==8 */
struct foo1 {
    char *p;     /* 8 bytes */
    char c;      /* 1 byte*/
    char pad[7]; /* 7 bytes */
    long x;      /* 8 bytes */
};
```

#### Trailing Padding

- **stride address of a structure**
  - It is the first address following the structure data that has the same alignment as the structure.
- The general rule of trailing structure padding is this: the compiler will behave as though the structure has trailing padding out to its stride address
- If your structure has **structure members**, the inner structs want to have the alignment of **longest scalar** too
```C
struct foo5 {
    char c;
    struct foo5_inner {
        char *p;
        short x;
    } inner;
};

```

- The char *p member in the inner struct forces the **outer struct to be pointer-aligned as well as the inner**

```C
struct foo5 {
    char c;           /* 1 byte*/
    char pad1[7];     /* 7 bytes */
    struct foo5_inner {
        char *p;      /* 8 bytes */
        short x;      /* 2 bytes */
        char pad2[6]; /* 6 bytes */
    } inner;
};
```

- This structure gives us a hint of **the savings that might be possible from repacking structures**. 
  - Of 24 bytes, 13 of them are padding. That’s more than 50% waste space!

#### 6. Bitfields

```C
struct foo6 {
    short s;
    char c;
    int flip:1;
    int nybble:4;
    int septet:7;
};
```
- What they give you the ability to do is declare structure fields of smaller than character width, down to 1 bit, like this.

- The thing to know about bitfields is that they are implemented with word- and byte-level mask and rotate instructions operating on machine words, and **cannot cross word boundaries**.
  - C99 guarentees that bit-fields will be packed as tightly as possible, provided they don’t cross storage unit boundaries
  - This restriction is relaxed in C11 (6.7.2.1p11) and C++14 ([class.bit]p1); 
    - It’s up to the implementation to decide;
      - GCC leaves it up to the ABI, which for x64 does prevent them from sharing an allocation unit.
```C
/* 32-bit machine */
struct foo6 {
    short s;       /* 2 bytes */
    char c;        /* 1 byte */
    int flip:1;    /* total 1 bit */
    int nybble:4;  /* total 5 bits */
    int pad1:3;    /* pad to an 8-bit boundary */
    int septet:7;  /* 7 bits */
    int pad2:25;   /* pad to 32 bits */
};
```

- As with normal structure padding, the padding bits **are not guaranteed to be zero**; C99 mentions this
- The base type of a bit field is interpreted for **signedness** but not necessarily for **size**.
  - It is up to implementors 
    - whether "short flip:1" or "long flip:1" are supported, 
    - whether those base types change the size of the storage unit the field is packed into.

```C
struct foo7 {
    int bigfield:31;      /* 32-bit word 1 begins */
    int littlefield:1;
};

struct foo8 {
    int bigfield1:31;     /* 32-bit word 1 begins /*
    int littlefield1:1;
    int bigfield2:31;     /* 32-bit word 2 begins */
    int littlefield2:1;
};

struct foo9 {
    int bigfield1:31;     /* 32-bit word 1 begins */
    int bigfield2:31;     /* 32-bit word 2 begins */
    int littlefield1:1;
    int littlefield2:1;   /* 32-bit word 3 begins */
};
```
- Again, C11 and C++14 may pack `foo9` tighter, but it would perhaps be ***unwise*** to count on this.

#### 7. Structure reordering

- The first thing to notice is that slop only happens in two places. 
  1. One is where storage bound to a larger data type (with stricter alignment requirements) follows storage bound to a smaller one. 
  2. The other is where a struct naturally ends before its stride address, requiring padding so the next one will be properly aligned

- The simplest way to eliminate slop is to reorder the structure members by **decreasing alignment**. 
```C
struct foo10 {
    char c;
    struct foo10 *p;
    short x;
};
struct foo10 {
    char c;          /* 1 byte */
    char pad1[7];    /* 7 bytes */
    struct foo10 *p; /* 8 bytes */
    short x;         /* 2 bytes */
    char pad2[6];    /* 6 bytes */
};
```
24 bytes TO 16 bytes
```C
struct foo11 {
    struct foo11 *p;
    short x;
    char c;
};
struct foo11 {
    struct foo11 *p; /* 8 bytes */
    short x;         /* 2 bytes */
    char c;          /* 1 byte */
    char pad[5];     /* 5 bytes */
};
```

- Reordering is not guaranteed to produce saving
- Curiously, strictly ordering your structure fields **by increasing size** also works to mimimize padding. 

- Why, if reordering for minimal slop is so simple, C compilers **don’t do it automatically**?
  - Automatic reordering would **interfere** with a systems programmer’s ability to lay out structures that exactly match the byte and bit-level layout of memory-mapped device control block
  -  **Rust** makes the opposite choice; by default, its compiler *may* reorder structure fields.

#### 8~ 未完待续