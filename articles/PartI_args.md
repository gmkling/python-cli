Writing Python programs that act like UNIX-style programs is a good thing if you or your users work at the command line. With some slight differences, it is possible to do this thanks to some great packages that have been created for just this purpose. 

Traditional UNIX-style programs do not exist in a vacuum, instead they work in an ecosystem of supporting tools. They use the *shell* properly: `stdout` and `stderr` get the appropriate info, output is pipe-able to other programs, and users can activate and configure features of the program by passing arguments. Files are read and written, and logs are kept as a trace of a program's activity. To get to an end product that satisfies the high expectations of the SysAdmins and their colleagues everywhere takes a good deal, of work, so the best idea is to build up tooling feature by feature, revising, extending, and trimming back as you better understand the use cases addressed by your tools.   

A good place to start is with user interaction at the command line - using flags and arguments to control the behavior of a program. Luckily, the [argparse](https://docs.python.org/3/library/argparse.html) package is here to help. There are many batteries included in this package: adding switches/flags, positional arguments, subcommands, help, and more are straightforward. For example, when we issue a command with  `-h` or `--help`, we expect to see the program print documentation that explains what options, flags, and parameters are available. Argparse makes this easy by making this help text part of declaration of the respective flags and parameters for the program:

```python
# Top-Level parser with description
from argparse import *

parser = argparse.ArgumentParser(description='This is an excellent program')
```

```python
# Adding boolean switches and flags with arguments
parser.add_argument('-q', '--quiet', action='store_true', help='Supress output')
parser.add_argument('-f', '--fname', metavar='Filename', type=str, nargs=1,
	help='Name of the file to output. If this is not present, print to stdout')
```

```python
# Subparsers for subcommands
subparsers = parser.add_subparsers(help="Subcommands Available:")
exponentialParse = subparsers.add_parser('exponential', help='generate numbers from the exponential distribution')
exponentialParse.add_argument('-N', '--NValues', type=int, nargs=1)
exponentialParse.add_argument('-s', '--scale', type=float, default=1.0, nargs=1)
exponentialParse.set_defaults(func=generateExponential)
```