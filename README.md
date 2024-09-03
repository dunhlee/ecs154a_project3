# Project 3 README

Student 1: Dunh Adam Lee (921476881)
Student 2: Weiyang Yu (919410927)

## Project Status

# Subcircuits implemented:

ADDER15, REGS15, ALU15, FLAGS8, IMM15, DECODE15, and CPU15

# Instructions implemented:

U-Type Instructions, R-type instructions, and most I-type instructions.

Testing rtype0.dat, itype.dat, and loading.dat returns the expected values after their operations.


## Known Issues
The project fails to function properly in several areas. 

First we have the instructions not implemented: All B-type instructions, and the S-Type instruction SW.

For instructions SLL, SRL, and SRA, the carry flags are not implemented correctly.

## ADDER15

For our adder, we decided to go with the intuitive approach of using 3 5-bit RCAs. After seeing Professor Nitta's post on Piazza about the correct CLA equations, we modified our adder accordingly. Using 3 5-bit CLAs means that we have 3 blocks. 

With this approach we are able to compute two 15-bit values.

## ALU15

For our ALU, we used a mux to select between the 8 operations with the value of OP. Operations ADD, SUB, AND, OR, XOR were straightforward to implement. 

For our shifters, we went with the book's way of implementing them. We used 15 15-1 muxes to implement SLL, SRL, and SRA.

## REGS15

In REGS15, we used a decoder to separate SeID into 8 possible values, 000 to 111. Data can only be written to a register depending on what is selected by SeID and when W = 1. To read from a register, we used muxes controlled by SeIA and SeIB to determine which register to read data into A or B.

## IMM15

For the immediate generator, we grouped up each instruction that uses immediate values into groups that use the same immediate value bit-to-instruction bit. After we mapped the immediate values to their instruction bits, we had to encode the the different possible immediate generators so that the mux would work as we wanted. Since there are 5 possible immediate values to choose from, are encoding used 3 bits to represent them all.

## DECODE15

For the decoder, it was straightforward although it took up a lot of time. We made kmaps for each of the outputs affected by the opcode using a mixture of POS and SOP when convenient.

## FLAGS8

For the Flags register, we ANDed the value of WM and NF to get the result to store in F.

## CPU15

We started with 15 T flip flops for a PC, but when we got to implementing jump instructions, we didn't know how to load a value into the flip flops, so we ended using a built-in counter in logisim.

The CPU has 2 states: Fetch/Decode and Execute/Write.

During the Fetch/Decode state, ADDR obtains the next instruction from the PC, IB is set to DATAIN, and the Opcode is decoded.

During Execute/Write, the information that is needed such as SeID, SeIA, SeIB is sent to REGS15. From there, the ALU computes two operands depending on which operation was used. The output of the calculation into Rd is determine by RegDataSRC which selects through a mux.

Specific for the LW and LUI instructions, the result of the ALU is sent to ADDR, so that we can get the data from memory.
Otherwise, the mux inputs the value of PC to ADDR during the fetching state, and an undefined value during the write state.


## References
http://www.cburch.com/logisim/docs/2.3.0/libs/mem/counter.html

