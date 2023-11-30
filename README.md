# advent-of-code-cpp-starter
This is a simple C++ runner for doing Advent-of-Code challenges. It's designed to have one executable which is given parameters for the day and part, and then it will run that particular solution.

It's designed for C++ under Linux. I've tested it with CentOS 8 and GCC version 8.3.1.

## Building the starter:
After checking out the project, it's just 2 commands to build

    ./mkdirs.sh
    make

`mkdirs.sh` will create the bin and build directories and their subdirectories. It only needs to be run the first time.
## Creating a new Solution

To start working on the solution for Day 1, you can follow these steps:

Create Files:

Create a new pair of files for the day, let's say aoc_day_1.h and aoc_day_1.cpp. Place them in the include/solutions directory.
Inherit from AocDay:

Open aoc_day_1.h and have it inherit from the AocDay class. It should look something like this:
```cpp
#pragma once
#include "solutions/aoc_day.h"

class AocDay1 : public AocDay {
public:
    AocDay1() : AocDay(1, "Day 1") {}
    string part1(string filename, vector<string> extra_args) override;
    string part2(string filename, vector<string> extra_args) override;
};
```
Implement the Solutions:

Open aoc_day_1.cpp and implement the solutions for part 1 and part 2. It should look something like this:
```cpp
#include "solutions/aoc_day_1.h"
#include "common/file_utils.h"

string AocDay1::part1(string filename, vector<string> extra_args) {
    // Your code for part 1 goes here
    // Use file reading functions from FileUtils class if needed
    return "Part 1 solution";
}

string AocDay1::part2(string filename, vector<string> extra_args) {
    // Your code for part 2 goes here
    // Use file reading functions from FileUtils class if needed
    return "Part 2 solution";
}
```
Update Makefile:

Open the Makefile and add the .o file for your new day to the list of objects to be linked. Also, make sure the .o file is added to the libsolutions.a library. For example:
```make
DAY1_OBJ = build/solutions/aoc_day_1.o

libsolutions.a: $(DAY0_OBJ) $(DAY1_OBJ)
    ar rcs bin/libsolutions.a $(DAY0_OBJ) $(DAY1_OBJ)
```
Build the Project:

Run ./mkdirs.sh (if not done before, only needs to be run once).
Run make to build the project.
Run the Solution:

Execute your solution with the provided command-line parameters. For example:
```bash
bin/aoc -d 1 -p 1 -f path/to/your/input.txt
```
Now you're set to implement and test your solutions for Day 1.


### Working on a Day's Solution

The starter comes with a Day 0 implementation, which is based on Advent of Code 2018, day 1, part 1. The part 1 solution matches the problem, and the part 2 solution the same as part 1, except it gives the negation of the part 1 result for a solution. Looking at [`aoc_day_0.h`](include/solutions/aoc_day_0.h), [`/aoc_day_0.cpp`](src/solutions/aoc_day_0.cpp), and [`aoc_days.cpp`](src/solutions/aoc_days.cpp) will show all the coding needed to hook in a new day.

Each day's solution will inherit from the AocDay class (include/solutions/aoc_day.h) as is done in include/solutions/aoc_day_0.h .

There are two functions to override in the child class:

    virtual string part1(string filename, vector<string> extra_args);
    virtual string part2(string filename, vector<string> extra_args);

The base AocDay class has default implementations of these functions, so you don't even have to define a part2 function until after part1 is done, if you'd prefer.

Each function takes two parameters - the filename for the input file and a vector of extra arguments, which I'll describe below.

The return value is a string for the solution. *"Why a string and not an int/long?"* you might ask. Although Advent of code 2019 had only numeric solutions, I found that in 2018 there were times when the solution was non-numeric (day 7 part 1, day 13 parts 1 and 2). So, this function returns a string back to the driver program.

### Extra arguments
The parameter of `extra_args` is useful to prevent code changes for hard-coding limits or other constants. For example, let's say the test cases presented in the problem description show a result after 10 iterations, but then the actual answer is supposed to run 1000000 times. You can pass the `10` or `1000000` value as an extra arguent instead of having to change a constant and recompile. These are also passed in as strings, which can then be converted to ints/longs/whatever as needed.

### Makefile changes
I'll preface this by saying that I don't like dealing with Makefiles. I'm sure I could make some of this prettier, but it is what it is.

The Makefile for this compiles a couple of libraries - librunner.a for some of the basic test runner functionality, and libsolutions.a for the daily solutions. As things get more complex, I'm sure more libraries will get added. If you want to see what I mean, there's a more complex example in my intCode repo (https://github.com/bcooperstl/intCode). 

For each day's solution, the Makefile file will need to be modified to add in a .o file target that corresponds to the .cpp file for that day. That .o file must also be added to libsolutions.a . 

The final executable is built as bin/aoc

### Useful-ish Helper Functions
There are some helper functions in the `FileUtils` class to read in and parse an input file. 

`bool read_as_list_of_strings(string filename, vector<string> & lines)` - Read the file given by `filename` and return the lines of the file in the `lines` vector.  
`bool read_as_list_of_split_strings(string filename, vector<vector<string>> & split_strings, char delimiter, char quote_char, char comment_char)` - Read the file given by filename, return it as a list of list of strings in the `split_strings` vector. `delimiter` identfies a delimiter to identify how to split the strings. `quote_char` gives a way to allow the delimiter to appear in a string by quoting around it. `comment_char` allows for a line to be skipped from the output if it starts with this character. This probably sounds like overkill, but I use a bunch of this with my test file format. See [testing.md](testing.md) for an example and more details.  
`bool read_as_list_of_split_longs(string filename, vector<vector<long>> & split_longs, char delimiter, char quote_char, char comment_char)` - Same as `read_as_list_of_split_strings` but with longs. Think of it as an easy way to read in an intcode program from 2019, if you will.


## Running a solution
There are three modes to run this program as shown in the usage. This section describes how to run one input file through. The [testing.md](testing.md) file describes the two other modes.  
The command line is:

    bin/aoc -d day -p part -f filename [extra_args...]

Everything should be straight forward - give it the day, part (1 or 2), input file, and optionally any extra arguments. It'll spit out the result or tell you if there's an error.

For example:  

    [brian@dev1 advent-of-code-cpp-starter]$ bin/aoc -d 0 -p 1 -f data/sample/day0_input.txt
    ***Day 0 Part 1 for file data/sample/day0_input.txt has result 569
