# ALX Simple Shell Team Project
<p align="center">
<img src="https://s3.amazonaws.com/intranet-projects-files/holbertonschool-low_level_programming/235/shell.jpeg" width="700" height="400" />
</p>

> This is an ALX collaboration project on Shell.
> We were tasked to create a simple shell that mimics the Bash shell.
> 
> Our shell shall be called `hsh`.
## Introduction
This repository is a ALX Holberton School Project. The school project consisted in writing a shell like `sh` (Bourne Shell) by Stephen Bourne , in `C`, using a limited number of standard library functions, so instead we used our own function that we rewrited over the past three month [Here.](https://github.com/TosinISOGUN/alx-low_level_programming)

The goal in this project is to make us understand how a shell works.
- To single out some core topics which includes;
  - What is the environment?
  - The difference between functions and system calls.
  - How to create processes using `execve...`

## General Requirements
<img src="https://github.com/TosinISOGUN/TosinISOGUN/blob/main/ALX.jpeg?raw=true" width="300" height="100" />

- `README file`, at the root of the folder of the project is mandatory.
- Allowed editors: `vi`, `vim`, `emacs`
- Compiler;
  - Ubuntu 20.04 LTS using gcc.
  - Using the options `-Wall -Werror -Wextra -pedantic -std=gnu89`
- Coding style;
  - Betty Style.
- Shell should not have any memory leaks.
- No more than 5 functions per file.
- All header files should be include guarded.

## How hsh works
- Prints a prompt and waits for a command from the user.
- Creates a child process in which the command is checked.
- Checks for built-ins, aliases in the PATH, and local executable programs.
- The child process is replaced by the command, which accepts arguments.
- When the command is done, the program returns to the parent process and prints the prompt.
- The program is ready to receive a new command.
- To exit: press `Ctrl-D` or enter "`exit`" (with or without a status).
- Works also in non interactive mode.

## Usage
In order to run this program,

Clone This Repo;

`git clone https://github.com/TosinISOGUN/simple_shell.git`

Compile it with;

`gcc 4.8.4 -Wall -Werror -Wextra -pedantic *.c -o hsh.`

You can then run it by invoking `./hsh` in that same directory.

How to use it
In order to use this shell, in a terminal, first run the program:
`prompt$ ./hsh`
It wil then display a simple prompt and wait for commands.
`$`
A command will be of the type `$` command

To invoke **hsh**, compile all .c files in the repository and run the resulting executable.

hsh can be invoked both interactively and non-interactively. If hsh is invoked with standard input not connected to a terminal, it reads and executes received commands in order.

Example:
```
$ echo "echo 'hello'" | ./hsh
'hello'
$
```
If hsh is invoked with standard input connected to a terminal (determined by isatty(3)), an interactive shell is opened. When executing interactively, hsh displays the prompt $ when it is ready to read a command.

Example:
```
$./hsh
$
```

Alternatively, if command line arguments are supplied upon invocation, hsh treats the first argument as a file from which to read commands. The supplied file should contain one command per line. hsh runs each of the commands contained in the file in order before exiting.

Example:
```
$ cat test
echo 'hello'
$ ./hsh test
'hello'
$
```

## Environment
Upon invocation, `hsh` receives and copies the environment of the parent process in which it was executed. This environment is an array of name-value strings describing variables in the format `NAME=VALUE`. A few key environmental variables are:

### HOME
The home directory of the current user and the default directory argument for the cd builtin command.
```
$ echo "echo $HOME" | ./hsh
/home/projects
```

### PWD
The current working directory as set by the cd command.
```
$ echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell
```

### OLDPWD
The previous working directory as set by the cd command.
```
$ echo "echo $OLDPWD" | ./hsh
/home/projects/alx/printf
```

### PATH
A colon-separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons, an initial colon, or a trailing colon) indicates the current directory.
```
$ echo "echo $PATH" | ./hsh
/home/projects/.cargo/bin:/home/projects/.local/bin:/home/projects/.rbenv/plugins/ruby-build/bin:/home/projects/.rbenv/shims:/home/projects/.rbenv/bin:/home/projects/.nvm/versions/node/v10.15.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/projects/.cargo/bin:/home/projects/workflow:/home/projects/.local/bin
```

