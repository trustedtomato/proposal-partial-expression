I've reworked some of Ramda's source files to show how clearer are they with the partial expression syntax.
For the original files, see [Ramda's respository's source directory](https://github.com/ramda/ramda/tree/master/source).

Note: The proposal's general purpose is to eliminate the need for these functions, as you can write `#?` instead of `identity`  ör `#? * 10` instead of `multiply(10)`. But hey, it can be used this way too, and it makes the code more readable, so why not?
