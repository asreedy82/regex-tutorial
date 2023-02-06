# Regex Tutorial

This tutorial will explain what regular expressions, or regex, are and how they work by walking through an example. The example we will use is a regext to match with a URL - `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`. We will explain each component of the regex with the purpose of giving you the tools to understand other regex and even make your own.

## Summary

The example we are using is a regext to match or find values that are URLs. For example, if you had long text and wanted to find all URLs that were in the text, this regex would do that. It also accounts for different ways URLs can be written, such as with `https://` in the beginning or without. We will explain the components of a regex with our example to help you, the reader, understand other regex examples and even create your own.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

Our example regex that will match with any URL - `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`
Notice that it starts and ends with the `/` character. Wrapping a regex with the `/` is a requirement in order for it to be interpreted as a regex.

### Anchors

Anchors represent the beginning and end of the regex. The `^` character begins the regex and means that the characters to be matched with will follow immediately. The `$` represents the end of the string. As you can see in our example, `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`, the very next character after our `/` is the `^` and the last character before the end `/` is the `$`.

### Quantifiers

Quantifiers determine how many instances can be matched. There are several characters used as quantifiers
- `*` which means the pattern will match 0 or more times
- `+` which means the pattern will match 1 or more times
- `?` which means the pattern will match zero or 1 time (also known as "optional")
- `{}` which give you some customization for matching
    - `{x}` 'x' alone will mean the pattern will be matched exactly `x` number of times
    - `{x, }` 'x,' will match `x` or more times
    - `{x, y}` will match at least `x` times but no more than `y` times

And as quantifiers are inherently 'greedy' and will match as many instances of the pattern as possible, you can add a `?` to the end of each of these quantifiers to limit the number of instances matched to fewest possible.

In our example, we see all quantifiers in use: `*`, `+`, `?`, and `{}`. The first appearance is `https?` and basically says the `s` is optional. Some websites may still be http rather than https, so both would match with our regex. Then we have another `?` outside the parenthesis of `(https?:\/\/)?` which makes the whole `https://` or `http://` optional. For example, all three of these could match: 
- `https://www.google.com` 
- `www.google.com`
- `google.com`

The next quantifier is the `+` in the next parenthesis, `([\da-z\.-]+)`. In this case, the pattern can occur 1 or more times. Basically, there needs to be at least 1 or more number, lowercase letter, `.`, or `-`.

Next we see `{2,6}` immediately following the second bracket, `[a-z\.]`. This means there has to be at least 2 occurences of some combination of lowercase letters and/or a `.` but no more than 6. The purpose of this quantifier is to account for various types of top-level domains such as `.com`, `.gov`, or `.edu`. Top-level domains, or TLD's, are usually at least 2 characters long but no more than 6.
These would match:
- `tv`
- `io`
- `.biz`

But these would not match:
- `.` because it is less than 2 characters
- `abcdef.` because it is longer than 6 characters
- `1xyz` because it contains a number

The next qualifiers we see are two `*` characters. The first one applies to the third bracket in the regex, `[\/\w \.-]`. In this case, 0 or more instances of combinations including `/`, any upper or lowercase letter, any number, any whitespace character, `_`, `.`, or `-`. This basically accounts for the directory of the URL, or the stuff that follows a `/`. The second `*` applies to the entire expression inside the parenthesis, meaning the pattern in the parenthesis can occur 0 or more times and still match.
Examples of what can match:
- `/abcde/`
- `//_- ab/`
- `123 abc`

What won't match:
- `$abc/` because `$` is not an accepted character
- `/whathappened?/` because `?` is not an accepted character

And finally we have one more `?` quantifier that is preceded by a `/`. This basically will match whether a URL ends with `/` or doesn't.


### Grouping Constructs
Grouping constructs make use of parenthesis, `()`, with more complex expressions to enable checking of different sections of the expression. Since a URL is made up of a few sections, grouping constructs are necessary. In fact there are 4 sections that leverage grouping constructs in this expression.

The first is the `https://` portion, `(https?:\/\/)`

Next is `([\da-z\.-]+)` which could be the `www` in `www.aol.com` or it could just be `aol`. 

The third grouping is what represents the top-level domain section, or the `.com`, `.edu`, etc. `([a-z\.]{2,6})` Given that it is after a required `.`, and it can only be between 2 and 6 characters, it accounts for all of the possible TLD's.

Last is the section that may or may not come after the `.com`. This accounts for directories or deep linking into a site. `([\/\w \.-]*)` and the `*` at the end makes it optional as well as can occur many more times.

### Bracket Expressions
Inside brackets, `[]`, are the bracket expressions, or the range of characters we want to match. When we have characters inside of brackets, our regex will match any of them (depending on quantifiers). Our URL regex has 3 instances of bracket expressions.

The first is `[\da-z\.-]`, which means any number, lowercase letter, `.`, or `-` will match.
These will match:
- `abc`
- `123abce-`
- `1.a.b`

These will not match:
- `ABC` because they are capital letters
- `abc 123` because there is a white space

The second bracket expression is similar, `[a-z\.]`. The key difference is numbers and the `-` are not allowed.

And last is `[\/\w \.-]`, which will match any "word" (which includes numbers, lower and upper case letters, or `_`), or any number of `/`, `.`, or `-`.

### Character Classes
Character classes are used to define sets of characters. These are kind of like shortcuts. For example `.` by itself represents any character other than a new line or `\n`. And `/D` is any non digit, or any character other than `[0-9]`. We use a couple of these in our URL example.

`/d` represents any digit, or `[0-9]`. In our example we have `[\da-z\.-]`
`/w` represents any "word". As mentioned above, a "word" can by any combination of number, upper or lowercase letter, or the `_` character. It is much eaier to write `/w` than `[0-9a-zA-z_]`

### The OR Operator
The OR operator, `|`, gives flexibility if there are multiple cases of what you want to match. Our example doesn't use the OR, but it can be very useful. Say you wanted to find a string that was either all lowercase letters or all numbers. You could use `[a-z]|[0-9]`. `abc` would match as well as `123`, but `a2c` would not match.

### Flags
Flags are the one character that can exist outside of the beginning and ending `/` of the expression. They happen at the very end and are used to define any additional functionality of the expression. One example is ending with `i` which would override any case-sensitivity inside the expression. It basically removes case-sensitivity altogether so that even with `[a-z]` a string like `ABC` would still match. Our expression doesn't use a flag.

### Character Escapes
Character escapes are very important to help decipher between literals and character classes or other characters that are part of components of the regex. The character escape is signified by a backslash, `\`. For example, since the `()` already have a job of being the grouping construct, if you wanted to match against a `(` in an expression, you would need to "escape" it like this `\(`. Our example uses many of these. In the very beginning we see two of the for the forward slashes in `https://`, so we have `\/\/`. We use escapes for each of our character classes such as `\d`, `\.`, and `\w`.


## Author
I am currently a coding bootcamp student through the University of Miami. After 10+ years in digital advertising, I decided to shift my career towards the tech side and I'm learning to be a full-stack web developer. To see more of my work, check out my GitHub profile.
[GitHub Profile](https://github.com/asreedy82)