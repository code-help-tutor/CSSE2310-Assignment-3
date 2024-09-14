# CSSE2310 – Semester 2, 2024 Assignment 3

**Marks**: 75
**Weighting**: 15%
**Due**: 3:00pm Friday 4 October, 2024

## Introduction
The goal of this assignment is to demonstrate skills in fundamental process management and communication concepts (pipes and signals), and to further develop C programming skills with a moderately complex program. The program to be created, called `uqzip`, allows users to compress files using external compression programs and create `.uqz` archive files. It also allows decompression of these files.

## Student Conduct
This is an individual assignment. Discussion of general C programming and assignment specification is allowed, but active help with design and coding is prohibited. Plagiarism and collusion will result in misconduct actions. Code usage and referencing rules are provided, including conditions for using code from various sources. Sharing the assignment specification is also considered misconduct.

## Specification
- **Command Line Arguments**: `uqzip` accepts specific command line arguments for compression and decompression.
    - Compression: `./uqzip [--parallel] [--xz|--gzip|--bz|--none|--zip] [--output outFilename ] filename...`
    - Decompression: `./uqzip [--parallel] --extract archive-file`
- **File Checking**: Checks for file existence and ability to open files for compression and decompression.
- **Program Behaviour – Compression (Archive Creation)**:
    - **Sequential Compression**: Compresses files one by one using child processes.
    - **Parallel Compression**: Starts all child compression processes before checking results.
- **Program Behaviour – Decompression (Archive Extraction)**:
    - **Sequential Decompression**: Extracts files one by one using child processes.
    - **Parallel Decompression**: Creates two child processes per file for parallel extraction.
- **Interrupting uqzip**: Handles SIGINT signal differently in sequential and parallel modes.
- **Other Requirements**: Meets various requirements such as memory management, file descriptor handling, and not using certain functions.

## Provided Library
A library with functions like `read_uqz_header_section` and `free_uqz_header_section` is provided. To use it, include appropriate headers and link with the library.

## Style
Must follow version 3 of the CSSE2310/CSSE7231 C programming style guide. A single global variable of type `bool` may be used for signal handling. Other use of global variables will be penalized.

## Hints
- A demo program is available for reference.
- Various functions and techniques are suggested for implementation.
- Review assignment one sample solution and week 6 and 7 lectures and Ed Lessons.

## Suggested Approach
- Check command line arguments.
- Implement compression functionality.
- Implement sequential and parallel compression.
- Add code for execution failure handling.
- Implement sequential and parallel decompression.
- Implement remaining functionality.

## Forbidden Functions and Statements
Using certain C statements, directives, and functions will result in zero marks.

## Testing
Responsibility is on the student to test the program. A demonstration program and a test script will be provided. Gradescope submission site will be available.

## Submission
Must include all source and required files (Makefile). Program must build on moss and in the Gradescope environment. Submission must be committed to the Subversion repository and submitted via Blackboard.

## Marks
- **Functionality (60 marks for CSSE2310)**: Based on correct implementation of various features.
- **Style Marking**: Based on style guide violations.
    - **Automated Style Marking (5 marks)**: Calculated based on compilation warnings and style violations.
    - **Human Style Marking (5 marks)**: Based on comments, naming, and modularity.
- **SVN Commit History Marking (5 marks)**: Based on appropriate use and frequency of commits and meaningful log messages.
- **Total Mark**: Calculated based on functionality, style, and SVN commit history marks, with a scaling factor determined after interviews if applicable.

## Late Penalties
Late penalties will apply as per the course profile.

## CSSE2310 Student Interviews
Interviews may be conducted to establish genuine authorship. Failure to attend or inability to explain code may result in mark reduction or misconduct investigation.


# The University of Queensland
School of Electrical Engineering and Computer Science
CSSE2310 – Semester 2, 2024
Assignment 3 (Version 1.0)

**Marks**: 75
**Weighting**: 15%
**Due**: 3:00pm Friday 4 October, 2024

## Introduction
The goal of this assignment is to demonstrate your skills and ability in fundamental process management and communication concepts (pipes and signals), and to further develop your C programming skills with a moderately complex program. You are to create a program (called uqzip) which allows users to compress a file (or set of files) using any one of a set of external compression programs (zip, gzip, xz or bzip2). This will create a.uqz archive file. The program will also allow decompression of the files that it creates. The assignment will also test your ability to code to a particular programming style guide, and to use a revision control system appropriately.

