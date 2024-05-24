#quoting
three types of quoting: matched single\double quotes,backslash.
single quotes:preserves the literal meaning of all the characters(except single quotes)
double quotes:except dollarsign($)\backquote(\`)\backslash(\\)

#Redirections
n==> file descriptor
箭头向右  stdout   向左  stdin
[n]> file   redirect stdout or n  to file
[n]< file   redirect stdin or n from file
[n]>| file  redirect with no overwrite
[n]>> file append n to file
[n1]<&[n2]  copy file descriptor n2 as fd n1(or stdout)
[n]<&- close n/stdin.
[n1]>&n2  copy fd n2 as n1(or stdin)
[n]>&- close n/stdout

#Search-and-execution
**shell commands:**
all of the shell positional parameters (except $0) are set to the arguments of the shell function. The positional parameters are restore to their original values when the command completes. This all occurs within the current shell.
**Shell builtins:**
are executed internally to the shell, without spawning a new process.
**normal program**
if the command name doesn't match a function or builtin, the command is searched for as a normal program in the file system. When a normal program is executed, the shell runs the program , passing the arguments and the environment to the program. If the program is not a normal executable file (i.e., if it does not begin with the "#!")
 the shell will interpret the program in a subshell. The child shell will reinitialize itself in this case, as if a new shell had been invoked to handle the ad-hoc shell script, except that the location of hashed commands located in the parent shell will be remembered by the child.

#Path-search
When locating a command, the shell first looks to see if it has a shell function by that name. Then it lookf for a builtin command by that name. If a builtin command is not found, one of two things happen:
1.command names containing a slash are simply executed without performing any searches;
2.The shell searches each entry in PATH in turn for the command. The value of the PATH variable should be a series of entries separated by colons. Each entry consists of directory name. The current directory may be indicated implicitly by an empty directory name, or explicitly by a single period.
#Command-Exit-Status
Each command has an exit status that can influence the behaviour of other shell commands. 
The paradigm is that a command exits with zero for normal or success, and non-zero for failure, error, or a false indication.
The man page for each command should indicate the various exit codes and what they mean. Additionally the builtin commands return exit codes, as does an executed shell function.
If a command consists entirely of variable assignments then the exit status of the command is that of the last command substitution if any, otherwise 0.
#Pipelines
[!]command 1[ | command 2 ...]
The stdout of command 1 is connected to the std in of command 2.
If the pipeline is not in the background, the shell waits for all commands to complete.
[!] means the exit status is the logical NOT of the exit status of the last command. 
==*Pipeline assignment of stdout/stdin both takes place before redirection:* ==
**command1 2>&1 | command2** sends both the stdout and stderr of command1 to the stdin of command2.
**==Note that unlike some other shells, each precess in the pipeline is a child of the invoking shell ( unless it is a shell builtin,but any effect it has on the environment is wiped)

#Background-Command &
#character-escape-sequence
\\a<bell>\b<backspace>\e<esc>
\f<form-feed>换页 \n<new-line>换行 \r<carriage-return> 回车
\t<tab> \v<vertical-tab> \\  \ddd <oct> \xhh<hex>
_'\x000000000000F' == '\xF'_