---
title: "RISC-V CPU Design"
excerpt: "Fully pipelined RISC-V CPU design (ALU, RegFile, Datapath, Control) in logic gate simulator <br/><img src='/images/datapath_fpic.jpg'>"
collection: portfolio
---
As the capping project of the Computer Architecture class at UC Berkeley, I designed and built a fully functioning 32-bit CPU implementing the RISC-V Instruction Set Architecture using a logic gate simulation software package called [Logisim](http://www.cburch.com/logisim/). The Reduced Instruction Set Computing paradigm was invented at UC Berkeley by Dave Patterson and John Hennessey, won a Turing award in 2017, and is now used in over 99% of all new chips [^fn1]. Besides this project, the Turing Award Colloquium special lecture given by Prof. Patterson was a real highlight of the course. 

The CPU was designed from scratch. First we created a Register File from flip-flop circuits and an Arithmetic Logic Unit from basic logic and arithmetic circuits. After this the 5 stages in the datapath were implemented using the ALU and Register File, and the control circuits were designed. These 5 phases in every computer instruction are: 

1. Instruction Fetch - where the next instruction to be executed is fetched from memory
2. Instruction Decode - where the instruction is decoded by the control logic so the proper control signals can be given
3. Execution - where the ALU executes the instruction
4. Memory Access - where load and store instructions are able to access memory
5. Register Write Back - where results are written back to the the registers in the CPU

The CPU we built was pipelined to improve clock frequency, and can run any single-threaded (C) program. 

![CPU pic](/images/datapath.jpg) 

[^fn1]: https://www.nytimes.com/2018/03/21/technology/computer-chips-turing-award.html 