## Student Conduct
This section is unchanged from assignment one – but you should remind yourself of the referencing requirements.
This is an individual assignment. You should feel free to discuss general aspects of C programming and the assignment specification with fellow students, including on the discussion forum. In general, questions like “How should the program behave if 〈this happens〉?” would be safe, if they are seeking clarification on the specification.
You must not actively help (or seek help from) other students or other people with the actual design, structure and/or coding of your assignment solution. It is cheating to look at another person’s assignment code and it is cheating to allow your code to be seen or shared in printed or electronic form by others.
All submitted code will be subject to automated checks for plagiarism and collusion. If we detect plagiarism or collusion, formal misconduct actions will be initiated against you, and those you cheated with. That’s right, if you share your code with a friend, even inadvertently, then both of you are in trouble. Do not post your code to a public place such as the course discussion forum or a public code repository. (Code in private posts to the discussion forum is permitted.) You must assume that some students in the course may have very long extensions so do not post your code to any public repository until at least three months after the result release date for the course (or check with the course coordinator if you wish to post it sooner). Do not allow others to access your computer – you must keep your code secure. Never leave your work unattended.
You must follow the following code usage and referencing rules for all code committed to your SVN repository (not just the version that you submit):
- **Code Origin**: Code provided by teaching staff this semester
    - **Code provided to you in writing this semester by CSSE2310 teaching staff (e.g., code hosted on Blackboard, found in /local/courses/csse2310/resources on moss, posted on the discussion forum by teaching staff, provided in Ed Lessons, or shown in class).**
    - **Permitted**: May be used freely without reference. (You must be able to point to the source if queried about it – so you may find it easier to reference the code.)
- **Code Origin**: Code you wrote this semester for this course
    - **Code you have personally written this semester for CSSE2310 (e.g. code written for A1 reused in A3) – provided you have not shared or published it.**
    - **Permitted**: May be used freely without reference. (This assumes that no reference was required for the original use.)
- **Code Origin**: Unpublished code you wrote earlier
    - **Code you have personally written in a previous enrolment in this course or in another UQ course or for other reasons and where that code has not been shared with any other person or published in any way.**
    - **Conditions apply, references required**: May be used provided you understand the code AND the source of the code is referenced in a comment adjacent to that code (in the required format – see the style guide). If such code is used without appropriate referencing then this will be considered misconduct.
- **Code Origin**: Code from man pages on moss
    - **Code examples found in man pages on moss. (This does not apply to code from man pages found on other systems or websites unless that code is also in the moss man page.)**
    - **Conditions apply, references required**: May be used provided you understand the code AND the source of the code or learning is referenced in a comment adjacent to that code (in the required format – see the style guide) AND an ASCII text file (named toolHistory.txt) is included in your repository and with your submission that describes in detail how the tool was used. (All of your interactions with the tool must be captured.) The file must be committed to the repository at the same time as any code derived from such a tool. If such code is used without appropriate referencing and without inclusion of the toolHistory.txt file then this will be considered misconduct. See the detailed AI tool use documentation requirements on Blackboard – this tells you what must be in the toolHistory.txt file.
- **Code Origin**: Code and learning from AI tools
    - **Code written by, modified by, debugged by, explained by, obtained from, or based on the output of, an artificial intelligence tool or other code generation tool that you alone personally have interacted with, without the assistance of another person. This includes code you wrote yourself but then modified or debugged because of your interaction with such a tool. It also includes code you wrote where you learned about the concepts or library functions etc. because of your interaction with such a tool. It also includes where comments are written by such a tool – comments are part of your code.**
    - **Conditions apply, references & documentation req’d**: May be used provided you understand the code AND the source of the code or learning is referenced in a comment adjacent to that code (in the required format – see the style guide) AND an ASCII text file (named toolHistory.txt) is included in your repository and with your submission that describes in detail how the tool was used. (All of your interactions with the tool must be captured.) The file must be committed to the repository at the same time as any code derived from such a tool. If such code is used without appropriate referencing and without inclusion of the toolHistory.txt file then this will be considered misconduct.
- **Code Origin**: Code copied from sources not mentioned above
    - **Code, in any programming language:
• copied from any website or forum (including Stack-Overflow and CSDN);
• copied from any public or private repositories;
• copied from textbooks, publications, videos, apps;
• copied from code provided by teaching staff only in a previous offering of this course (e.g. previous assignment one solution);
• written by or partially written by someone else or written with the assistance of someone else (other than a teaching staff member);
• written by an AI tool that you did not personally and solely interact with;
• written by you and available to other students; or
• from any other source besides those mentioned in earlier table rows above.**
    - **Prohibited**: May not be used. If the source of the code is referenced adjacent to the code then this will be considered code without academic merit (not misconduct) and will be removed from your assignment prior to marking (which may cause compilation to fail and zero marks to be awarded). Copied code without adjacent referencing will be considered misconduct and action will be taken.
