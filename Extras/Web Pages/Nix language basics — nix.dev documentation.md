# Nix language basics

The Nix language is designed for conveniently creating and composing _derivations_ – precise descriptions of how contents of existing files are used to derive new files. It is a domain-specific, purely functional, lazily evaluated, dynamically typed programming language.

Notable uses of the Nix language

- [Nixpkgs](../../reference/glossary.html#term-Nixpkgs)
    
    The largest, most up-to-date software distribution in the world, and written in the Nix language.
    
- [NixOS](../../reference/glossary.html#term-NixOS)
    
    A Linux distribution that can be configured fully declaratively and is based on Nix and Nixpkgs.
    
    Its underlying modular configuration system is written in the Nix language, and uses packages from Nixpkgs. The operating system environment and services it provides are configured with the Nix language.
    

You may quickly encounter Nix language expressions that look very complicated. As with any programming language, the required amount of Nix language code closely matches the complexity of the problem it is supposed to solve, and reflects how well the problem – and its solution – is understood. Building software is a complex undertaking, and Nix both _exposes_ and _allows managing_ this complexity with the Nix language.

Yet, the Nix language itself has only few basic concepts that will be introduced in this tutorial, and which can be combined arbitrarily. What may look complicated comes not from the language, but from how it is used.

## Overview[#](#overview "Permalink to this heading")

This is an introduction to **reading the Nix language**, for the purpose of following other tutorials and examples.

**Using the Nix language** in practice entails multiple things:

- Language: syntax and semantics
    
- Libraries: `builtins` and `pkgs.lib`
    
- Developer tools: testing, debugging, linting, formatting, …
    
- Generic build mechanisms: `stdenv.mkDerivation`, trivial builders, …
    
- Composition and configuration mechanisms: `override`, `overrideAttrs`, overlays, `callPackage`, …
    
- Ecosystem-specific packaging mechanisms: `buildGoModule`, `buildPythonApplication`, …
    
- NixOS module system: `config`, `option`, …
    

This tutorial only covers the most important language features, briefly discusses libraries, and at the end will direct you to reference material and resources on the other components.

### What will you learn?[#](#what-will-you-learn "Permalink to this heading")

This tutorial should enable you to read typical Nix language code and understand its structure. Its goal is to highlight where the Nix language may differ from languages you are used to.

It therefore shows the most common and distingushing patterns in the Nix language:

- [Assigning names and accessing values](#names-values)
    
- Declaring and calling [functions](#functions)
    
- [Built-in and library functions](#libraries)
    
- [Impurities](#impurities) to obtain build inputs
    
- [Derivations](#derivations) that describe build tasks
    

Important

This tutorial _does not_ explain all Nix language features in detail and _does not_ go into specifics of syntactical rules.

See the [Nix manual](https://nixos.org/manual/nix/stable/language/index.html) for a full language reference.

### What do you need?[#](#what-do-you-need "Permalink to this heading")

- Familiarity with software development
    
- Familiarity with Unix shell, to read command line examples
    
- A [Nix installation](../install-nix.html#install-nix) to run the examples
    

### How long does it take?[#](#how-long-does-it-take "Permalink to this heading")

- No experience with functional programming: 2 hours
    
- Familiar with functional programming: 1 hour
    
- Proficient with functional programming: 30 minutes
    

We recommend to run all examples. Play with them to validate your assumptions and test what you have learned. Read detailed explanations if you want to make sure you fully understand the examples.

### How to run the examples?[#](#how-to-run-the-examples "Permalink to this heading")

- A piece of Nix language code is a _Nix expression_.
    
- Evaluating a Nix expression produces a _Nix value_.
    
- The content of a _Nix file_ (file extension `.nix`) is a Nix expression.
    

Note

To _evaluate_ means to transform an expression into a value according to the language rules.

This tutorial contains many examples of Nix expressions. Each one is followed by the expected evaluation result.

The following example is a Nix expression adding two numbers:

1 + 2

3

#### Interactive evaluation[#](#interactive-evaluation "Permalink to this heading")

Use [`nix repl`](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-repl.html) to evaluate Nix expressions interactively (by typing them on the command line):

$ nix repl
Welcome to Nix 2.13.3. Type :? for help.

nix-repl> 1 + 2
3

Note

The Nix language uses lazy evaluation, and `nix repl` by default only computes values when needed.

Some examples show a fully evaluated data structure for clarity. If your output does not match the example, try prepending `:p` to the input expression.

Example:

nix-repl> { a.b.c = 1; }
{ a = { ... }; }

nix-repl> :p { a.b.c = 1; }
{ a = { b = { c = 1; }; }; }

Type `:q` to exit [`nix repl`](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-repl.html).

#### Evaluating Nix files[#](#evaluating-nix-files "Permalink to this heading")

Use [`nix-instantiate --eval`](https://nixos.org/manual/nix/stable/command-ref/nix-instantiate.html) to evaluate the expression in a Nix file.

$ echo 1 + 2 > file.nix
$ nix-instantiate --eval file.nix
3

Detailed explanation

The first command writes `1 + 2` to a file `file.nix` in the current directory. The contents of `file.nix` are now `1 + 2`, which you can check with

$ cat file.nix
1 + 2

The second command runs `nix-instantiate` with the `--eval` option on `file.nix`, which reads the file and evaluates the contained Nix expression. The resulting value is printed as output.

`--eval` is required to evaluate the file and do nothing else. If `--eval` is omitted, `nix-instantiate` expects the expression in the given file to evaluate to a special value called a _derivation_, which we will come back to at the end of this tutorial in [Derivations](#derivations).

Note

`nix-instantiate --eval` will try to read from `default.nix` if no file name is specified.

$ echo 1 + 2 > default.nix
$ nix-instantiate --eval
3

Note

The Nix language uses lazy evaluation, and `nix-instantiate` by default only computes values when needed.

Some examples show a fully evaluated data structure for clarity. If your output does not match the example, try adding the `--strict` option to `nix-instantiate`.

Example:

$ echo "{ a.b.c = 1; }" > file.nix
$ nix-instantiate --eval file.nix
{ a = <CODE>; }

$ echo "{ a.b.c = 1; }" > file.nix
$ nix-instantiate --eval --strict file.nix
{ a = { b = { c = 1; }; }; }

### Notes on whitespace[#](#notes-on-whitespace "Permalink to this heading")

White space is used to delimit [lexical tokens](https://en.m.wikipedia.org/wiki/Lexical_analysis#Token), where required. It is otherwise insignificant.

Line breaks, indentation, and additional spaces are for readers’ convenience.

The following are equivalent:

let
 x = 1;
 y = 2;
in x + y

3

let x=1;y=2;in x+y

3

## Names and values[#](#names-and-values "Permalink to this heading")

Values in the Nix language can be primitive data types, lists, attribute sets, and functions.

We show examples of primitive data types and lists in the context of [attribute sets](#attrset). Later in this section we cover special features of character strings: [string interpolation](#string-interpolation), [file system paths](#file-system-paths), and [indented strings](#indented-strings). We deal with [functions](#functions) separately.

[Attribute sets](#attrset) and [`let` expressions](#let) are used to assign names to values. Assignments are denoted by a single equal sign (`=`).

Whenever you encounter an equal sign (`=`) in Nix language code:

- On its left is the assigned name.
    
- On its right is the value, delimited by a semicolon (`;`).
    

### Attribute set `{ ... }`[#](#attribute-set "Permalink to this heading")

An attribute set is a collection of name-value-pairs, where names must be unique.

The following example shows all primitive data types, lists, and attribute sets.

Note

If you are familiar with JSON, imagine the Nix language as _JSON with functions_.

Nix language data types _without functions_ work just like their counterparts in JSON and look very similar.

**Nix**

{
  string = "hello";
  integer = 1;
  float = 3.141;
  bool = true;
  null = null;
  list = [ 1 "two" false ];
  attribute-set = {
    a = "hello";
    b = 2;
    c = 2.718;
    d = false;
  }; # comments are supported
}

**JSON**

{
  "string": "hello",
  "integer": 1,
  "float": 3.141,
  "bool": true,
  "null": null,
  "list": [1, "two", false],
  "object": {
    "a": "hello",
    "b": 1,
    "c": 2.718,
    "d": false
  }
}

Note

- Attribute names usually do not need quotes.[[1]](#attrnames)
    
- List elements are separated by white space.[[2]](#list-whitespace)
    

#### Recursive attribute set `rec { ... }`[#](#recursive-attribute-set-rec "Permalink to this heading")

You will sometimes see attribute sets declared with `rec` prepended. This allows access to attributes from within the set.

Example:

rec {
  one = 1;
  two = one + 1;
  three = two + 1;
}

{ one = 1; three = 3; two = 2; }

Note

Elements in an attribute set can be declared in any order, and are ordered on evaluation.

Counter-example:

{
  one = 1;
  two = one + 1;
  three = two + 1;
}

error: undefined variable 'one'

       at «string»:3:9:

            2|   one = 1;
            3|   two = one + 1;
             |         ^
            4|   three = two + 1;

### `let ... in ...`[#](#let-in "Permalink to this heading")

Also known as “`let` expression” or “`let` binding”

`let` expressions allow assigning names to values for repeated use.

Example:

let
  a = 1;
in
a + a

2

Detailed explanation

Assignments are placed between the keywords `let` and `in`. In this example we assign `a = 1`.

After `in` comes the expression in which the assignments are valid, i.e., where assigned names can be used. In this example the expression is `a + a`, where `a` refers to `a = 1`.

By replacing the names with their assigned values, `a + a` evaluates to `2`.

Names can be assigned in any order, and expressions on the right of the assignment (`=`) can refer to other assigned names.

Example:

let
  b = a + 1;
  a = 1;
in
a + b

3

Detailed explanation

Assignments are placed between the keywords `let` and `in`. In this example we assign `a = 1` and `b = a + 1`.

The order of assignments does not matter. Therefore the following example, where the assignments are in reverse order, is equivalent:

let
  a = 1;
  b = a + 1;
in
a + b

3

Note that the `a` in `b = a + 1` refers to `a = 1`.

After `in` comes the expression in which the assignments are valid. In this example the expression is `a + b`, where `a` refers to `a = 1`, and `b` refers to `b = a + 1`.

By replacing the names with their assigned values, `a + b` evaluates to `3`.

This is similar to [recursive attribute sets](#rec-attrset): in both, the order of assignments does not matter, and names on the left can be used in expressions on the right of the assignment (`=`).

Example:

`let ... in ...`

let
  b = a + 1;
  c = a + b;
  a = 1;
in {  c = c; a = a; b = b; }

{ a = 1; b = 2; c = 3; }

`rec { ... }`

rec {
  b = a + 1;
  c = a + b;
  a = 1;
}

{ a = 1; b = 2; c = 3; }

The difference is that while a recursive attribute set evaluates to an [attribute set](#attrset), any expression can follow after the `in` keyword.

In the following example we use the `let` expression to form a list:

let
  b = a + 1;
  c = a + b;
  a = 1;
in [ a b c ]

[ 1 2 3 ]

Only expressions within the `let` expression itself can access the newly declared names. We say: the bindings have local scope.

Counter-example:

{
  a = let x = 1; in x;
  b = x;
}

error: undefined variable 'x'

       at «string»:3:7:

            2|   a = let x = 1; in x;
            3|   b = x;
             |       ^
            4| }

### Attribute access[#](#attribute-access "Permalink to this heading")

Attributes in a set are accessed with a dot (`.`) and the attribute name.

Example:

let
  attrset = { x = 1; };
in
attrset.x

1

Accessing nested attributes works the same way.

Example:

let
  attrset = { a = { b = { c = 1; }; }; };
in
attrset.a.b.c

1

The dot (`.`) notation can also be used for assigning attributes.

Example:

{ a.b.c = 1; }

{ a = { b = { c = 1; }; }; }

### `with ...; ...`[#](#with "Permalink to this heading")

The `with` expression allows access to attributes without repeatedly referencing their attribute set.

Example:

let
  a = {
    x = 1;
    y = 2;
    z = 3;
  };
in
with a; [ x y z ]

[ 1 2 3 ]

The expression

with a; [ x y z ]

is equivalent to

[ a.x a.y a.z ]

Attributes made available through `with` are only in scope of the expression following the semicolon (`;`).

Counter-example:

let
  a = {
    x = 1;
    y = 2;
    z = 3;
  };
in
{
  b = with a; [ x y z ];
  c = x;
}

error: undefined variable 'x'

       at «string»:10:7:

            9|   b = with a; [ x y z ];
           10|   c = x;
             |       ^
           11| }

### `inherit ...`[#](#inherit "Permalink to this heading")

`inherit` is shorthand for assigning the value of a name from an existing scope to the same name in a nested scope. It is for convenience to avoid repeating the same name multiple times.

Example:

let
  x = 1;
  y = 2;
in
{
  inherit x y;
}

{ x = 1; y = 2; }

The fragment

inherit x y;

is equivalent to

x = x; y = y;

It is also possible to `inherit` names from a specific attribute set with parentheses (`inherit (...) ...`).

Example:

let
  a = { x = 1; y = 2; };
in
{
  inherit (a) x y;
}

{ x = 1; y = 2; }

The fragment

inherit (a) x y;

is equivalent to

x = a.x; y = a.y;

`inherit` also works inside `let` expressions.

Example:

let
  inherit ({ x = 1; y = 2; }) x y;
in [ x y ]

[ 1 2 ]

Detailed explanation

While this example is contrived, in more complex code you will regularly see nested `let` expressions that re-use names from their outer scope.

Here we use the attribute set `{ x = 1; y = 2; }` to have something non-trivial to inherit from. The `let` expression inherits `x` and `y` from that attribute set using `( )`, which is equivalent to writing:

let
  x = { x = 1; y = 2; }.x;
  y = { x = 1; y = 2; }.y;
in

The new inner scope now contains `x` and `y`, which are used in the list `[ x y ]`.

### String Interpolation `${ ... }`[#](#string-interpolation "Permalink to this heading")

Previously known as “antiquotation”.

The value of a Nix expression can be inserted into a character string with the dollar-sign and braces (`${ }`).

Example:

let
  name = "Nix";
in
"hello ${name}"

"hello Nix"

Only character strings or values that can be represented as a character string are allowed.

Counter-example:

let
  x = 1;
in
"${x} + ${x} = ${x + x}"

error: cannot coerce an integer to a string

       at «string»:4:2:

            3| in
            4| "${x} + ${x} = ${x + x}"
             |  ^
            5|

Interpolated expressions can be arbitrarily nested.

(This can become hard to read, and we recommend to avoid it in practice.)

Example:

let
  a = "no";
in
"${a + " ${a + " ${a}"}"}"

"no no no"

Warning

You may encounter strings that use the dollar sign (`$`) before an assigned name, but no braces (`{ }`):

These are _not_ interpolated strings, but usually denote variables in a shell script.

In such cases, the use of names from the surrounding Nix expression is a coincidence.

Example:

let
  out = "Nix";
in
"echo ${out} > $out"

"echo Nix > $out"

### File system paths[#](#file-system-paths "Permalink to this heading")

The Nix language offers convenience syntax for file system paths.

Absolute paths always start with a slash (`/`).

Example:

/absolute/path

/absolute/path

Paths are relative when they contain at least one slash (`/`) but do not start with one. They evaluate to the path relative to the file containing the expression.

The following examples assume the containing Nix file is in `/current/directory` (or `nix repl` is run in `/current/directory`).

Example:

./relative

/current/directory/relative

Example:

relative/path

/current/directory/relative/path

One dot (`.`) denotes the current directory within the given path.

You will often see the following expression, which specifies a Nix file’s directory.

Example:

./.

/current/directory

Detailed explanation

Since relative paths must contain a slash (`/`) but must not start with one, and the dot (`.`) denotes no change of directory, the combination `./.` specifies the current directory as a relative path.

Two dots (`..`) denote the parent directory.

Example:

../.

/current

Note

Paths can be used in interpolated expressions – an [impure operation](#impurities) we will cover in detail in a [later section](#path-impurities).

#### Search path[#](#search-path "Permalink to this heading")

Also known as “angle bracket syntax”.

Example:

<nixpkgs>

/nix/var/nix/profiles/per-user/root/channels/nixpkgs

The value of a named path is a file system path that depends on the contents of the [`$NIX_PATH`](https://nixos.org/manual/nix/unstable/command-ref/env-common.html?highlight=nix_path#env-NIX_PATH) environment variable.

In practice, `<nixpkgs>` points to the file system path of some revision of [`nixpkgs`](https://github.com/NixOS/nixpkgs), the source repository of Nixpkgs.

For example, `<nixpkgs/lib>` points to the subdirectory `lib` of that file system path:

<nixpkgs/lib>

/nix/var/nix/profiles/per-user/root/channels/nixpkgs/lib

While you will see many such examples, we recommend to [avoid search paths](../../recipes/best-practices.html#search-path) in practice, as they are [impurities](#impurities) which are not reproducible.

### Indented strings[#](#indented-strings "Permalink to this heading")

Also known as “multi-line strings”.

The Nix language offers convenience syntax for character strings which span multiple lines that have common indentation.

Indented strings are denoted by _double single quotes_ (`'' ''`).

Example:

''
multi
line
string
''

"multi\nline\nstring\n"

Equal amounts of prepended white space are trimmed from the result.

Example:

''
  one
   two
    three
''

"one\n two\n  three\n"

## Functions[#](#functions "Permalink to this heading")

Functions are everywhere in the Nix language and deserve particular attention.

A function always takes exactly one argument. Argument and function body are separated by a colon (`:`).

Wherever you see a colon (`:`) in Nix language code:

- On its left is the function argument
    
- On its right is the function body.
    

Function arguments are the third way, apart from [attribute sets](#attrset) and [`let` expressions](#let), to assign names to values. Notably, values are not known in advance: the names are used as placeholders that are filled when [calling a function](#calling-functions).

Function declarations in the Nix language can appear in different forms. Each of them is explained in the following, and here is an overview:

- Single argument
    
    x: x + 1
    
    - Multiple arguments via nesting
        
        x: y: x + y
        
- Attribute set argument
    
    { a, b }: a + b
    
    - With default attributes
        
        { a, b ? 0 }: a + b
        
    - With additional attributes allowed
        
        { a, b, ...}: a + b
        
- Named attribute set argument
    
    args@{ a, b, ... }: a + b + args.c
    
    or
    
    { a, b, ... }@args: a + b + args.c
    

Functions have no names. We say they are anonymous, and call such a function a _lambda_.[[3]](#lambda)

Example:

x: x + 1

<LAMBDA>

The `<LAMBDA>` indicates the resulting value is an anonymous function.

As with any other value, functions can be assigned to a name.

Example:

let
  f = x: x + 1;
in f

<LAMBDA>

### Calling functions[#](#calling-functions "Permalink to this heading")

Also known as “function application”.

Calling a function with an argument means writing the argument after the function.

Example:

let
  f = x: x + 1;
in f 1

2

Example:

let
  f = x: x.a;
in
f { a = 1; }

1

The above example calls `f` on a literal attribute set. One can also pass arguments by name.

Example:

let
  f = x: x.a;
  v = { a = 1; };
in
f v

1

Since function and argument are separated by white space, sometimes parentheses (`( )`) are required to achieve the desired result.

Example:

(x: x + 1) 1

2

Detailed explanation

This expression applies an anonymous function `x: x + 1` to the argument `1`. The function has to be written in parentheses to distinguish it from the argument.

Example:

List elements are also separated by white space, therefore the following are different:

let
 f = x: x + 1;
 a = 1;
in [ (f a) ]

[ 2 ]

let
 f = x: x + 1;
 a = 1;
in [ f a ]

[ <LAMBDA> 1 ]

The first example reads: apply `f` to `a`, and put the result in a list. The resulting list has one element.

The second example reads: put `f` and `a` in a list. The resulting list has two elements.

#### Multiple arguments[#](#multiple-arguments "Permalink to this heading")

Also known as “[curried](https://en.m.wikipedia.org/wiki/Currying) functions”.

Nix functions take exactly one argument. Multiple arguments can be handled by nesting functions.

Such a nested function can be used like a function that takes multiple arguments, but offers additional flexibility.

Example:

x: y: x + y

<LAMBDA>

The above function is equivalent to

x: (y: x + y)

<LAMBDA>

This function takes one argument and returns another function `y: x + y` with `x` set to the value of that argument.

Example:

let
  f = x: y: x + y;
in
f 1

<LAMBDA>

Applying the function which results from `f 1` to another argument yields the inner body `x + y` (with `x` set to `1` and `y` set to the other argument), which can now be fully evaluated.

let
  f = x: y: x + y;
in
f 1 2

3

### Attribute set argument[#](#attribute-set-argument "Permalink to this heading")

Also known as “keyword arguments” or “destructuring” .

Nix functions can be declared to require an attribute set with specific structure as argument.

This is denoted by listing the expected attribute names separated by commas (`,`) and enclosed in braces (`{ }`).

Example:

{a, b}: a + b

<LAMBDA>

The argument defines the exact attributes that have to be in that set. Leaving out or passing additional attributes is an error.

Example:

let
  f = {a, b}: a + b;
in
f { a = 1; b = 2; }

3

Counter-example:

let
  f = {a, b}: a + b;
in
f { a = 1; b = 2; c = 3; }

error: 'f' at (string):2:7 called with unexpected argument 'c'

       at «string»:4:1:

            3| in
            4| f { a = 1; b = 2; c = 3; }
             | ^
            5|

#### Default values[#](#default-values "Permalink to this heading")

Also known as “default arguments”.

Destructured arguments can have default values for attributes.

This is denoted by separating the attribute name and its default value with a question mark (`?`).

Attributes in the argument are not required if they have a default value.

Example:

let
  f = {a, b ? 0}: a + b;
in
f { a = 1; }

1

Example:

let
  f = {a ? 0, b ? 0}: a + b;
in
f { } # empty attribute set

0

#### Additional attributes[#](#additional-attributes "Permalink to this heading")

Additional attributes are allowed with an ellipsis (`...`):

{a, b, ...}: a + b

Unlike in the previous counter-example, passing an argument that contains additional attributes is not an error.

Example:

let
  f = {a, b, ...}: a + b;
in
f { a = 1; b = 2; c = 3; }

3

### Named attribute set argument[#](#named-attribute-set-argument "Permalink to this heading")

Also known as “@ pattern”, “@ syntax”, or “‘at’ syntax”.

An attribute set argument can be given a name to be accessible as a whole.

This is denoted by prepending or appending the name to the attribute set argument, separated by the at sign (`@`).

Example:

{a, b, ...}@args: a + b + args.c

<LAMBDA>

or

args@{a, b, ...}: a + b + args.c

<LAMBDA>

Example:

let
  f = {a, b, ...}@args: a + b + args.c;
in
f { a = 1; b = 2; c = 3; }

6

## Function libraries[#](#function-libraries "Permalink to this heading")

In addition to the [built-in operators](https://nixos.org/manual/nix/stable/language/operators.html) (`+`, `==`, `&&`, etc.), there are two widely used libraries that _together_ can be considered standard for the Nix language. You need to know about both to understand and navigate Nix language code.

We recommend to at least skim them to familiarise yourself with what is available.

### `builtins`[#](#builtins "Permalink to this heading")

Also known as “primitive operations” or “primops”.

Nix comes with many functions that are built into the language. They are implemented in C++ as part of the Nix language interpreter.

Note

The Nix manual lists all [Built-in Functions](https://nixos.org/manual/nix/stable/language/builtins.html), and shows how to use them.

These functions are available under the `builtins` constant.

Example:

builtins.toString

<PRIMOP>

#### `import`[#](#import "Permalink to this heading")

Most built-in functions are only accessible through `builtins`. A notable exception is `import`, which is also available at the top level.

`import` takes a path to a Nix file, reads it to evaluate the contained Nix expression, and returns the resulting value. If the path points to a directory, the file `default.nix` in that directory is used instead.

Example:

$ echo 1 + 2 > file.nix

import ./file.nix

3

Detailed explanation

The preceding shell command writes the contents `1 + 2` to the file `file.nix` in the current directory.

The above Nix expression refers to this file as `./file.nix`. `import` reads the file and evaluates to the contained Nix expression.

It is an error if the file system path does not exist.

After reading `file.nix` the Nix expression is equivalent to the file contents:

1 + 2

3

Since a Nix file can contain any Nix expression, `import`ed functions can be applied to arguments immediately.

That is, whenever you see additional tokens after a call to `import`, the value it returns should be a function, and anything that follows are arguments to that function.

Example:

$ echo "x: x + 1" > file.nix

import ./file.nix 1

2

Detailed explanation

The preceding shell command writes the contents `x: x + 1` to the file `file.nix` in the current directory.

The above Nix expression refers to this file as `./file.nix`. `import ./file.nix` reads the file and evaluates to the contained Nix expression.

It is an error if the file system path does not exist.

After reading the file, the Nix expression `import ./file.nix` is equivalent to the file contents:

(x: x + 1) 1

2

This applies the function `x: x + 1` to the argument `1`, and therefore evaluates to `2`.

Note

Parentheses are required to separate function declaration from function application.

### `pkgs.lib`[#](#pkgs-lib "Permalink to this heading")

The [`nixpkgs`](https://github.com/NixOS/nixpkgs) repository contains an attribute set called [`lib`](https://github.com/NixOS/nixpkgs/blob/master/lib/default.nix), which provides a large number of useful functions. They are implemented in the Nix language, as opposed to [`builtins`](#builtins), which are part of the language itself.

Note

The Nixpkgs manual lists all [Nixpkgs library functions](https://nixos.org/manual/nixpkgs/stable/#sec-functions-library).

These functions are usually accessed through `pkgs.lib`, as the Nixpkgs attribute set is given the name `pkgs` by convention.

Example:

let
  pkgs = import <nixpkgs> {};
in
pkgs.lib.strings.toUpper "search paths considered harmful"

SEARCH PATHS CONSIDERED HARMFUL

Detailed explanation

This is a more complex example, but by now you should be familiar with all its components.

The name `pkgs` is declared to be the expression `import`ed from some file. That file’s path is determined by the value of the search path `<nixpkgs>`, which in turn is determined by the `$NIX_PATH` environment variable at the time this expression is evaluated. As this expression happens to be a function, it requires an argument to evaluate, and in this case passing an empty attribute set `{}` is sufficient.

Now that `pkgs` is in scope of `let ... in ...`, its attributes can be accessed. From the Nixpkgs manual one can determine that there exists a function under [`lib.strings.toUpper`](https://nixos.org/manual/nixpkgs/stable/#function-library-lib.strings.toUpper).

For brevity, this example uses a search path to obtain _some version_ of Nixpkgs. The function `toUpper` is trivial enough that we can expect it not to produce different results for different versions of Nixpkgs. Yet, more sophisticated software is likely to suffer from such problems. A fully reproducible example would therefore look like this:

let
  nixpkgs = fetchTarball https://github.com/NixOS/nixpkgs/archive/06278c77b5d162e62df170fec307e83f1812d94b.tar.gz;
  pkgs = import nixpkgs {};
in
pkgs.lib.strings.toUpper "always pin your sources"

ALWAYS PIN YOUR SOURCES

See [Towards reproducibility: pinning Nixpkgs](towards-reproducibility-pinning-nixpkgs.html#pinning-nixpkgs) for details.

What you will also often see is that `pkgs` is passed as an argument to a function. By convention one can assume that it refers to the Nixpkgs attribute set, which has a `lib` attribute:

{ pkgs, ... }:
pkgs.lib.strings.removePrefix "no " "no true scotsman"

<LAMBDA>

To make this function produce a result, you can write it to a file (e.g. `file.nix`) and pass it an argument through `nix-instantiate`:

$ nix-instantiate --eval file.nix --arg pkgs 'import <nixpkgs> {}'
"true scotsman"

Oftentimes you will see in NixOS configurations, and also within Nixpkgs, that `lib` is passed directly. In that case one can assume that this `lib` is equivalent to `pkgs.lib` where only `pkgs` is available.

Example:

{ lib, ... }:
let
  to-be = true;
in
lib.trivial.or to-be (! to-be)

<LAMBDA>

To make this function produce a result, you can write it to a file (e.g. `file.nix`) and pass it an argument through `nix-instantiate`:

$ nix-instantiate --eval file.nix --arg lib '(import <nixpkgs> {}).lib'
true

Sometimes both `pkgs` and `lib` are passed as arguments. In that case, one can assume `pkgs.lib` and `lib` to be equivalent. This is done to improve readability by avoiding repeated use of `pkgs.lib`.

Example:

{ pkgs, lib, ... }:
# ... multiple uses of `pkgs`
# ... multiple uses of `lib`

For historical reasons, some of the functions in `pkgs.lib` are equivalent to [`builtins`](#builtins) of the same name.

## Impurities[#](#impurities "Permalink to this heading")

So far we have only covered what we call _pure expressions_: declaring data and transforming it with functions.

In practice, describing build tasks requires observing the outside world.

There is only one impurity in the Nix language that is relevant here: reading files from the file system as _build inputs_

Build inputs are files that build tasks refer to in order to describe how to derive new files. When run, a build task will only have access to explicitly declared build inputs.

The only way to specify build inputs in the Nix language is explicitly with:

- File system paths
    
- Dedicated functions.
    

Nix and the Nix language refer to files by their content hash. If file contents are not known in advance, it’s unavoidable to read files during expression evaluation.

Note

Nix supports other types of impure expressions, such as [search paths](../../recipes/best-practices.html#search-path) or the constant [`builtins.currentSystem`](https://nixos.org/manual/nix/stable/language/builtin-constants.html#builtins-currentSystem). We do not cover those here in more detail, as they do not matter for how the Nix language works in principle, and because they are discouraged for the very reason of breaking reproducibility.

### Paths[#](#paths "Permalink to this heading")

Whenever a file system path is used in [string interpolation](#string-interpolation), the contents of that file are copied to a special location in the file system, the _Nix store_, as a side effect.

The evaluated string then contains the Nix store path assigned to that file.

Example:

$ echo 123 > data

"${./data}"

"/nix/store/h1qj5h5n05b5dl5q4nldrqq8mdg7dhqk-data"

Detailed explanation

The preceding shell command writes the characters `123` to the file `data` in the current directory.

The above Nix expression refers to this file as `./data` and converts the file system path to an [interpolated string](#string-interpolation) `${ ... }`.

Such interpolated expressions must evaluate to something that can be represented as a character string. A file system path is such a value, and its character string representation is the corresponding Nix store path:

/nix/store/<hash>-<name>

The Nix store path is obtained by taking the hash of the file’s contents (`<hash>`) and combining it with the file name (`<name>`). The file is copied into the Nix store directory `/nix/store` as a side effect of evaluation. It is an error if the file system path does not exist.

For directories the same thing happens: The entire directory (including nested files and directories) is copied to the Nix store, and the evaluated string becomes the Nix store path of the directory.

### Fetchers[#](#fetchers "Permalink to this heading")

Files to be used as build inputs do not have to come from the file system.

The Nix language provides built-in impure functions to fetch files over the network during evaluation:

- [builtins.fetchurl](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchurl)
    
- [builtins.fetchTarball](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchTarball)
    
- [builtins.fetchGit](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchGit)
    
- [builtins.fetchClosure](https://nixos.org/manual/nix/stable/language/builtins.html#builtins-fetchClosure)
    

These functions evaluate to a file system path in the Nix store.

Example:

builtins.fetchurl "https://github.com/NixOS/nix/archive/7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"

"/nix/store/7dhgs330clj36384akg86140fqkgh8zf-7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"

Some of them add extra convenience, such as automatically unpacking archives.

Example:

builtins.fetchTarball "https://github.com/NixOS/nix/archive/7c3ab5751568a0bc63430b33a5169c5e4784a0ff.tar.gz"

"/nix/store/d59llm96vgis5fy231x6m7nrijs0ww36-source"

Note

The Nixpkgs manual on [Fetchers](https://nixos.org/manual/nixpkgs/stable/#chap-pkgs-fetchers) lists numerous additional library functions to fetch files over the network.

It is an error if the network request fails.

## Derivations[#](#derivations "Permalink to this heading")

A build task in Nix is called a _derivation_.

Build tasks are at the core of both Nix and the Nix language:

- The Nix language is used to describe build tasks.
    
- Nix runs build tasks to produce _build results_.
    
- Build results can in turn be used as inputs for other build tasks.
    

The Nix language primitive to declare a build task is the built-in impure function `derivation`.

It is usually wrapped by the Nixpkgs build mechanism `stdenv.mkDerivation`, which hides much of the complexity involved in non-trivial build procedures.

Note

You will probably never encounter `derivation` in practice.

Whenever you see `mkDerivation`, it denotes something that Nix will eventually _build_.

Example: [a package using `mkDerivation`](#mkderivation-example)

The evaluation result of `derivation` (and `mkDerivation`) is an [attribute set](#attrset) with a certain structure and a special property: It can be used in [string interpolation](#string-interpolation), and in that case evaluates to the Nix store path of its build result.

Example:

let
  pkgs = import <nixpkgs> {};
in "${pkgs.nix}"

"/nix/store/sv2srrjddrp2isghmrla8s6lazbzmikd-nix-2.11.0"

Note

Your output may differ. It may produce a different hash or even a different package version.

A derivation’s output path is fully determined by its inputs, which in this case come from _some_ version of Nixpkgs.

This is why we recommend to [avoid search paths](../../recipes/best-practices.html#search-path) to ensure predictable outcomes, except in examples intended for illustration only.

Detailed explanation

The example imports the Nix expression from the search path `<nixpkgs>`, and applies the resulting function to an empty attribute set `{}`. Its output is assigned the name `pkgs`.

Converting the attribute `pkgs.nix` to a string with [string interpolation](#string-interpolation) is allowed, as `pkgs.nix` is a derivation. That is, ultimately `pkgs.nix` boils down to a call to `derivation`.

The resulting string is the file system path where the build result of that derivation will end up.

There is more depth to the inner workings of derivations, but at this point it should be enough to know that such expressions evaluate to Nix store paths.

String interpolation on derivations is used to refer to other build results as file system paths when declaring new build tasks.

This allows constructing arbitrarily complex compositions of derivations with the Nix language.

## Worked examples[#](#worked-examples "Permalink to this heading")

So far we have seen artifical examples illustrating the various constructs in the Nix language.

You should now be able to read Nix language code for simple packages and configurations, and come up with similiar explanations of the following practical examples.

Note

The goal of the following exercises is not to understand what the code means or how it works, but how it is structured in terms of functions, attribute sets, and other Nix language data types.

### Shell environment[#](#shell-environment "Permalink to this heading")

{ pkgs ? import <nixpkgs> {} }:
let
  message = "hello world";
in
pkgs.mkShell {
  buildInputs = with pkgs; [ cowsay ];
  shellHook = ''
    cowsay ${message}
  '';
}

This example declares a shell environment (which runs the `shellHook` on initialization).

Explanation:

- This expression is a function that takes an attribute set as an argument.
    
- If the argument has the attribute `pkgs`, it will be used in the function body. Otherwise, by default, import the Nix expression in the file found on the search path `<nixpkgs>` (which is a function in this case), call the function with an empty attribute set, and use the resulting value.
    
- The name `message` is bound to the string value `"hello world"`.
    
- The attribute `mkShell` of the `pkgs` set is a function that is passed an attribute set as argument. Its return value is also the result of the outer function.
    
- The attribute set passed to `mkShell` has the attributes `buildInputs` (set to a list with one element: the `cowsay` attribute from `pkgs`) and `shellHook` (set to an indented string).
    
- The indented string contains an interpolated expression, which will expand the value of `message` to yield `"hello world"`.
    

### NixOS configuration[#](#nixos-configuration "Permalink to this heading")

{ config, pkgs, ... }: {

  imports = [ ./hardware-configuration.nix ];

  environment.systemPackages = with pkgs; [ git ];

  # ...

}

This example is (part of) a NixOS configuration.

Explanation:

- This expression is a function that takes an attribute set as an argument. It returns an attribute set.
    
- The argument must at least have the attributes `config` and `pkgs`, and may have more attributes.
    
- The returned attribute set contains the attributes `imports` and `environment`.
    
- `imports` is a list with one element: a path to a file next to this Nix file, called `hardware-configuration.nix`.
    
    Note
    
    `imports` is not the impure built-in `import`, but a regular attribute name!
    
- `environment` is itself an attribute set with one attribute `systemPackages`, which will evaluate to a list with one element: the `git` attribute from the `pkgs` set.
    
- The `config` argument is not (shown to be) used.
    

### Package[#](#package "Permalink to this heading")

{ lib, stdenv }:

stdenv.mkDerivation rec {

  pname = "hello";

  version = "2.12";

  src = builtins.fetchTarball {
    url = "mirror://gnu/${pname}/${pname}-${version}.tar.gz";
    sha256 = "1ayhp9v4m4rdhjmnl2bq3cibrbqqkgjbl3s7yk2nhlh8vj3ay16g";
  };

  meta = with lib; {
    license = licenses.gpl3Plus;
  };

}

This example is a (simplified) package declaration from Nixpkgs.

Explanation:

- This expression is a function that takes an attribute set which must have exactly the attributes `lib` and `stdenv`.
    
- It returns the result of evaluating the function `mkDerivation`, which is an attribute of `stdenv`, applied to a recursive set.
    
- The recursive set passed to `mkDerivation` uses its own `pname` and `version` attributes in the argument to the built-in function `fetchTarball`.
    
- The `meta` attribute is itself an attribute set, where the `license` attribute has the value that was assigned to the nested attribute `lib.licenses.gpl3Plus`.
    

## References[#](#references "Permalink to this heading")

- [Nix manual: Nix language](https://nixos.org/manual/nix/stable/language/index.html)
    
- [Nix manual: String interpolation](https://nixos.org/manual/nix/stable/language/string-interpolation.html)
    
- [Nix manual: Built-in Functions](https://nixos.org/manual/nix/stable/language/builtins.html)
    
- [Nix manual: `nix repl`](https://nixos.org/manual/nix/stable/command-ref/new-cli/nix3-repl.html)
    
- [Nixpkgs manual: Functions reference](https://nixos.org/manual/nixpkgs/stable/#sec-functions-library)
    
- [Nixpkgs manual: Fetchers](https://nixos.org/manual/nixpkgs/stable/#chap-pkgs-fetchers)
    

## Next steps[#](#next-steps "Permalink to this heading")

### Get things done[#](#get-things-done "Permalink to this heading")

- [Declarative and reproducible developer environments](declarative-and-reproducible-developer-environments.html#declarative-reproducible-envs) – create reproducible shell environments from a Nix file
    
- [Garbage Collection](https://nixos.org/manual/nix/stable/package-management/garbage-collection.html) – remove unused build results from the Nix store
    

### Learn more[#](#learn-more "Permalink to this heading")

If you worked through the examples, you will have noticed that reading the Nix language reveals the structure of the code, but does not necessarily tell what the code actually means.

Often it is not possible to determine from the code at hand

- the data type of a named value or function argument.
    
- the data type a called function accepts for its argument.
    
- which attributes are present in a given attribute set.
    

Example:

{ x, y, z }: (x y) z.a

How do we know…

- that `x` will be a function that, given an argument, returns a function?
    
- that, given `x` is a function, `y` will be an appropriate argument to `x`?
    
- that, given `(x y)` is a function, `z.a` will be an appropriate argument to `(x y)`?
    
- that `z` will be an attribute set at all?
    
- that, given `z` is an attribute set, it will have an attribute `a`?
    
- which data type `y` and `z.a` will be?
    
- the data type of the end result?
    

And how does the caller of this function know that it requires an attribute set with attributes `x`, `y`, `z`?

Answering such questions requires knowing the context in which a given expression is supposed to be used.

The Nix ecosystem and code style is driven by conventions. Most names you will encounter in Nix language code come from Nixpkgs:

- [Nix Pills](https://nixos.org/guides/nix-pills/) - a detailed explanation of derivations and how Nixpkgs is constructed from first principles
    

Nixpkgs provides generic build mechanisms that are widely used:

- [`stdenv`](https://nixos.org/manual/nixpkgs/stable/#chap-stdenv) - most importantly `mkDerivation`
    
- [Trivial Builders](https://nixos.org/manual/nixpkgs/stable/#chap-trivial-builders) - to create files and shell scripts
    

Packages from Nixpkgs can be modified through multiple mechanisms:

- [overrides](https://nixos.org/manual/nixpkgs/stable/#chap-overrides) – specifically `override` and `overrideAttrs` to modify single packages
    
- [overlays](https://nixos.org/manual/nixpkgs/stable/#chap-overlays) – to produce a custom variant of Nixpkgs with individually modified packages
    

Different language ecosystems and frameworks have different requirements to accommodating them into Nixpkgs:

- [Languages and frameworks](https://nixos.org/manual/nixpkgs/stable/#chap-language-support) lists tools provided by Nixpkgs to build language- or framework-specific packages with Nix.
    

The NixOS Linux distribution has a modular configuration system that imposes its own conventions:

- [NixOS modules](https://nixos.org/manual/nixos/stable/index.html#sec-writing-modules) shows how NixOS configurations are organized.
    

---

[[1](#id1)]

Details: [Nix manual - attribute set](https://nixos.org/manual/nix/stable/language/values.html#attribute-set)

[[2](#id2)]

Details: [Nix manual - list](https://nixos.org/manual/nix/stable/language/values.html#list)

[[3](#id9)]

The term _lambda_ is a shorthand for [lambda abstraction](https://en.wikipedia.org/wiki/Lambda_calculus#lambdaAbstr) in the [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus).

[

previous

Reproducible interpreted scripts



](reproducible-scripts.html "previous page")[

next

Towards reproducibility: pinning Nixpkgs

](towards-reproducibility-pinning-nixpkgs.html "next page")

Contents

- [Overview](#overview)
    - [What will you learn?](#what-will-you-learn)
    - [What do you need?](#what-do-you-need)
    - [How long does it take?](#how-long-does-it-take)
    - [How to run the examples?](#how-to-run-the-examples)
        - [Interactive evaluation](#interactive-evaluation)
        - [Evaluating Nix files](#evaluating-nix-files)
    - [Notes on whitespace](#notes-on-whitespace)
- [Names and values](#names-and-values)
    - [Attribute set `{ ... }`](#attribute-set)
        - [Recursive attribute set `rec { ... }`](#recursive-attribute-set-rec)
    - [`let ... in ...`](#let-in)
    - [Attribute access](#attribute-access)
    - [`with ...; ...`](#with)
    - [`inherit ...`](#inherit)
    - [String Interpolation `${ ... }`](#string-interpolation)
    - [File system paths](#file-system-paths)
        - [Search path](#search-path)
    - [Indented strings](#indented-strings)
- [Functions](#functions)
    - [Calling functions](#calling-functions)
        - [Multiple arguments](#multiple-arguments)
    - [Attribute set argument](#attribute-set-argument)
        - [Default values](#default-values)
        - [Additional attributes](#additional-attributes)
    - [Named attribute set argument](#named-attribute-set-argument)
- [Function libraries](#function-libraries)
    - [`builtins`](#builtins)
        - [`import`](#import)
    - [`pkgs.lib`](#pkgs-lib)
- [Impurities](#impurities)
    - [Paths](#paths)
    - [Fetchers](#fetchers)
- [Derivations](#derivations)
- [Worked examples](#worked-examples)
    - [Shell environment](#shell-environment)
    - [NixOS configuration](#nixos-configuration)
    - [Package](#package)
- [References](#references)
- [Next steps](#next-steps)
    - [Get things done](#get-things-done)
    - [Learn more](#learn-more)

By the nix.dev contributors.

© Copyright 2016-2023, NixOS Foundation.