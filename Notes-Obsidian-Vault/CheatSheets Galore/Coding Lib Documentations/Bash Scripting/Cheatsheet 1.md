## Table of Contents

- [Bash Scripting Cheat Sheet](#bash\scripting\cheat\sheet)
  - [Getting Started](#Getting\Started)
  - [Basic Script Example](#Basic\Script\Example)
  - [Variables](#Variables)
- [Use quotes to prevent word splitting and globbing](#use\quotes\to\prevent\word\splitting\and\globbing)
  - [String Quotes](#String\Quotes)
  - [Shell Execution (Command Substitution)](#Shell\Execution\(Command\Substitution))
  - [Conditional Execution](#Conditional\Execution)
  - [Functions](#Functions)
  - [Conditionals](#Conditionals)
  - [Strict Mode](#Strict\Mode)
  - [Brace Expansion](#Brace\Expansion)
  - [Advanced Bash Scripting](#Advanced\Bash\Scripting)
    - [Arrays](#Arrays)
    - [Associative Arrays (Hash Maps)](#Associative\Arrays\(Hash\Maps))
    - [Redirections](#Redirections)
    - [Loops](#Loops)
      - [For Loop](#For\Loop)
      - [While Loop](#While\Loop)
    - [Arithmetic Operations](#Arithmetic\Operations)
    - [Here Documents](#Here\Documents)
    - [Process Substitution](#Process\Substitution)
    - [Advanced Features](#Advanced\Features)
      - [Using `trap` for Cleanup](#Using\`trap`\for\Cleanup)
      - [Regular Expressions](#Regular\Expressions)
      - [Substring Extraction](#Substring\Extraction)
      - [Case Statement](#Case\Statement)
      - [Functions with Return Values](#Functions\with\Return\Values)
    - [Debugging](#Debugging)

# Bash Scripting Cheat Sheet

Bash, or the Bourne Again SHell, is a powerful command-line tool and scripting language used on many Unix-like operating systems. This cheat sheet provides a quick overview of Bash scripting basics, from basic syntax to more advanced features.
## Getting Started

- **Learn Bash in Y Minutes:** [Learn Bash Quickly](https://learnxinyminutes.com/docs/bash/)
- **Bash Guide:** [Wooledge Bash Guide](https://mywiki.wooledge.org/BashGuide)
- **Bash Hackers Wiki:** [Bash Hackers Wiki](https://wiki.bash-hackers.org/)
## Basic Script Example

```bash
#!/usr/bin/env bash

name="John"
echo "Hello $name!"
```

## Variables

```bash
name="John"
echo $name   # Output: John
echo "$name" # Output: John
echo "${name}!" # Output: John!

# Use quotes to prevent word splitting and globbing
wildcard="*.txt"
options="iv"
cp -$options $wildcard /tmp
```

## String Quotes

- Double quotes `" "`: Variables are expanded.
- Single quotes `' '`: Variables are not expanded.

```bash
name="John"
echo "Hi $name"  # Output: Hi John
echo 'Hi $name'  # Output: Hi $name
```

## Shell Execution (Command Substitution)

```bash
echo "I'm in $(pwd)"
echo "I'm in `pwd`"  # Obsolescent form
```

## Conditional Execution

```bash
git commit && git push  # Push if commit succeeds
git commit || echo "Commit failed"  # Echo if commit fails
```

## Functions

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"  # Output: You are John
```

## Conditionals

```bash
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
fi
```

## Strict Mode

For robust scripting:

```bash
set -euo pipefail
IFS=$'\n\t'
```

## Brace Expansion

```bash
echo {A,B}.js        # Output: A.js B.js
echo {1..5}          # Output: 1 2 3 4 5
echo {{1..3},{7..9}} # Output: 1 2 3 7 8 9
```

---

## Advanced Bash Scripting

### Arrays

```bash
array=(one two three)
echo ${array[0]}     # Output: one
echo ${array[@]}     # Output: one two three
```

### Associative Arrays (Hash Maps)

```bash
declare -A arr
arr["key"]="value"
echo ${arr["key"]}   # Output: value
```

### Redirections

```bash
cmd > file.txt 2>&1  # Redirect stdout and stderr to file.txt
```

### Loops

#### For Loop

```bash
for i in {1..5}; do
  echo "Number $i"
done
```

#### While Loop

```bash
i=1
while [ $i -le 5 ]; do
  echo "Number $i"
  ((i++))
done
```

### Arithmetic Operations

```bash
((sum = 1 + 2))
echo $sum  # Output: 3
```

### Here Documents

```bash
cat << EOF
This is a text block
EOF
```

### Process Substitution

```bash
diff <(command1) <(command2)
```

### Advanced Features

#### Using `trap` for Cleanup

```bash
trap "echo 'Script interrupted'; exit" INT
```

#### Regular Expressions

```bash
[[ $var =~ ^[a-zA-Z]+$ ]] && echo "Alphabetic"
```

#### Substring Extraction

```bash
str="Bash Scripting"
echo ${str:5:7}  # Output: Script
```

#### Case Statement

```bash
case $var in
  pattern1) command1 ;;
  pattern2) command2 ;;
  *) default_command ;;
esac
```

#### Functions with Return Values

```bash
calculate() {
  local sum=$(( $1 + $2 ))
  return $sum
}

calculate 3 5
echo "Result: $?"  # Output: Result: 8
```

### Debugging

- Use `set -x` to debug.
- Use `set +x` to stop debugging.

---
