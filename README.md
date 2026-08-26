# Assembler-360-Hello-World

Hello World in IBM System/360 assembler, done the classic way: the
program prints its message with the `WTO` ("Write To Operator") macro,
which is the shortest route to visible output on OS/360 and its
successors (MVS, z/OS).

## Files

* `hello.asm` – the program
* `hello.jcl` – a job that assembles, links and runs it in one go,
  using the stock `ASMFCLG` procedure (the source is included inline)

## How it works

`WTO` hands the message to the operating system (SVC 35), which shows
it on the operator console and in the job log. Around that there is
only the usual housekeeping: save the caller's registers, set up a
base register with `BALR`/`USING`, restore the registers and return
with return code 0 via `BR 14`.

## Running it

You need something that behaves like an IBM mainframe. The usual way
today is the free MVS 3.8j Turnkey system (TK4- or TK5) on the
Hercules emulator: start the system and feed the job to the card
reader, for example

    nc -w1 localhost 3505 < hello.jcl

`HELLO, WORLD!` then appears on the MVS console, and the assembler
listing lands in the job's printer output.

On z/OS the same source works with the High Level Assembler – just
use the `ASMACLG` procedure instead of `ASMFCLG`.
