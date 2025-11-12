<!---
{
  "id": "f46a35bf-69f3-4385-ab0c-b23074589a35",
  "teaches": "Using `argparse` in Python Applications",
  "depends_on": ["7130a694-458e-4e24-80b7-d8673f765e69"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-05-13",
  "keywords": ["argparse", "python", "cli", "arguments", "--version", "-h"]
}
--->

# Using `argparse` in Python Applications

> In this exercise you will learn how to use the `argparse` module to parse command-line arguments in Python. Furthermore we will explore how to structure CLI tools that follow best practices, such as including `--version` and `-h` for help.

### Introduction

Command-line interfaces (CLIs) are essential for many applications, especially in automation and scripting contexts. Python's built-in `argparse` module provides a robust and user-friendly framework to parse command-line arguments. Since `argparse` is part of Python's standard library, it is available by default in any standard Python installation (version 3.2 and above), requiring no additional packages or installations. This makes it an ideal choice for building reliable and portable CLI tools.

Using `argparse`, developers can define what arguments their program expects, handle different types of arguments, generate usage help messages, and control tool metadata such as versioning. One of the key conventions in CLI design is the inclusion of two specific options: `-h` or `--help`, which outputs a helpful summary of how to use the tool, and `--version`, which returns the version of the script or application. These options enhance usability and maintainability of scripts and are considered best practice in any serious CLI development.

This sheet walks you through three different tasks that build on each other. The first demonstrates basic usage, the second shows how to use mutually exclusive arguments, and the third integrates sub-commands for more complex CLIs. Throughout, we emphasize the inclusion of `--version` and `-h`.

### Further Readings and Other Sources

* [Python `argparse` Documentation](https://docs.python.org/3/library/argparse.html)
* [YouTube: Python `argparse` Crash Course](https://www.youtube.com/watch?v=cdblJqEUDNo)
* [Real Python on `argparse`](https://realpython.com/command-line-interfaces-python-argparse/)

---

## Task 1: Basic Argument Parsing with `--version` and `-h`

Create a script `basic_cli.py` that accepts a required filename and supports the `--version` flag.

```python
import argparse

parser = argparse.ArgumentParser(description='Basic CLI tool with version info.')
parser.add_argument('filename', help='File to process')
parser.add_argument('--version', action='version', version='basic_cli 1.0', help='Show version and exit')
args = parser.parse_args()

print(f"Processing file: {args.filename}")
```

Run this script using:

```
python basic_cli.py example.txt
python basic_cli.py --version
```

## Task 2: Mutually Exclusive Arguments

Extend your CLI to use mutually exclusive options: `--compress` and `--decompress`, while retaining `--version` and `-h`. Save this as `exclusive_cli.py`.

```python
import argparse

parser = argparse.ArgumentParser(description='CLI with mutually exclusive options.')
parser.add_argument('filename', help='File to process')
parser.add_argument('--version', action='version', version='exclusive_cli 1.0', help='Show version and exit')

group = parser.add_mutually_exclusive_group(required=True)
group.add_argument('--compress', action='store_true', help='Compress the file')
group.add_argument('--decompress', action='store_true', help='Decompress the file')

args = parser.parse_args()

if args.compress:
    print(f"Compressing file: {args.filename}")
else:
    print(f"Decompressing file: {args.filename}")
```

Run this script with:

```
python exclusive_cli.py example.txt --compress
python exclusive_cli.py --version
```

## Task 3: Subcommands with `argparse`

Implement a more structured CLI with subcommands using `argparse`. Save the file as `subcommands_cli.py`. It should include a `greet` and a `farewell` subcommand.

```python
import argparse

parser = argparse.ArgumentParser(description='CLI with subcommands')
parser.add_argument('--version', action='version', version='subcommands_cli 1.0', help='Show version and exit')
subparsers = parser.add_subparsers(dest='command', required=True)

# Greet subcommand
greet_parser = subparsers.add_parser('greet', help='Greet someone')
greet_parser.add_argument('name', help='Name of the person to greet')

# Farewell subcommand
farewell_parser = subparsers.add_parser('farewell', help='Say goodbye to someone')
farewell_parser.add_argument('name', help='Name of the person to say goodbye to')

args = parser.parse_args()

if args.command == 'greet':
    print(f"Hello, {args.name}!")
elif args.command == 'farewell':
    print(f"Goodbye, {args.name}!")
```

Run the script like this:

```
python subcommands_cli.py greet Alice
python subcommands_cli.py --version
```

---

## Advice

As you work through these tasks, make sure to test your scripts thoroughly with different combinations of arguments. Pay close attention to how `argparse` handles help text generation and errors — understanding this will save you a lot of time in larger projects. Including `--version` and `-h` is not just good practice but a requirement for professional tools. Take your time to explore how `argparse` structures its help output, and consider reading its documentation in full. Since `argparse` is a standard module, using it helps ensure compatibility and minimizes external dependencies. This foundation will be invaluable when you move on to more complex CLI interfaces or frameworks like `click` or `typer`, which build on similar principles.
