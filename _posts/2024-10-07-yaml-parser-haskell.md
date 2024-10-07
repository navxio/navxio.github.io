---
title: "Learning haskell by building a yaml parser"
date: 07/10/2024
categories:
  - blog
tags:
  - parser
  - haskell
  - yaml
  - diy
---

I've been learning haskell for some time now and it has been amazing grappling with pure functional programming concepts. Although it's not for everyone, I believe its potential is too much in terms purity.

The best way for me to learn anything is to just build something and i'm quite infatuated with yaml right now, so I suppose i'll build a very basic yaml parser to learn about parsing concepts and how haskell helps with its functional prowess. Since the markup language has just data and no instructions, it's suited for a recursive data definition which Haskell excels in, among other things.

###### Prelude

> A markup language is designed for humans to show data in human readable format while being machine compatible reliably. Yaml stands for yet another markup language and is quite simple. Here's an example

```
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

This small example shows what makes up yaml's constituents - scalar values, mappings and sequences.

##### Interface

Let's say we create yamlParser.hs and this is what it's going to look like when it's done.

```
import parse, YamlVal from yamlParser

input_str = "- item1\n- item2\nkey: value"

val = parse(input_str)
```

We have to implement the `parse` function that takes an arbitrary string and converts it to YamlVal, which is a native custom datatype we're gonna define as part of the parser. Let's dive in

Step 1: define the custom data type that represents a yaml value in our program

```
data YamlVal
	= YamlScalar String
	| YamlSequence [YamlVal]
	| YamlMapping [(String, YamlVal)]
	deriving (Show, Eq)
```

This data type will take care of our basic example parsing as described above. (Side note: We can define numbers / bools etc as YamlScalars as actual yaml does but we'll get to that after the basic implementation). The Show and Eq typeclasses are derived from for obvious reasons.

Now that we have the 'interface' and definition of structure to be parsed, let's go on implementing a parser. Ordinarily, one would use dedicated library (say parsec / megaparsec) but since the point is to learn how the parser works, we're gonna implement the parsing by hand.

##### Parsing functions

Since we have defined our custom yaml data type to be comprised of scalars, sequences, and mappings, we have to write 3 functions separately that handle the respective scenarios and later compose them into a parent parser function.

###### Parsing Scalars

Let's start with parsing scalars. Scalars, for now, are only strings, so it should be easy. Here's an implementation

```
parseScalar :: String -> YamlVal
parseScalar s =
	let (scalar, _) = break (== '\n') s
	in (YamlScalar scalar)
```

We've considered that every scalar is delimited by a new line, so we're gonna split the input string by `\n` as split index, and throw away the rest of the string.

We can define a helper function `trim` which trims the whitespace around the scalar if that is not desired-

```
trim :: String -> String
trim = t . t
	where t = reverse . dropWhile (== ' ')
```

This function removes the white space from the front / end of the string. So let's add trim to our definiton of parseScalar

```
parseScalar :: String -> YamlVal
parseScalar s =
	let (scalar, _) = break (== '\n') s
	in (YamlScalar trim scalar)
```

###### Parsing Sequences

Sequences in yaml are basically list of yaml values (YamlVal in our definition). Let's take a dig

```
parseSequence :: String -> YamlVal

parseSequence s = let items = parseItems s in YamlSequence items
```

Define a helper function `parseItems` that'll do the heavy lifting for processing the input string recursively for items.

```
parseItems :: String -> [YamlVal]
parseItems [] = [] -- strings are but list of chars in Haskell
parseItems (x:xs)
| x == '-' = -- line starts with - means it's a list item
	let (scalar, rest) = break (==  '\n') xs
	in (YamlVal (trim scalar) : parseItems (drop 1 rest))
| otherwise = []
```

Pretty straightforward. We break the string at every new line, inspect the first char as - or not, and act accordingly. This is where the power of descriptive haskell takes reins.

###### Parsing Key Val Mapping

In yaml, the mappings have a key followed by color (:) and a value. We can easily write a recursive parsing function for the same. Following the previous recipe

```
parseMapping :: String -> YamlVal
parseMapping s = let pairs = parsePairs s in YamlMapping pairs
```

and the attendant helper function `parsePairs`

```
parsePairs :: String -> [(String, YamlVal)]
parsePairs [] = []
parsePairs line:lines =
	| ':' `elem` line =
	let (key, value) = break (== ':') line
		trimmedKey = trim key
		trimmedVal = trim (drop 1 value) -- drop the :
		in (trimmedKey, YamlVal trimmedVal) : parsePairse lines
	| otherwise = parsePairs lines
```

Now we have three different functions that parse different components of yaml. How to put them together? The most obvious way is to

```
parseYaml :: String -> YamlVal

parseYaml input
| isSequence input = parseSequence input
| isMapping input = parseMapping input
| otherwise = parseScalar input
```

with `isSequence` , `isMapping` as helper functions. But there are some limitations - this function cannot handle mixed input / nested structures. How to go about it? This brings us to the nested part of the yaml structure, a hint of which is there in our data definition for YamlVal which is recursive.

To be continued..
