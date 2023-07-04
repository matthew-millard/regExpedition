# Email Validation 101: A Comprehensive Guide to Regular Expressions

In this tutorial, we are going to dive into the world of regular expressions (RegEx) and learn how to validate email addresses.

## Summary

One common use case for RegEx is validating user input, such as email addresses.
I'll specifically cover the pattern `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`, which is designed to match standard email addresses.
This tutorial will break down the components of this regular expression, and demonstrate how to implement it in code to validate an email address.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors

Two commonly used anchors in a regular expression are the caret (^) and the dollar sign ($). The ^ anchor matches the position of the first character in the string.

Example:
`^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`

In our email address validation regular expression, you can see that the string begins with a ^. This signifies that the user's email must start with either a lowercase character between a-z, a number between 0-9, an underscore, a period, or a hyphen `([a-z0-9_\.-]+)`.

*It's important to note that the + character allows for one or more occurrences of these character types.*

Similarly, the `$` anchor matches right after the last character in the string. Thus, our regex pattern is set to match an email address that ends with at least 2 characters up to a maximum of 6, and must contain lowercase characters between a-z, along with the possibility of a period `([a-z\.]{2,6})$`.

### Quantifiers

When working with regular expressions, you often encounter situations where you need to handle repeated characters. Two frequently used symbols in regex for this purpose are the asterisk (*) and the plus sign (+).

    The asterisk (*) signifies "zero or more."
    The plus sign (+) indicates "one or more."

Both symbols can match multiple characters. However, the asterisk (*) allows for matching zero or more characters, while the plus sign (+) requires at least one character to be present.

#### Example

In our regular expression `^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`, take note of the plus sign (+) placed outside the square brackets at the beginnning `^([a-z0-9_\.-]+)`.

This regex is designed to match one or more characters that are either lowercase letters between a-z, numbers between 0-9, underscores, periods, or hyphens.

![RegexPal example](./screenshots/Screenshot%202023-07-03%20at%2014-49-50%20Regex%20Tester.png)

In the screenshot provided, the first three items are successfully matched, but as expected the capitalized 'MATT' does not match.

There are also times when you want to specify an exact number of characters, like three digits in a phone's area code or the last four digits of a serial number.

You can be specific about how many repetitions you want to accept by using curly braces, you can enter the number of repetitions.

Here the example matches three reptitions:
`{3}`

By placing a comma after the three, any reptition of three or greater will be matched:

`{3,}`

Putting a number after the comma will bound the number of repetitions.

`{3,5}` matches between 3 and 5 repetitions.

Example:
In the provided regular expression `^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`, the quantifier {2,6} is utilized.

Pay attention to the {2,6} used in the final match. This pattern aims to match a sequence comprising of a minimum of 2 and a maximum of 6 characters, which can consist of lowercase letters between a-z and may include a period.

### OR Operator

In regular expressions, the "or" operator, denoted by the vertical bar (|), allows you to specify multiple alternatives for a particular match.

Let's take a look at an example where we have a pattern designed to match a domain that ends with either .com, .net, .org, or .co.uk:

`([a-z]+)\.(com|net|org|co\.uk)`

In this pattern, the parentheses () are used to group the domain extensions together. The vertical bar | acts as the "or" operator, allowing the match to be any one of the alternatives specified.

With this pattern, you would successfully match the following domains:

    google.com
    google.net
    google.org
    google.co.uk

However, this pattern is limited in its real-world application because there are numerous domain extensions available, far beyond just .com, .net, .org, and .co.uk.

That's why when it comes to matching email addresses, a more inclusive pattern is often used for the domain extension, such as: `\.([a-z\.]{2,6})$`. This pattern will match any domain extension composed of two to six lowercase letters, which may also include a period, like .co.uk.

### Character Classes

Character classes in regular expressions, denoted by square brackets [], let you specify a group of characters that you want to match. Here's a breakdown of how character classes are used in your regex pattern:
`^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`

`^([a-z0-9_\.-]+)`: This portion of the pattern is designed to match the username part of an email address. The character class `[a-z0-9_\.-]` specifies a range of lowercase letters (a-z), numbers (0-9), an underscore (_), a period (.), and a hyphen (-). The + quantifier allows one or more of these characters. The caret (^) at the start indicates that the match should start at the beginning of the string.

`@`: This character is not a class, but it is part of the pattern, matching the @ symbol in an email address.

`([\da-z\.-]+)`: This portion matches the domain name of an email address. The character class `[\da-z\.-]` specifies digits (\d is equivalent to `[0-9]`), a range of lowercase letters (a-z), a period (.), and a hyphen (-). The + quantifier allows one or more of these characters.

`\.([a-z\.]{2,6})`: This portion matches the domain extension of an email address. The character class `[a-z\.]` specifies a range of lowercase letters (a-z) and a period (.). The {2,6} quantifier allows for a sequence of two to six of these characters. This is suitable for extensions like .com and .co.uk.

