# Coding Style Guides & Best Practices

Writing readable code is an art and following style guides is one way of ensuring our code is always clean, readable and consistent. Language specific style guides exist but general coding standards apply to all programming languages. These are not to be blindly followed; strive to understand these and ask when in doubt.

## General
-   Don't duplicate the functionality of a built-in library (Note that this is not the case if the task at hand is purely algorithmic).
-   Don't swallow exceptions or "fail silently".
-   Don't write code that guesses at future functionality.
-   Exceptions should be exceptional.

## Naming

-   Make use of meaningful variable names. Avoid abbreviations.
-   Name the enumeration parameter the singular of the collection. e.g when looping over a collection of books, say for book in books and not for b in books. It’s clearer this way.
-   Name variables created by a factory after the factory (`userFactory` or `UserFactory` creates user).
-   Name variables, methods, and classes to reveal intent. Intent describes the function of the class, method or variable. A typical example is if we were describing a class method to sort a collection by a certain parameter. It’s preferred to define this as `obj.sort_by(param)` or `obj->sort_by(param)`. It makes code more readable and requires less documentation.
-   Treat acronyms as words in names (`XmlHttpRequest` not `XMLHTTPRequest`), even if the acronym is the entire name (class `Html` not class `HTML`).
-   Suffix variables holding a factory with _factory (user_factory)
    
