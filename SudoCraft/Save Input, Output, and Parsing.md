# SaveIO Class
This is the top class that will implement the basic stuff for reading and parsing the binary data structures.

## Requirements
- Skip between sets
- Basic reading and writing
- Delete set
- Replace set
- Insert set
- Read whole file into buffer
- Get header data

## Implementation

If the programmer chooses to leave the file open and read it in pieces then we should return a char array of each section/set that he or she may request.

If the programmer chooses to read all of the file contents into a buffer then we should create a new File class that should hold the file contents.