# Extending the interpreter and its interactive environment

_The purpose of the Monkey Programming Language is to learn different aspects of implementing a language. The main purpose of this repo is for me to learn go by fooling around. In addition, it serves the purpose of plunging into the business of interpreting Monkey PL more systematically. It reveals bugs in the original software by adding tests and offers fixes for these bugs. Moreover, it extends the interactive environment such that a user can gain more insight into the structure of the chosen abstract syntax tree (ast) and the steps of interpretation. At least, that's the plan..._


Monkey PL is a language that allows one to learn how a language is implemented. The intent of this repo is mainly to give a learner (me in the first place) the possibility to visualize certain aspects of it - mostly the ast and the evaluation process. It this provides many tools (...)
In addition, there is an interactive environment with commands calling these tools and some for convenience (like paste).

Since Monkey is a language intended for learning purposes, the implementations are designed in such a way that it is open for changes and additions.

The interpreter described in the book has some problems. Some of these are documented in additional tests. The effect of changes in the implementation can be visualized and watched by the interactive environment.



 _to be continued..._

## Step 5: Add representations of evaluation

The command `trace` now provides possibilities for displaying environments

- for each evaluation step, it displays a short name - e.g. `e0` - of the environment used in the this step.
- if the user hits `e`, it displays the current state of this environment.
- if the the current environment changes, this is indicated by printing the environment in red.
  - changes can either be:
    - the evaluation switches to another environment
    - the current environment has been changed

### An example
![shot](assets/images/shot_tracer_env.png)

### A demo
![Demo11](assets/demos/demo11.gif)

### As pdf

- version 0 - dags are represented by node duplication
- new setting: `evalfile`

| NAME           |                   | USAGE                                                    |
--- | --- | --- |
| (set)          | ~ evalfile `<f>`   | set file that outputs  eval-pdfs to `<f>`                        |



- expl for evaluation of the program `if(true){}`:

![expl](visualizer/images/eval1.png)



### In the console - version 2

- TODO: _add verbosity by displaying info on environment_

