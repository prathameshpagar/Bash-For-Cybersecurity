- to see the list of environment variables set by bash, run the env command directly from the shell 


``` bash 
env

SHELL=/bin/bash 
LANGUAGE=en_CA:en
DESKTOP_SESSION=ubuntu
PWD=/home/user
```


- we can read individual environmental variables using the echo command. 

```bash
echo ${SHELL}
echo $SHELL

/bin/bash
```



Full list of bash variables can be found here : 

https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html

#### SheBang Line 

```shell
#!/bin/bash
```


- the shebang line can also take optional arguments to change how the script executes. For example, you could pass the special arguments -x to your bash shebang, like so: 

```shell
#!/bin/bash
```

This option will print all commands and their arguments as they are executed to the terminal. It is useful for debugging scripts as you are developing them.


- Another example of an optional arguments is -r:

```shell
#!/bin/bash -r 
```

this optional argument will create a restricted bash shell which restricts certain potentially dangerous commands the could for example, navigate to certain directories, change sensitive environment variables. or attempt to turn off the restricted shell from within the script 


- Specifying an argument within the shebang line requires modifying the script, but you can also pass arguments to the bash interpreter using the syntax in listing 

for e.g. 

```shell
bash -r myscript.sh 
```

### Comments 

- This are the comments in the bash 

```shell
#this is my first script 
```

Except shebang line ^ 

### Execution 

```shell
chmod u+x helloworld.sh
```

```shell
./helloworld.sh
```

we can execute bash file using syntax bash also 

```shell
bash helloworld.sh
```

### Debugging 

```shell
bash -n helloworld.sh
```


- If you want to start debugging at a given point in the script, you can do this by including the set command in the script itself 

```shell
#!/bin/bash

set -x 

set +x 
```



## Basic Syntax 

The most basic bash scripts are just lists of Linux commands collected in a single file. For example, you could write a script that creates resources on a system and then prints information about these resources to the screen.

```shell
#!/bin/bash
# All this script does is create a directory, create a file 
# within the directory, and then list the content of the directory

mkdir directory
touch mydirectory/myfile
ls -l mydirectory
```


## Variables 

• They can include alphanumeric characters.
• They cannot start with a numerical character.
• They can contain an underscore (_).
• They cannot contain whitespace.

### Assigning and Accessing Variables 

```shell
book="Pride and Prejudice"
echo "This book's name is ${book}"
```

You can also assign the output of command to a variable using the command substitution syntax $(), placing the desired command between the two parentheses. You'll use this syntax often in bash programming 

```shell
root_directory="$(ls -la /)"
echo "$(root_directory)"
```

- note that you shouldn't leave whitespace around the assignments symbol (=) when creating a variable. 

### Unassigning Variables 

You can unassign assigned variables using the unset command, as shown here 

```shell
book="Pride and Prejudice"
unset book
echo "${book}"
```

- if you execute these command s in the terminal no output will be shown after the echo command executes  


## Scoping Variables 

Global variables are those available to the entire program but variables in bash can also be scoped so that they ate only accessible from within a certain block of a code. These variables are called local variables and are declared using the local keyword. 

```shell 
#!/bin/bash 

PUBLISHER="Jane Austen"
print_name() {
local name
name="Pride and Prejudice"
echo "${name} by ${PUBLISHER}"
}
print_name

echo "The variable ${name} will not be printed because it is a local variable."
```


The echo command at the end of the script file will result in an empty variable, as the name variable is locally scoped to the print_name() function, which means that nothing outside of the function can access it. So, it will simply return without a value.


## Arithmetic Operators 

Arithmetic operators allow you to perform mathematical operations on integers

| Operator | Description                |
| -------- | -------------------------- |
| +        | Addition                   |
| -        | Subtraction                |
| *        | Multiplication             |
| /        | Division                   |
| %        | Modulo                     |
| +=       | Incrementing by a constant |
| -=       | Decrementing by a constant |


you can perform these arithmetic operation in bash in few ways: using the let command, using the double parentheses syntax $((expression)), and using the expr command. Let's consider an example of each method 

- Here we perform a multiplication operation using the let command: 

```shell
let result="4 * 5"
echo $result
```

- This command takes a variable name, then performs an arithmetic calculation to resolve its value. Next, we perform another multiplication operation using the double parentheses syntax.

```shell
result=$((5 * 5))
echo $result
```

- we perform an addition operation using the expr command:

