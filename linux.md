# Linux system

- [GNU C Library](#gnu-c-library)
- [Linux kernel](#linux-kernel)
- [objdump](#objdump)
- [readelf](#readelf)
- [strace](#strace)

# GNU C Library

## Get glibc version

```shell
getconf GNU_LIBC_VERSION
```

## Unset pointer randomization for setjmp for ld.so (glibc from 2.4 to 2.22))

```shell
LD_POINTER_GUARD=0
```

# Linux kernel

## ASLR

```shell
# Off
echo 0 | /proc/sys/kernel/randomize_va_space

# On, conservative randomization
echo 1 | /proc/sys/kernel/randomize_va_space

# On, full randomization
echo 2 | /proc/sys/kernel/randomize_va_space
```

For permanent changes, add a conf file `/etc/sysctl.d/01-disable-aslr.conf`:
```shell
kernel.randomize_va_space = 0
```

## Debug file descriptor

```shell
cat /proc/<PID>/fdinfo/<FILE_NO>
```

# objdump

## List symbols

```shell
# Symbol table
objdump -t

# Dynamic symbol table
objdump -T
```

## Disassemble

```shell
# Disassemble symbol only
objdump --disassemble=<SYMBOL>

# Disassemble all sections
objdump --disassemble-all
```

## Get RPATH

```shell
objdump -x <BINARY> | grep 'R.*PATH'
```

## Extract ELF symbol value

```bash
# List symbols of ELF file (binary, .o, .so)
$ objdump -t <ELF_FILE> | grep <SYMBOL>
0000000000066020 l     O .data  <SIZE>              <SYMBOL>

# Get symbol offset (not always needed)
$ objdump -D -F <ELF_FILE> | grep <SYMBOL>
0000000000066020 <<SYMBOL>> (File Offset: <OFFSET>):

# Extract symbol with size at given offset
$ dd if=<ELF_FILE> of=<OUTPUT> ibs=1 skip=<OFFSET> count=<SIZE>
```

**Note:** for symbols that are pointers, the extraction will extract the pointed address.
A second extraction is needed to get the actual value.
For instance, `const char *STRING = "..."` needs two-steps, but `const char STRING[] = "..."` needs not.

# readelf

## Check for NX bit

```shell
# NX bit is set if 'RWE' is present
readelf -a FILE | grep GNU_STACK
```

## Get ELF dynamic section

```shell
readelf -d <BINARY/LIBRARY>
```

## Get hexdump of symbol

Find section number index for the symbol:
```shell
readelf -Ws <OBJECT> | grep <SYMBOL>
   Num:    Value          Size Type    Bind   Vis      Ndx Name
  9243: 00000000005b4f88    16 OBJECT  LOCAL  DEFAULT    N <SYMBOL>
```

Get the hex dump of the entire section:
```shell
readelf -x<Ndx> <OBJECT>
```

# strace

```shell
# Filter syscalls
strace --trace=<SYSCALLS> <COMMAND>
```
