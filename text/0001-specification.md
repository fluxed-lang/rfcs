# Specification

- Start Date: 2023-01-10
- RFC PR: fluxed-lang/rfcs#1
- Flux Issue:

## Contents

1. [Summary](#summary)
2. [Syntax](#syntax)

## Summary

This RFC outlines the Flux language specification - that is the grammar and basic language semantics.

Flux is a general purpose, multi-paradigm programming language, designed to be an intermediary between a language such as Python, and a lower-level language like Rust. It has a number of features:

- Currying
- Polymorphism

## Syntax

### Literals

Flux supports a number of common literal types:

- Integers
- Floats
- Strings
- Characters
- Booleans
- Arrays
- Tuples

#### Numeric Grammar

```ebnf
binary digit = "0" | "1";
octal digit = binary digit | "2" | "3" | "4" | "5" | "6" | "7";
denary digit = octal digit | "8" | "9";
hex digit = denary digit | "A" | "B" | "C" | "D" | "E" | "F" | "a" | "b" | "c" | "d" | "e" | "f";

binary integer = [ "-" ], "0b", binary digit, { digit | "_" };
octal integer = [ "-" ], "0o", octal digit, { octal digit | "_" };
denary integer = [ "-" ], [ "0d" ], denary digit, { denary digit | "_" };
hex integer = [ "-" ], [ "0x" ], hex digit, { hex digit | "_" };

integer = binary integer | octal integer | denary integer | hex integer;

exponent = "e", denary integer;
float = denary integer, ".", denary digit, { denary digit | "_" }, [ exponent ];
```

#### String Grammar

```ebnf
inner char = 
	alphanumeric 
	| "\t"
	| "\n"
	| "\r"
	| "\u", hexadecimal digit, { hexadecimal digit };

char =  "'", char, "'";
string = '"', { inner char } ], '"';
```

#### Array Grammar

```ebnf
atom = "(", expr ")" | literal;
empty array = "[]";
array = empty array | "[", { atom, "," }, atom "]";
```

#### Tuple Grammar

```ebnf
atom = "(", expr ")" | literal;
unit tuple = "()";
tuple = unit tuple | "(", { atom, "," }, atom ")";
```

### Binary Expressions

Flux can perform binary operations on variables and literals.

```ebnf
atom =  "(", expr ")" | literal;

div = atom, { "/", atom };
mul = div, { "*", div };
add = mul, { "+", mul };
sub = sub, { "-" };

binary expr = sub;
```

#### Examples

```flux
1 + 2
# precedence
3 + 4 * 5
(1 + 2) * 3

# comparison and inequality
1 == 2
3 < 4
4 > 5

# using variables
a + b
a + b * c
```

### Control Flow

Flux supports the common control flow statements defined in most imperative programming languages:

#### Conditional Grammar

```ebnf
block = "{", statement, "}";
condition = "(", expr, ")" | binary expression;

if statement = "if",  condition, block;
else if statement = "else", if statement;
else statement = "else", block;

conditional = if statement, { else if statement }, [ else statement ]; 
```

#### Loop Grammar

```ebnf
loop = "loop", block;
while loop = "while", condition, block;
```

> It is worth noting that all control flow statements, with the exceptions of `return` and `break`, are expressions.

#### Examples

```flux
if a < b { 2 } else { 3 }
if a < b {
	let c = a + b
	c - 2
} else {
	let c = a - b
	c + 2
}
```

### Type Expressions

Flux's type system is designed to be as expressive as possible, akin to that of TypeScript. It features type expressions, generics, constraints, etc.

#### Type Expression Grammar

```ebnf
identifier = alphabetical character, { alphanumeric character | "_" };

atom = identifier | "(", type expression , ")";

unit = "()";
tuple type = unit | "(", { atom, "," }, atom, ")";

array type = type expression, "[]";

record key = identifier
	| "[", identifier, "in", type expression, "]";
record entry = record key, ":", type expression;
empty record type = "{}";
record type = empty record type | "{", { record entry, "," }, record entry "}";

type intersection = atom, { "&", atom };
type union = type intersection, { "|", type intersection };
type operation = type union;

type expression = identifier
	| tuple type
	| array type
	| record type
	| type operation;
```
