# 8051-timing-db


## Introduction

The purpose of this repository is to store machine-readable tables of
instruction timings for various microcontrollers and CPU microarchitectures
based on the [8051 (MCS-51)][8051] instruction set. These tables can be used
for compiler optimization, predicting code performance, comparing how
different 8051 microarchitectures might perform when executing the same code,
identifying the specific microarchitecture being used in a particular 8051
microcontroller, and more.


## Organization of this Repository

Cycle timing tables are named based on the following format:

```
cycles_NAME_{measured,published}.csv
```

Where `NAME` is the name of the microcontroller or microarchitecture the table
contains cycle timing information for, and the last part of the filename
indicates whether the results were measured from a real chip or were derived
from published specifications.

This repository also contains a mapping between instruction mnemonics and
their instruction byte, the corresponding byte mask, and the number of bytes
used by that instruction (including the instruction byte). This mapping can be
found in [`opcode_map.csv`][opcode-map].


## Format of the Tables


### Cycle Times

The format of the cycle time tables is very simple. The format is CSV, using
commas as column delimiters and newlines as row delimiters. The first line is
the header and contains the column names, `opcode` and `cycles`. Each of the
following lines contains the masked instruction byte (or "opcode") of a
particular instruction in the left column, and the number of clock cycles
required to execute that instruction in the right column.

Not all valid instructions may be present in a table. For example, as the
`RETI` instruction can be difficult to measure the timing of accurately, it is
not present in any of the tables of measured cycle times in this repository.


### Opcode Map

Like the cycle time tables, the [opcode map][opcode-map] is also a CSV with
commas to delimit columns and newlines to delimit rows. Unlike the opcode map,
however, the header row contains the following column names: `mnemonic`,
`opcode`, `mask`, and `bytes`. The `mnemonic` is a text string (sometimes
quoted) containing the name of the instruction and its arguments. Like the
cycle time tables, the `opcode` is the masked instruction byte (or "opcode")
of the instruction described by the `mnemonic`. The `mask` is the bit mask
used along with the `opcode` to uniquely identify an instruction, since some
instructions (like `ACALL`, `AJMP`, `DEC Rn`, or `MOV @Ri,#data`) store some
or all of their arguments in some of the bits in the instruction byte.
Finally, the `bytes` column contains the count of the total number of bytes
occupied by that instruction. This count includes the instruction byte, so it
is always greater than or equal to 1.


## License

Except where indicated otherwise, the contents of this repository are licensed
under the [Creative Commons Zero Public Domain Dedication][cc0].


[8051]: https://en.wikipedia.org/wiki/Intel_MCS-51
[opcode-map]: opcode_map.csv
[cc0]: https://creativecommons.org/publicdomain/zero/1.0/