This prohibition includes code written in other programming languages that has been converted to C.
- **Code Origin**: Code that you have learned from
    - **Examples, websites, discussions, videos, code (in any programming language), etc. that you have learned from or that you have taken inspiration from or based any part of your code on but have not copied or just converted from another programming language. This includes learning about the existence of and behaviour of library functions and system calls that are not covered in class.**
    - **Conditions apply, references required**: May be used provided you do not directly copy code AND you understand the code AND the source of the code or inspiration or learning is referenced in a comment adjacent to that code (in the required format – see the style guide). If such code is used without appropriate referencing then this will be considered misconduct.

You must not share this assignment specification with any person (other than course staff), organisation, website, etc. Uploading or otherwise providing the assignment specification or part of it to a third party including online tutorial and contract cheating websites is considered misconduct. The university is aware of many of these sites and many cooperate with us in misconduct investigations. You are permitted to post small extracts of this document to the course Ed Discussion forum for the purposes of seeking or providing clarification on this specification.
In short – Don’t risk it! If you’re having trouble, seek help early from a member of the teaching staff.
Don’t be tempted to copy another student’s code or to use an online cheating service. Don’t help another CSSE2310/7231 student with their code no matter how desperate they may be and no matter how close your relationship. You should read and understand the statements on student misconduct in the course profile and on the school website: https://eecs.uq.edu.au/current-students/guidelines-and-policies-students/student-conduct.

## Specification
The uqzip program will allow a user to create a.uqz archive file that will hold a compressed file (or files) that can later be extracted from the archive (decompressed) using uqzip. uqzip won’t actually do the compression itself – it will rely on external programs (such as zip, gzip, xz or bzip2 available on moss) to do the compression and will package up the resulting data into a.uqz archive. The.uqz archive file will also store the method of compression used and the filename for each compressed file stored in the archive.
When used for decompression, uqzip will extract each compressed data stream from the.uqz archive file and pipe it to an external decompression program whose output will be saved to a file in the current directory with the original filename.
uqzip is to support both sequential and parallel compression and decompression. Sequential processing means files are compressed (or decompressed) one after the other. Parallel processing means files are compressed (or decompressed) in parallel – each using a separate child process of uqzip (or two child processes per file in the case of parallel decompression).

### Command Line Arguments
Your uqzip program is to accept command line arguments as follows when doing file compression, i.e. creating a.uqz archive file:
`./uqzip [--parallel] [--xz|--gzip|--bz|--none|--zip] [--output outFilename ] filename...`
and as follows when doing decompression (archive extraction):
`./uqzip [--parallel] --extract archive-file`
The square brackets ([]) indicate optional arguments or groups of arguments. The pipe symbol (|) indicates a choice. The italics indicate placeholders for user-supplied value arguments. An ellipsis (...) indicates the previous argument can be repeated. Note that the option arguments (those beginning with “--” and their associated value, if any) can be in any order.
When used for compression, uqzip will accept an optional argument specifying the compression method. When none of --bz, --gzip, --zip, --xz or --none are specified then the program is to act as if --none is specified – i.e. no compression is used when creating the archive file.
When used for compression, an optional output file name (outFilename ) can be specified by using the --output option argument. If this argument pair is not specified, then the default filename out.uqz is to be used. It can be assumed that (i.e. we will not test a situation when) the outFilename (or default output filename) is given as a filename argument on the command line.
When used for compression, one or more existing filenames to be compressed into the.uqz archive must also be specified after any option arguments. It can be assumed that the first (or only) file name argument does not start with the characters “--”. (If the user wants to specify a file whose name does start with “--” then they should prefix the name with “./”. Any arguments after the first file name are also assumed to be file names, even if they begin with “--”.)
When used for decompression (with the --extract option argument specified), uqzip expects a filename argument to be present (archive-file, which must be the last argument). This is the name of the.uqz archive file to be decompressed.
If the --parallel option argument is present, then uqzip is to use child processes running in parallel to do the compression or decompression. If this argument is not present, then uqzip will use child processes running sequentially. Full details of the required functionality are provided later in this document.
Some examples of how the program might be run include the following:
- `./uqzip /usr/dict/share/words`
- `./uqzip --parallel --bz one.txt two.txt three.txt`
- `./uqzip --zip --output lists.uqz --parallel list1 list2 list3 list4 list5 list6`
- `./uqzip --output archive --xz /usr/share/dict/words local/dictionary`
- `./uqzip --extract archive.abc`
- `./uqzip --parallel --extract out.uqz`
More details on the expected behaviour of the program are provided later in this document.

