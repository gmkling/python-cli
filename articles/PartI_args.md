Writing Python programs that act like UNIX-style programs is a good thing if you or your users work at the command line. With some slight differences, it is possible to do this thanks to some great packages that have been created for just this purpose. 

Traditional UNIX-style programs do not exist in a vacuum, instead they work in an ecosystem of supporting tools. They use the *shell* properly: `stdout` and `stderr` get the appropriate info, output is pipe-able to other programs, and users can activate and configure features of the program by passing arguments. Files are read and written, and logs are kept as a trace of a program's activity. To get to an end product that satisfies the high expectations of the SysAdmins and their colleagues everywhere takes a good deal, of work, so the best idea is to build up tooling feature by feature, revising and extending as you better understand the user story behind your tools.   

A good place to start is with user interaction at the command line - using flags and arguments to control the behavior of a program. Luckily, the [argparse](https://docs.python.org/3/library/argparse.html) package is here to help. For example, when we issue a command with  `--help` or `-h`, we expect to see the program print documentation that explains what options, flags, and parameters are available. Argparse makes this easy by making providing this help text part of declaring flags and parameters for the program, rather than hand building this help printout as a C function as with traditional UNIX programs:

```python
from argparse import *

parser = argparse.ArgumentParser(description='This is an excellent program')
```