```shell
result=$(expr 5 + 505)
echo $result
```


## Arrays 

Arrays are a collection of elements that are indexed you can access these elements using their index numbers, where the first indexed number starts from zero  In bash scripts, you might use arrays whenever you need to iterate over a number of strings and run the same commands on each one.


```shell
#!/bin/bash
IP_ADDRESSES=(192.168.1.1 192.168.1.2 192.168.1.3)

echo "${IP_ADDRESSES[*]}"      #this prints evry element from array

echo "${IP_ADDRESSES[0]}"      #this prints specific element 
```


As you can see, we were able to get bash to print all elements in the array, as well as just the first element.

You can also delete elements from an array. The following will delete 192.168.1.2 from the array:

```shell
IP_ADDRESSES=(192.168.1.1 192.168.1.2 192.168.1.3)

unset IP_ADDRESSES[1]

IP_ADDRESSES[0]="192.168.1.10"
```


## Streams 

Streams are files that act as communication channels between a program and its environment. When you interact with a program (whether a built-in Linux utility such as ls or mkdir or one that you wrote yourself) you're interacting with one or more streams. In bash, there are three standard data streams, as shown 

| Stream name              | Description                            | File Descriptor Number |
| ------------------------ | -------------------------------------- | ---------------------- |
| Standard Input (stdin)   | Data coming into some program as input | 0                      |
| Standard Output (stdout) | Data coming out of a program           | 1                      |
| Standard Error (stderr)  | Errors coming out of a program         | 2                      |

## Control Operators 

Control operators in bash are tokens that perform a control function.

| Operator | Description                                                                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| &        | Sends a command to the background                                                                                                                                  |
| &&       | Used as a logical AND. The second command in the expression will be evaluated only if the first command evaluated to true                                          |
| (and)    | Used for command grouping                                                                                                                                          |
| ;        | Used as a list terminator. A command following the terminator will run after the preceding command has finished, regardless of whether it evaluates to true or not |
| ;;       | Ends a  case statement                                                                                                                                             |
| \|       | Redirects the output of a command as input to another command                                                                                                      |
| \|\|     | Used as a logical OR. The second command will run if the first one evaluates to false                                                                              |


The && operator allows us to perform an AND opertaion between two commands in this case the file test123 will be created only if the first command was successful:

```shell
touch test && touch123
```

The () operator allows us to group commands together so they act a single unit when we need to redirect them together:

```shell
(ls; ps)
```

The ; operator allows us to run multiple commands irrespective of their exit status, each command is executed one after the other, as soon as the previous one finishes

```shell
ls; ps; whoami
```

The || operator allows us to chain commands together using an OR operator, In this example the echo command will be executed only if t he first command fails.

```shell
lzl || echo "the lzl command failed"
```


## Redirection Operators 

The three standard streams we highlighted earlier can be redirected form one program to another. Redirection is taking some output from one command or script and using it as the input to another,

| Operator | Description                                                                     |
| -------- | ------------------------------------------------------------------------------- |
| >        | Redirects stdout to a file                                                      |
| >>       | Redirects stdout to a file by appending it to the existing content              |
| &> or >& | Redirects stdout and stderr to a file                                           |
| &>>      | Redirects stdout and stderr to a file by appending it to the existing content   |
| <        | Redirects input to a command                                                    |
| <<       | Called a here document or heredoc, redirects multiple input lines to a command  |

- The > operator redirects the standard output stream to a file. Any command that preceds this character will send its output to the specified location

```shell
echo "Hello World" > output.txt
```

We redirected  the standard output stream to a file named output.txt to see the content of output.txt, simply run the following:

```shell
cat output.txt

Hello World!
```

Next, we'll use the >> operator to append some content to the end of the same file:

```shell
echo "Goodbye!" >> output.txt
cat 
```


You can redirect both the standard output stream and the standard error stream to file using &>.  This is useful when you don't want to send any output to the screen and instead save everything in a log file (perhaps for later analysis)

```shell
ls -l / &> stdout_and_stderr.txt
```


- To append both standard output and standard error streams to a file, simple use double chevron (&>>). If we wanted to send the standard  error stream to another? this is also possible using the streams' file descriptor numbers:

```shell
ls -l / l> stdout.txt 2> stderr.txt
```

- You may sometimes find it useful to redirect the standard error stream to a file, as we'he done here, so you can log any errors that occur during runtime. For example the next example run a non-existent command lzl. This should generate that will be written into the error.txt file.

