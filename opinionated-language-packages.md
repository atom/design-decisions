# Opinionated Language Packages

**Question:** Should Atom language package defaults be opinionated when it comes to language conventions?

Languages have syntactic requirements and the communities around them have conventions. When languages have syntactic requirements, such as Makefiles requiring tab characters for indentation, the clear answer is to use those requirements as the default so as to prevent the user from shooting themselves in the foot. When there are clear conventions, even ones that are strongly recommended but still not required, should Atom language packages use those conventions as the language package defaults?

* Examples
  * [language-python](https://github.com/atom/language-python) has four spaces as the default for indentation as recommended by [PEP8](https://www.python.org/dev/peps/pep-0008/#indentation)
  * [language-gfm](https://github.com/atom/language-gfm) disables trailing whitespace trimming because two spaces at the end of a line can be used as a line-continuation sentinel
* Counter-examples
  * [language-c](https://github.com/atom/language-c) and [language-java](https://github.com/atom/language-java) do not set indentation width to 4 even though this is a clear and decades-long convention

Ref: https://github.com/atom/language-coffee-script/pull/91

## Arguments For

## Arguments Against

## Decision

TBD