### In the console - version 1
inspo from [swipl](https://www.swi-prolog.org/)

If the setting `logtrace` is set, the evaluation trace is still output in a table.

If the command `trace` is used, the evaluation trace is output step by step - this can be interrupted by typing `a`.

![Demo10](assets/demos/demo10.gif)

#### New instructions

| NAME           |                   | USAGE                                                    |
--- | --- | --- |
| trace          | ~ `<input>`         | show evaluation trace step by step                       |
| (set)          | ~ logtrace        | additionally output evaluation trace                     |


### In the console - version 0
If the setting `logtrace` is set or the command `trace` is used, the evaluation trace is output.


![Demo9](assets/demos/demo9.gif)


## Step 4: Add representations of asts 

![Demo8](assets/demos/demo8.gif)

- new instruction set: 

| NAME           |                   | USAGE                                                    |
--- | --- | --- |
| cl[earscreen]  | ~                 | clear the terminal screen                                |
| clear          | ~                 | clear the environment                                    |
| e[val]         | ~ `<input>`         | print out value of object `<input>` evaluates to           |
| expr[ession]   | ~ `<input>`         | expect `<input>` to be an expression                       |
| h[elp]         | ~                 | list all commands with usage                             |
|                | ~ `<cmd>`           | print usage command `<cmd>`                                |
| l[ist]         | ~                 | list all identifiers in the environment alphabetically   |
|                |                   |      with types and values                               |
| p[arse]        | ~ `<input>`         | parse `<input>`                                            |
| paste          | ~ `<input>`         | evaluate multiline `<input>` (terminated by blank line)    |
| prog[ram]      | ~ `<input>`         | expect `<input>` to be a program                           |
| q[uit]         | ~                 | quit the session                                         |
| reset          | ~ `<setting>`       | set `<setting>` to default                                 |
|                |                   |      for an overview consult :settings and/or :h set     |
| set            | ~ process `<p>`     | `<p>` must be: [e]val, [p]arse, [t]ype                     |
|                | ~ level `<l>`       | `<l>` must be: [p]rogram, [s]tatement, [e]xpression        |
|                | ~ logparse        | additionally output ast-string                           |
|                | ~ logtype         | additionally output objecttype                           |
|                | ~ incltoken       | include tokens in representations of asts                |
|                | ~ paste           | enable multiline support                                 |
|                | ~ prompt `<prompt>` | set prompt string to `<prompt>`                            |
|                | ~ treefile `<f>`    | set file that outputs pdfs to `<f>`                        |
| settings       | ~                 | list all settings with their current values and defaults |
| stmt|statement | ~ `<input>`         | expect `<input>` to be a statement                         |
| t[ype]         | ~ `<input>`         | show objecttype `<input>` evaluates to                     |
| unset          | ~ `<setting>`       | set boolean `<setting>` to false                           |
|                |                   |      for an overview consult :settings and/or :h set     |

#### As pdf


![some tree](assets/images/show.png)


#### In the console
If the setting `logtype` is set or the command `parse` is used, the output so far was just the output of the `String()`-method that nodes provide. Now, there is a more detailed representation provided.

Expression nodes are colored in yellow, statement nodes in blue and program nodes in a darker blue. The colors don't work for windows users.

![Demo7](assets/demos/demo7.gif)

## Step 3: Add dimensions: settings `level <l>` and `process <p>`

![Demo6](assets/demos/demo6.gif)


#### `(set|reset) level (program|statement|expression)`

Every ast node is either a program, a statement or an expression. Until now, we treat each input as a program, which means, we can also only evaluate a program and show the type for the evaluation result of a program.
In this step, we implement another setting that chooses to parse and thus further evaluate the input as either program,statement or expression.
In addition, the commands `expr[ession]`, `stmt|statement` and `prog[ram]` are implemented.

#### `(set|reset) process (parse|eval|type)`

Furthermore, we implement settings for the way the input is to be processed: it can either be only parsed (`parse`) and output the ast, which implements the Stringer interface, or evaluated and output, which type the value is (`type`) or the value of the object via the `Inspect()`-method of objects (`eval`). The commands `type`, `eval` and `parse` behave exactly as if `process (parse|eval|type)` were set for a single command. Since, `type` and `eval` are already implemented, only `parse` is added.

Logging can be extended by the setting `logparse`, which additionally outputs the ast as string.

The full instruction set is now:

| NAME           |                   | USAGE                                                    |
--- | --- | --- |
| clear          | ~                 | clear the environment                                    |
| e[val]         | ~ `<input>`         | print out value of object `<input>` evaluates to           |
| expr[ession]   | ~ `<input>`         | expect `<input>` to be an expression                       |
| h[elp]         | ~                 | list all commands with usage                             |
|                | ~ `<cmd>`           | print usage command `<cmd>`                                |
| l[ist]         | ~                 | list all identifiers in the environment alphabetically   |
|                |                   |      with types and values                               |
| p[arse]        | ~ `<input>`         | parse `<input>`                                            |
| paste          | ~ `<input>`         | evaluate multiline `<input>` (terminated by blank line)    |
| prog[ram]      | ~ `<input>`         | expect `<input>` to be a program                           |
| q[uit]         | ~                 | quit the session                                         |
| reset          | ~ process         | set process to default                                   |
|                | ~ level           | set level to default                                     |
|                | ~ logparse        | set logparse to default                                  |
|                | ~ logtype         | set logtype to default                                   |
|                | ~ paste           | set multiline support to default                         |
|                | ~ prompt          | set prompt to default                                    |
| set            | ~ process `<p>`     | `<p>` must be: [e]val, [p]arse, [t]ype                     |
|                | ~ level `<l>`       | `<l>` must be: [p]rogram, [s]tatement, [e]xpression        |
|                | ~ logparse        | additionally output ast-string                           |
|                | ~ logtype         | additionally output objecttype                           |
|                | ~ paste           | enable multiline support                                 |
|                | ~ prompt `<prompt>` | set prompt string to `<prompt>`                            |
| settings       | ~                 | list all settings with their current values and defaults |
| stmt|statement | ~ `<input>`         | expect `<input>` to be a statement                         |
| t[ype]         | ~ `<input>`         | show objecttype `<input>` evaluates to                     |
| unset          | ~ logparse        | don't additionally output ast-string                     |
|                | ~ logtype         | don't additionally output objecttype                     |
|                | ~ paste           | disable multiline support                                |


## Step 2: Implement a small initial instruction set for the interactive environment

- new interactive environment in directory `session`
    - inspo from [ghci](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/ghci.html#ghci-commands) and [gore](https://github.com/motemen/gore) 

![Demo5](assets/demos/demo5.gif)


| NAME   |                   | USAGE                                                   |
|--------|-------------------|---------------------------------------------------------|
| clear    | ~                 | clear the environment                                    |
| e[val]   | ~ `<input>`         | print out value of object `<input>` evaluates to           |
| h[elp]   | ~                 | list all commands with usage                             |
|          | ~ `<cmd> `          | print usage command `<cmd>`                                |
| l[ist]   | ~                 | list all identifiers in the environment alphabetically   |
|          |                   |      with types and values                               |
| paste    | ~ `<input>`         | evaluate multiline `<input>` (terminated by blank line)    |
| q[uit]   | ~                 | quit the session                                         |
| reset    | ~ logtype         | set logtype to default                                   |
|          | ~ paste           | set multiline support to default                         |
|          | ~ prompt          | set prompt to default                                    |
| set      | ~ logtype         | when eval, additionally output objecttype                |
|          | ~ paste           | enable multiline support                                 |
|          | ~ prompt `<prompt>` | set prompt string to `<prompt> `                           |
| settings | ~                 | list all settings with their current values and defaults |
| t[ype]   | ~ `<input>`         | show objecttype `<input>` evaluates to                     |
| unset    | ~ logtype         | when eval, don't additionally output objecttype          |
|          | ~ paste           | disable multiline support                                |


- if `input` is not prefixed by `:<cmd>`, it is equivalent to `:eval input`


## Step 1: Write Tests for bugs in parser and evaluator

- `parser/parser_add_test.go`
- `evaluator/evaluator_add_test.go`

## Step 0: Starting Point: Copy the Code

- unaltered code from the book [_Writing an Interpreter in Go_](https://interpreterbook.com/), Version 1.7