## hsh Builtin Commands
### cd
- Usage: `cd [DIRECTORY]`
- Changes the current directory of the process to `DIRECTORY`.
- If no argument is given, the command is interpreted as `cd $HOME`.
- If the argument `-` is given, the command is interpreted as `cd $OLDPWD` and the pathname of the new working directory is printed to standad output.
- If the argument, `--` is given, the command is interpreted as `cd $OLDPWD` but the pathname of the new working directory is not printed.
- The environment variables `PWD` and `OLDPWD` are updated after a change of directory.

Example:
```
$ ./hsh
$ pwd
/home/projects/alx/simple_shell
$ cd ../
$ pwd
/home/projects/alx
$ cd -
$ pwd
/home/projects/alx/simple_shell
```

### alias
- Usage: `alias [NAME[='VALUE'] ...]`
- Handles aliases.
- `alias`: Prints a list of all aliases, one per line, in the form `NAME='VALUE'`.
- `alias NAME [NAME2 ...]`: Prints the aliases NAME, NAME2, etc. one per line, in the form `NAME='VALUE'`.
- `alias NAME='VALUE' [...]`: Defines an alias for each NAME whose `VALUE` is given. If name is already an alias, its value is replaced with `VALUE`.

Example:
```
$ ./hsh
$ alias show=ls
$ show
AUTHORS            builtins_help_2.c  errors.c         linkedlist.c        shell.h       test
README.md          env_builtins.c     getline.c        locate.c            hsh
alias_builtins.c   environ.c          helper.c         main.c              split.c
builtin.c          err_msgs1.c        helpers_2.c      man_1_simple_shell  str_funcs1.c
builtins_help_1.c  err_msgs2.c        input_helpers.c  proc_file_comm.c    str_funcs2.c
```

### exit
- Usage: exit [STATUS]
- Exits the shell.
- The STATUS argument is the integer used to exit the shell.
- If no argument is given, the command is interpreted as exit 0.

Example:
```
$ ./hsh
$ exit
```

### env
- Usage: env
- Prints the current environment.

Example:
```
$ ./hsh
$ env
NVM_DIR=/home/projects/.nvm
...
```

### setenv
- Usage: setenv [VARIABLE] [VALUE]
- Initializes a new environment variable, or modifies an existing one.
- Upon failure, prints a message to stderr.

Example:
```
$ ./hsh
$ setenv NAME Poppy
$ echo $NAME
Poppy
```

### unsetenv
- Usage: `unsetenv [VARIABLE]`
- Removes an environmental variable.
- Upon failure, prints a message to `stderr`.

Example:
```
$ ./hsh
$ setenv NAME Poppy
$ unsetenv NAME
$ echo $NAME
$
```
**Thank you for going through our shell documentation.**

## AUTHORS

### 🤵🏼 Olaoluwa ISOGUN
- [GitHub](https://github.com/OlaoluwaISOGUN)
- [LinkedIn](https://www.linkedin.com/in/olaoluwa-isogun-31b602247)

### 🤵🏻‍ Oluwatomisin ISOGUN

<a href="https://www.linkedin.com/in/oluwatomisin-isogun-1b766823b/"><img align="left" src="https://raw.githubusercontent.com/TosinISOGUN/TosinISOGUN/main/linkedin.png" alt="Tosin ISOGUN | LinkedIn" width="40px"/></a>
<a href="https://m.facebook.com/tosintokunbo.isogun/"><img align="left" src="https://raw.githubusercontent.com/TosinISOGUN/TosinISOGUN/main/facebook.svg" alt="Tosin ISOGUN | Facebook" width="40px"/></a>
<a href="https://wa.link/nxtuti/"><img align="left" src="https://raw.githubusercontent.com/TosinISOGUN/TosinISOGUN/main/whatsapp2.png" alt="Tosin ISOGUN | WhatsApp" width="40px"/></a>
<a href="https://www.instagram.com/oluwatomisinisogun/"><img align="left" src="https://raw.githubusercontent.com/TosinISOGUN/TosinISOGUN/main/instagram.svg" alt="Tosin ISOGUN | Instagram" width="40px"/></a>
<a href="https://mobile.twitter.com/tomson172/"><img align="left" src="https://raw.githubusercontent.com/TosinISOGUN/TosinISOGUN/main/twitter.svg" alt="Tosin ISOGUN | Twitter" width="40px"/></a>
