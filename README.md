# project
Final Project

Due December 7 but extensions into exam week will be accepted if most of the work has been completed and committed to your Github repository.

The purpose is to create a simulator for a very simple computer with a GUI to show the computer executing a program (including calculating factorial and doing sorts). Our project was originally inspired by a project called Pippin--from pp.210-214 of "The Analytical Engine, An Introduction to Computer Science Using the Internet" by Rick Decker and Stuart Hirshfield (B Publishing Company, 1998). However, many extensions have been made.

The computer has a Model that describes the CPU, the Instruction type that the CPU can execute. There are also Memories for code and for data, which the Model references. There will be a GUI and as the computer executes a series of instructions, the GUI changes its contents.

Create two packages `project` and `projectview`. Make a class `Data` with a `public static constant int DATA_SIZE` set to 2048 (that may be changed before the project is complete). The private fields are an `int[]` array called `data` of length `DATA_SIZE` and an int classed `changedIndex`, initially -1. We need package private methods: a getter method for this array for JUnit testing, `getData(int index)` and `setData(int index, int value)` to read and write values from and to this array. In these method throw `MemoryAccessException` if index is negative to too large. Here is the unchecked Exception class `MemoryAccessException` with the message _"Illegal access to data memory, index " + index_ -- see the files above. Also provide a getter method for `changedIndex`. In the method `setData(int index, int value)`, assign `changedIndex` to `index`. The `changedIndex` will be used by the GUI to colorize the location that is changed.

Make a class `Model`. A lot goes on in this class, which will be build up in steps. In `Model`, make a nested class

```java
  static class CPU {
    ...
  }
```
The private `int` fields are `accumulator`, `instructionPointer`, `memoryBase`.

Also in `Model` put the _enum_ 

```java
	static enum Mode {
		INDIRECT, DIRECT, IMMEDIATE;
		Mode next() {
			if (this==DIRECT) return IMMEDIATE;
			if (this==INDIRECT) return DIRECT;
			return null;
		}
	}
```

Also in `Model` put the interface `Instruction` that declares the method `void execute(int arg, Mode mode)` and an interface `HaltCallBack` that declares the method `void halt()`.

For now, give Model the _private_ fields `final Instruction[] INSTR` an array of length 15, `CPU cpu = new CPU()` and `Data dataMemory = new Data()`, `HaltCallBack callback`.

A LOT of work goes into the main constructor `public Model(HaltCallBack cb)`. 

The first line is `callBack = cb;` Then put the comment `//Job initialization goes here later`. Next we make all the instructions of our computer. Since Interface is a _functional_ interface, we can use lambda expressions. This time they are recursive.

The instructions are ADD, AND, CMPL, CMPZ, DIV, HALT, JMPZ, JUMP, LOD, MUL, NOP, NOT, STO, SUB.

* HALT, NOT, NOP take no argument (although we pass 0 to `execute`) and the `Mode` should be null, since it is ignored.
* CMPL, CMPZ use the argument as a dataMemory address, which is the DIRECT Mode and it is the value stored in dataMemory that is used in the instruction.
* AND only uses IMMEDIATE and DIRECT Modes. The IMMEDIATE Mode is the one where the argument is the actual value used in the instruction.
* STO only uses DIRECT and INDIRECT Modes. The INDIRECT Mode uses the argument as a dataMemory address _but_ the value at that address is then used as the dataMemory address for the instruction itself. STO is a mnemonic for "Store in memory" and it sets the dataMemory at the index in the instruction to the curretn value in the accumulator of the CPU
* JUMP and JMPZ are jump instructions that change the `instructionPointer` and use the 3 Modes in slightly different ways as explained below.
* The other 5 inststructions use all 3 modes as we will describe below when discussing ADD.

