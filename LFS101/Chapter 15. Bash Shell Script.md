##Features and cababilities
1. shebang
  1. #!/bin/bash
  2. #!/usr/bin/python
2. comment #
3. read var_1
4. echo $var_1
5. exit val
6. echo $?

##Syntax
```
- \# comment
- \ continue to the next line
- ; execute as a new command
- $ variable
```
1. 
  1. cmd_1 ; cmd_2 ; cmd_3
  2. cmd_1 && cmd_2 && cmd_3
  3. cmd_1 || cmd_2 || cmd_3
2. function:
  func () {
    echo $1;
    echo $2
  }
3. Command substitution
  - ls -l $(ls | sort | head -n 1)
4. Environment variables
  - VAR=value
5. Exporting environment variables
  - child processes don't have automatic access to variables
  - VAR=value; export VAR
6. Script parameters
  - $0 name of the script
  - $1, $2, etc
  - $* all parameters
  - $# number of parameters
7. Output redirect 
  - >
  - >>
8. Input redirect
  - <
  
##Constructs
1. if [[test]]; then ; fi
2. test: man test
3. numerical test
4. expr
  - x = $(expr 1 +　2) ; echo $x
  