### Prior to doing anything else, your program must check the command line arguments for validity. If the program receives an invalid command line then it must print the following (two line) message:
```
Usage:./uqzip [--parallel] [--xz|--gzip|--bz|--none|--zip] [--output outFilename] filename...
Or:./uqzip [--parallel] --extract archive-file
```
to standard error (with a newline at the end of each line), and exit with an exit status of 20. Note that the colon (:) symbols on each line are aligned, i.e. three spaces precede “Or:”.
Invalid command lines include (but may not be limited to) any of the following:
- More than one of the option arguments --bz, --gzip, --zip, --xz, --none or --extract is given on the command line
- Any of the option arguments is listed more than once.
- The --output option argument is given but it is not followed by a filename argument
- No filenames are given on the command line.
- --extract is given on the command line along with --output
- --extract is given on the command line along with more than one filename
- An unexpected argument is present.
- Any argument is the empty string.
Checking whether files exist or can be opened is not part of the usage checking (other than checking that filename values are not empty). This is checked after command line validity as described below.

### File Checking
#### Compression
If uqzip is creating an archive file (i.e. --extract is NOT specified on the command line) then it must attempt to open the output file (the given outFilename from the command line, or, if none is present, then the default output filename – out.uqz) for writing. If the file already exists, it must be truncated and overwitten. If the file is unable to be opened for writing then uqzip must print the message:
```
uqzip: can’t write to file "filename "
```
to standard error (with a following newline) where filename is replaced by the name of the file that could not be opened for writing. uqzip must then exit with status 8.
The files to be compressed are not checked for existence.

#### Decompression
If --extract is specified on the command line and uqzip is unable to open the given archive filename (archive-file argument) for reading, then your program must print the message:
```
uqzip: unable to read from file "filename "
```
to standard error (with a following newline) where filename is replaced by the name of the.uqz archive file from the command line. The double quotes must be present. uqzip must then exit with status 18.

## Program Behaviour – Compression (Archive Creation)
If the command line and file checks described above are successful and an archive file is to be created (the argument --extract is NOT specified on the command line) then uqzip is to behave as described below.
First, uqzip must write out the header section for the archive file. (See Table 1 for details of the file format, including the header section.) Placeholders should initially be used for the file record offsets because these aren’t known yet. These will need to updated in the file after the compressed files are added to the archive.

### Sequential Compression
Individual files specified on the uqzip command line are to be compressed (in the order given on the command line) using a separate child process for each running the compression command shown in Table 2. (Programs are to be found on the user’s PATH.) The output of each command must be piped back to the parent (uqzip) and uqzip must add a complete file record to the archive file. (See Table 1 for details of the file record format.)
The filename within the record must be only the “basename” of the given filename – i.e. the name excluding any directory path. In other words, if a ‘/’ character is present in the supplied filename then only that part of the name after the last ‘/’ is to be saved in the archive file. For example, if the filename /etc/motd is given on the command line, then it will be saved in the archive using the filename motd. When EOF is detected when reading from the pipe, the child process is to be reaped.
If a compression program is unable to be executed (e.g. not found on the user’s PATH) then the child process that attempted the exec must send a SIGUSR1 signal to itself to terminate itself. (By default this signal is not caught and causes process termination). If the parent (uqzip) detects that a child has died due to SIGUSR1 then it must print the following message to stderr (with a trailing newline):
```
uqzip: Unable to execute "command "
```
where command is replaced by the name of the command whose execution failed (e.g. “gzip” – no arguments are included). The double quotes must be present. uqzip must then exit with status 6.
If a child compression process fails for any other reason (i.e. does not exit normally with a zero exit status), then your program must print the following message to stderr (with a trailing newline):
```
uqzip: "command " command failed for filename "filename "
```
where command is replaced by the name of the command being executed (e


# CSSE2310 Assignment 3

# CS Tutor | 计算机编程辅导 | Code Help | Programming Help

# WeChat: cstutorcs

# Email: tutorcs@163.com

# QQ: 749389476

# 非中介, 直接联系程序员本人
