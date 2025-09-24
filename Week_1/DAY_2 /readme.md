# INTRODUCTION TO timing.libs
## library formate
Liberty format is an industry-standard file format used to describe library cells of a technology.
A cell can be a standard cell, I/O buffer, or a complex IP block.
Each cell description includes:
Timing information
Power estimation
Area
Functionality
Operating conditions
Technically, it represents the timing and power properties of black boxes (modules whose internal details are hidden).
It is an ASCII-based format, typically stored in files with a .lib extension.
The contents of a Liberty file can be divided into two parts:
Common information shared by all standard cells
Cell-specific information (e.g., timing, power, functionality)