```shell
lzl 2> error.txt    # remember 2 is stderr used for error output 
cat error.txt
```

| Number | Name   | Meaning          |
| ------ | ------ | ---------------- |
| 0      | stdin  | input (keyboard) |
| 1      | stdout | normal output    |
| 2      | stderr | error output     |

- Next let's use the standard input stream. Run the following command in the shell to supply the contents of output.txt as input to the cat command.

```shell
cat < output.txt
Hello World!
Goodbye!
```

- What if we wanted to redirect multiple lines to a command. redirection (<<) can help with this: In this command we pass multiple lines as input to a command the EOF in this example acts a delimiter marking the start and end points of the input

```shell
cat <<EOF
Hello World  
Goodbye
EOF
```


- The pipe operator (|) redirects the output of one command and uses it as the input of another. For example m we could run the ls command on the root directory and then use another command to extract some data from it, as shown here: 

```shell
ls -l / | grep "bin"
```



## Positional Arguments  

Bash scripts can take positional arguments (also called parameters) passed on the command line, Arguments are especially useful, for example, when you want to develop a program that modifies its behaviors based on some input passed to it by another program or user. Arguments can also change features of the script such as the output format and how verbose it will be during runtime.

A bash script can access arguments passed to it on the command line using the variables $1, $2, and so on. The number represents the order in which the argument was entered. To illustrate this, the following script takes in an argument (an IP address or domain name) and performs a ping test against it using the ping utility. Save this file as ping_with_arguments.sh:

```shell
#!/bin/bash

# this script will ping any address provided as an argument

SCRIPT_NAME="${0}"
TARGET="${1}"

echo "Running  the script ${SCRIPT_NAME}"
echo "Pinging the target: ${TARGET}..."
ping "${TARGET}"
```

0 itself is script name and 1 will be first positional argument 


- What if you wanted to access all arguments. You can do so using the variable $ @ Also, using $#, you can get the total number of arguments passed

```shell
# # shows the number of arguments passed 
# @ show total arguments which are passed 

echo "The arguments are: $@"
echo "The total number of arguments are: $#"
```


| Variable   | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| $0         | The name of the script file                                            |
| $1, $2, $3 | Positional Arguments                                                   |
| $#         | The number of passed positional arguments                              |
| $*         | All positional arguments                                               |
| $@         | All positional arguments, where each arguments is individually quoted  |

when a script makes use of "$" with the quotes included, bash will expand arguments into a single word, For instance, the following groups the arguments into one word : 

```shell
script.sh "1" "2" "3"
1 2 3 
```

when a script makes use of "$@" (again including the quotes), it will expand arguments into separate words:

```shell
script.sh "1" "2" "3"
 1
 2
 3
```

(In most cases, you will want to use "$@" so that every arguments is treated as an individual word.)

## Input Prompting 

- Some bash scripts don't take any arguments during execution. however, they may need to ask the user for some information in a n interactive wat and have the response feed into their runtime. In these cases, we can use the read command. You often see applications use input prompting when attempting to install some software, asking the user to enter yes to proceed or no to cancel the operation

- In this script we will ask the use for their first and last name, then print these to the standard output stream

```shell
#!/bin/bash
# Takes input form the user and assigns it to variables 

echo "What is your firs name ? "
read -r firstname

echo "What is your lastname "
read -r lastname 

echo "Your first name is ${firstname} and your last name is ${lastname}"
```


## Exit Status Codes 

Bash commands return status codes, which indicate whether the execution of the command succeeded. Status codes fall in the 0-255 range, where 0 means success, 1 means failure, 126 means that the command was found but is not executable, and 127 means the command was not found. The meaning of any other number depends on the specific command being used and the logic it uses.

- To see status codes in action, save the following script to a file named exit_codes.sh and run it.

```shell
!#/bin/bash

# Experimenting with status codes 

ls -l > /dev/null
echo "The status code of the ls command  was: $?"

lzl 2> /dev/null
echo "The status  code of the non-existing lzl command was: $?"
```

┌──(kali㉿kali)-[~/Desktop/Bash]
└─$ ./exit_codes.sh 
The status code of the ls command was: 0
The status code of the non-existing lzl command was: 127

## Setting a Script's Exit Codes 

You can set the exit code of a script using the exit command, as shown below 

```shell
#!/bin/base

#Sets the exit code of the script to be 223

echo "Exiting with status code: 223"
exit 223
```
