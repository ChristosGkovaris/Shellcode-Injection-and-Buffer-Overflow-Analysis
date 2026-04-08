# Shellcode-Injection-Analysis

This project implements a **Buffer Overflow attack** from scratch on a **64-bit Linux server**, including **Stack Analysis** using GDB, **NOP-sled** construction, and an automated **Exploit Script** in Perl. It was developed as part of the **ΜΥΕ007 – Security of Computer and Communication Systems** course at the **University of Ioannina**.

---

## TABLE OF CONTENTS
1. [Overview](#overview)
2. [Features](#features)
3. [Input Data](#input-data)
4. [Algorithms Implemented](#algorithms-implemented)
5. [Implementation Details](#implementation-details)
6. [Usage](#usage)
7. [License](#license)
8. [Contact](#contact)

---

## OVERVIEW

The **Shellcode-Injection-Analysis** project focuses on compromising a 64-bit echo server by manipulating its memory layout. 

Students analyze, implement, and verify:
- Stack frame organization including local variables, RBP, and saved RIP. 
- Vulnerability identification in C programs compiled without modern protections.
- Precise offset calculation using unique pattern generation.
- Remote shell access via TCP-based payload delivery.

The full analysis and exploit methodology are documented in the provided report.

---

## FEATURES

- **Memory Vulnerability Analysis**
    - Identification of stack frames and return addresses using GDB.
    -  Disabling security features such as Stack Canaries and DEP for educational analysis.

- **Exploit Development**
    - Creation of a **NOP-sled** (80 bytes) to increase exploit reliability by providing a landing zone.
    - Integration of a 64-bit **shellcode** payload to spawn `/bin/sh`.
    - Automatic calculation of padding to precisely overwrite the **RIP register** at an offset of 152 bytes.

- **Network-Based Execution**
    - Automated TCP socket connection using **Perl**.
    - Real-time monitoring of successful exploitation within the debugger.

---

## INPUT DATA

- **Source Files**: `simple_server.c` (vulnerable server), `shell.c` (payload source), and `exploit.pl`.
- **Target OS**: Debian 64-bit Linux (Kernel 6.12.57).
- **Hardware Info**: Tested on AMD Ryzen 7 7730U environment.
- **Exploit Inputs**:
    - 256-byte unique patterns for offset discovery.
    - Hexadecimal shellcode vectors for 64-bit architectures.

---

## ALGORITHMS IMPLEMENTED

1. **Stack Frame Mapping**
    - Examining memory 512 bytes around the stack pointer (`$rsp`).
    - Identifying exactly where the buffer overflows into the saved frame pointer.

2. **Offset Discovery**
    - Utilizing `pattern_create.rb` and `pattern_offset.rb` from Metasploit.
    - Locating the exact 152-byte offset required to reach the RIP.

3. **Payload Construction**
    - Arrangement: `[NOP-sled (80b)] + [Shellcode (31b)] + [Padding (41b)] + [Return Address (8b)]`.
    - Little-endian address conversion for the target stack pointer (`0x7fffffffe010`).

4. **Remote Shell Execution**
    - Redirecting program control flow to the NOP-sled address.
    - Executing system calls to invoke an interactive shell.

---

## IMPLEMENTATION DETAILS

- **Language**: C for the server, Perl for the exploit script.
- **Debugger**: GDB (GNU Debugger) used for real-time register tracking.
- **Compiler**: `gcc` with `-fno-stack-protector` and `-z execstack` flags.
- **Environment**: Virtualized Linux via VMware Workstation Pro.

---

## USAGE

1. Compile the server without protections
```bash
gcc -g -fno-stack-protector -z execstack -o simple_server simple_server.c
```
2. Run the server in GDB
```bash
gdb -q simple_server
(gdb) run
```
3. Execute the exploit script
```bash
perl exploit.pl
```
4. Verify the shellLook for the $ prompt in the GDB terminal to confirm remote access to /bin/sh.

---

## LICENSE

This project was developed for academic purposes as part of the **ΜΥΕ007 – Security of Computer and Communication Systems** course at the University of Ioannina.

Implementation and report by Christos-Grigorios Gkovaris.

---

## CONTACT

**Christos-Grigorios Gkovaris**
University of Ioannina – Computer Science and Engineering
[GitHub](https://github.com/ChristosGkovaris)