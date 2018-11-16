# project
Final Project

Due December 7 but extensions into exam week will be accepted if most of the work has been completed and committed to your Github repository.

The purpose is to create a simulator for a very simple computer with a GUI to show the computer executing a program (including calculating factorial and doing sorts). Our project was originally inspired by a project called Pippin--from pp.210-214 of "The Analytical Engine, An Introduction to Computer Science Using the Internet" by Rick Decker and Stuart Hirshfield (B Publishing Company, 1998). However, many extensions have been made.

The computer has a Model that describes the CPU, the Instruction type that the CPU can execute. There are also Memories for code and for data, which the Model references. There will be a GUI and as the computer executes a series of instructions, the GUI changes its contents.

Create two packages `project` and `projectview`. Make a class `Data` with a `public static constant int DATA_SIZE` set to 2048 (that may be changed before the project is complete). The private fields are an `int[]` array called `data` of length `DATA_SIZE` and an int classed `changedIndex`, initially -1. We need package private methods: a getter method for this array for JUnit testing, `getData(int index)` and `setData(int index, int value)` to read and write values from and to this array. In these method throw `MemoryAccessException` if index is negative to too large. Here is the unchecked Exception class `MemoryAccessException` with the message _"Illegal access to data memory, index " + index_ -- see the files above. Also provide a getter method for `changedIndex`. In the method `setData(int index, int value)`, assign `changedIndex` to `index`. The `changedIndex` will be used by the GUI to colorize the location that is changed.