$: The dollar sign at the end indicates that the match should end at the end of the string. This ensures that the entire string must match the pattern (i.e., it is a valid email address) and not just a portion of it.

### Flags

Flags in regular expressions modify the overall behavior of the pattern. There are several flags available, but we'll focus on three: i, g, and m. These flags override certain default behaviors of regular expressions.

    i - Case-insensitive: By appending i to the regular expression, the parser will ignore case sensitivity when attempting to find matches.

    g - Global: Without any flags, a regular expression stops searching after the first match. Adding the g flag instructs the parser to find all matches in a string, not just the first one.

    m - Multiline: Typically, the caret (^) matches only the beginning of a string and the dollar sign ($) matches only the end, irrespective of any line breaks. When the m flag is set, the parser treats line breaks as new lines, meaning ^ and $ will match the start and end of each line, not just the start and end of the entire string.

To add these flags to a regular expression in JavaScript, place them after the last slash of the regex literal. You can include them in any order.

For example, let's take our earlier regex pattern intended to match an email address:

`^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`

If we want this to be case-insensitive (to match both uppercase and lowercase letters), we'd add the i flag respectively:

`/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/i`

### Grouping and Capturing

Grouping refers to the process of defining portions of the regular expression as a single unit. This is done using parentheses ().

1. `^([a-z0-9_\.-]+)`: This is the first capturing group. It matches the username part of an email address. 

2. `([\da-z\.-]+)`: This is the second capturing group. It matches the domain name of an email address. 

3. `\.([a-z\.]{2,6})`: This is the third capturing group. It matches the domain extension of an email address.

### Bracket Expressions

Bracket expressions are a type of character class in regular expressions that match any single character enclosed in the brackets []. They are used to define a set of characters from which we want to match only one.

Let's break down the use of bracket expressions in your regex pattern:

`^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$`

1. `([a-z0-9_\.-]+)`: Here, `[a-z0-9_\.-]` is a bracket expression that matches any single lowercase letter (a-z), digit (0-9), underscore (_), period (.), or hyphen (-). The plus sign (+) after the bracket expression means "one or more" of the enclosed characters.

2. `([\da-z\.-]+)`: In this segment, `[\da-z\.-]` is a bracket expression that matches any single digit (\d which is equivalent to 0-9), lowercase letter (a-z), period (.), or hyphen (-). Like the previous segment, the plus sign (+) means it will match "one or more" of the enclosed characters.

3. `([a-z\.]{2,6})`: Here, `[a-z\.]` is a bracket expression that matches any single lowercase letter (a-z) or period (.). The {2,6} following the bracket expression means it will match between 2 and 6 instances of the enclosed characters.

### Greedy and Lazy Match

A **greedy** match in a regular expression will try to match as much as possible. Greedy quantifiers are the default behavior in most regular expressions. They will match the longest possible string that satisfies the pattern.

A **lazy** match, also known as a non-greedy match, will try to match as little as possible. Lazy quantifiers can be specified by following the quantifier with a question mark ?. They will match the shortest possible string that satisfies the pattern.

Let's look at our regex pattern:

`^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$.`

In this pattern, the (+) quantifier is **greedy**, which means it will match as many characters as possible that satisfy the pattern before it, as long as the rest of the pattern can still be matched.

For instance, in the first part `([a-z0-9_\.-]+)`, the (+) will match as many characters as it can (consisting of lower case letters, numbers, underscore, period or hyphen), and still allow the rest of the pattern to match.

On the other hand, if we were to modify your pattern to use a lazy quantifier like this:
`^([a-z0-9_\.-]+?)@([\da-z\.-]+?)\.([a-z\.]{2,6})$`, it would try to match as few characters as possible while still allowing the rest of the pattern to match.

So in the context of an email address, the *greedy* version would try to include as many valid characters in the username part as it can before it hits the `@` symbol, whereas the lazy version would try to include as few as it can while still maintaining a valid match.

It's important to note that whether you use greedy or lazy quantifiers can have a big impact on what your regex matches, especially when you are dealing with optional characters or repetitions. In many cases, though, they will behave the same if there is only one way to satisfy the pattern.

### Boundaries

We've previously discussed some common boundary types in the anchor section of this tutorial. Particularly, we've examined the use of `^` and `$` to represent the start and end of a string in our regex expression. There are also other commonly used boundaries, such as `\b` and `\B`, which represent word boundaries, but discussing them falls beyond the scope of this tutorial.

### Back-references

In the context of our regular expression, there are no backreferences because it doesn't require repeating subpatterns. Each of the three groups defined by parentheses has a unique pattern that doesn't need to match any of the other groups.

### Look-ahead and Look-behind

Our regular expression does not use look-aheads or look-behinds.

## Author

Hello, I'm Matthew Millard. As a passionate and committed web developer, I am currently refining my skills at the University of Toronto's Web Development Bootcamp. This was my first tutorial on GitHub Gist, and I eagerly anticipate producing more as I progress further in my coding journey. Please feel free to explore my work and reach out to me through my GitHub account [Matthew-Millard](https://github.com/matthew-millard).
