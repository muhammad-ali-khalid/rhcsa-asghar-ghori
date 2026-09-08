# RHCSA Chapter 21: Shell Scripting

This is a concise reference guide based on Chapter 21 of the RHCSA book by Asghar Ghori. 

## 1. Overview of Shell Scripts
Shell scripts are text files containing Linux commands, control structures, and optional comments to automate repetitive tasks. 
- Scripts run top-to-bottom and do not need to be compiled.
- The first line must be the **shebang** (`#!/bin/bash`), which defines the absolute path to the interpreter.
- Comments start with the `#` character.

### Creating and Executing Scripts
- Default permissions for a new file usually lack execution rights.
- Add execute permissions: `chmod +x script.sh`
- Execute the script: `./script.sh` or by using the absolute path (e.g., `/usr/local/bin/script.sh`).

### Debugging
To debug a script and identify execution errors, run the script in debug mode:
- Run with bash: `bash -x script.sh`
- Or add `-x` to the shebang: `#!/bin/bash -x`

## 2. Variables
Variables hold data such as system information, paths, or strings.
- **Local Variables:** Defined within the script (e.g., `SYSNAME=server10.example.com`).
- **Environment Variables:** Pre-defined system variables (e.g., `$SHELL`, `$LOGNAME`).

## 3. Command Substitution
You can store the output of a command into a variable using two methods:
1. `$(command)` syntax (e.g., `SYSNAME=$(hostname)`)
2. Backticks `` `command` `` (e.g., KERNVER=`` `uname -r` ``)

## 4. Shell Parameters
Parameters hold values and are heavily used in shell scripting.

### Special Parameters
- `$0`: The name of the script/command itself.
- `$*` or `$@`: All arguments supplied to the script.
- `$#`: The total count of supplied arguments.
- `$$`: The Process ID (PID) of the script.
- `$?`: The exit code of the last executed command.

### Positional Parameters (Command Line Arguments)
- Arguments passed to the script are accessed via `$1`, `$2`, `$3`, etc.
- Arguments beyond 9 must be enclosed in curly brackets: `${10}`.

### The `shift` Command
- Moves arguments one position to the left. For instance, the value of `$2` becomes `$1`.
- The original value of `$1` is lost during a shift.
- You can perform multiple shifts at once (e.g., `shift 2`).

## 5. Exit Codes and Test Conditions

### Exit Codes
- Return a zero (`0`) for success.
- Return a non-zero value for failure.
- Checked using the special parameter `$?`.
- Can be explicitly set in scripts using the `exit <number>` command.

### Test Conditions
Evaluated using the `test` command or square brackets `[ condition ]`.
*(Note: Always ensure spaces after `[` and before `]`)*

**Integer Tests:**
- `-eq` (equal to), `-ne` (not equal to)
- `-lt` (less than), `-gt` (greater than)
- `-le` (less than or equal to), `-ge` (greater than or equal to)

**String Tests:**
- `=` (identical), `!=` (not identical)
- `-z` (length is zero)
- `-n` (length is non-zero)

**File Tests:**
- `-d` (directory), `-f` (normal file)
- `-e` (exists), `-s` (non-empty)
- `-r` (readable), `-w` (writable), `-x` (executable)
- `file1 -nt file2` (file1 is newer than file2), `file1 -ot file2` (file1 is older)

**Logical Operators:**
- `!` (NOT)
- `-a` or `&&` (AND)
- `-o` or `||` (OR)

## 6. Logical Constructs

### `if-then-fi`
Evaluates a condition and executes an action if true.
```bash
if [ condition ]
then
    action
fi
```

### `if-then-else-fi`
Executes Action 1 if true, otherwise executes Action 2.
```bash
if [ condition ]
then
    action1
else
    action2
fi
```

### `if-then-elif-fi`
Evaluates multiple conditions sequentially.
```bash
if [ condition1 ]
then
    action1
elif [ condition2 ]
then
    action2
else
    action3
fi
```

## 7. Arithmetic Test Conditions
Used for mathematical evaluation in scripts.
- Arithmetic processors/commands: `expr`, `let`
- Alternative syntax: `(())` or `""`
- **Operators:**
  - `!`: Negation
  - `+`, `-`, `*`, `/`: Basic arithmetic
  - `%`: Remainder
  - `<`, `<=`, `>`, `>=`: Comparisons
  - `=`: Assignment
  - `==`, `!=`: Equality / Non-equality

## 8. Looping Constructs (The `for` Loop)
Executes an action repeatedly on an array of elements until the list is exhausted.
```bash
for VAR in list
do
    action
done
```
*Example:*
```bash
for USER in user{10..12}
do
    /usr/sbin/useradd $USER
done
```
*(Note: Other loops like `while` and `until` exist but are out of scope for this chapter.)*