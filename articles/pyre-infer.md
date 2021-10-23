# Using facebook's pyre to infer types in python automatically

It is time to admit that you have to start using type annotations in python right now because python3 has been out there for a while already. I understand it is daunting, but at this point, even the most stubborn "old-school" python developers must be out of excuses.

And now, I'll show you how to automatically type up to 90% of your project in just a few minutes.

## Why pyre
At Facebook, we use an in-house tool for python static type checking, called [pyre](https://github.com/facebook/pyre-check). Luckily this tool is available open-source. Pyre, in comparison to [mypy](https://github.com/python/mypy) (native python's type checker), is

* Much, much faster. You won't notice it on the smaller project. But for a project which has millions of lines of code, mypy is extremely slow. You don't want to wait for your type check for 10 minutes before each commit.
* Much stricter, meaning mypy misses some cases where typing can be incorrect during the runtime.
To illustrate the last point, let me show you an example.
Consider the code:

```(python)
from typing import Optional


class Test:
    test_attribute: Optional[str] = None


def some_func(attr: str) -> None:
    print(attr)


test = Test()

test.test_attribute = "some string"

some_func(test.test_attribute)
```

Mypy sees no problem here. However, pyre points out explicitly that `test_attribute` might be `None`.
```
Incompatible parameter type [6]: Expected `str` for 1st positional only parameter to call `some_func` but got `Optional[str]`.
```

This is because, during the runtime, the `test_attribute` can be changed by another thread between setting it and passing its value to the `some_func()` function.

So the correct code for pyre be is the following:

```(python)
from typing import Optional


class Test:
    test_attribute: Optional[str] = None


def some_func(attr: str) -> None:
    print(attr)


test = Test()

test.test_attribute = "some string"

attr = test.test_attribute

if attr is None:
    raise ValueError("Attr can't be None")

some_func(attr)
```

And finally, pyre provides type inference, which I'll describe further in the article.

Because of those reasons, I use pyre for my projects in addition to mypy.

## Pyre installation
This section will be short. To start using pyre, initialize a new virtualenv (or use existing) and do the following in your project root:

```(bash)
pip install pyre-check
pyre init
```

A tiny hint: before committing the changes, check the contents of the `.pyre_configuration` file; in my case, it had an absolute path hardcoded, which I removed without any consequences.

Now you can run `pyre check` and proceed with type checking your repository. This operation will yield a lot of errors, though, if you haven't done any typing yet.

## Pyre infer

Now to the magick. The killer feature of the pyre is automatic type inference. It can check your code and put type annotations in the appropriate places.

To do that, just run:
```(bash)
pyre infer -i
```

Here is an example of what I got running it on one of my completely untyped projects recently https://github.com/prius/python-leetcode/commit/7691bb090c7b438d6cf9aefba62352ba90174c0a

Worth mentioning that pyre won't infer types for less clear cases. If your code is already well-written and each function explicitly returns values and never changes its behavior when some events happen outside, you might expect 90% of your code to be typed automatically. However, it is rarely the case. So I'd say it will be around 30% on average. The rest you will have to type yourself.

## Typing without modifying the source code
It is not always possible to modify the existing code (you want to generate types for the external project, for example). For that case, you can generate type annotations in separate files, called stub files. Those files have a `.pyi` extension and are described in [PEP-0485](https://www.python.org/dev/peps/pep-0484/#stub-files).

Run `pyre infer` without the `-i` (in-place) option to generate those stub files.

Example of such a file:

```(python)
from __future__ import annotations         
                                                                                      
class AnyOfGraphqlQuestionDetailSolution:                                             
    def __eq__(self, other: AnyOfGraphqlQuestionDetailSolution) -> bool: ...                                                                                                
    def __init__(self) -> None: ...     
    def __ne__(self, other) -> bool: ...
    def to_str(self) -> str: ...
```

## Inference in large projects
I approach typing of large projects iteratively, going bottom-up. First, you run `pyre infer` on the most low-level libraries. After typing those libraries, you run type inference on whatever libraries and apps are using those, and so on.

Also, be careful doing manual changes. Running `pyre infer` is relatively safe. But doing manual changes is not (even if your code is 100% covered by unittests). Separate automatic and manual changes into different releases. Manual typing changes lead to very subtle bugs. So be patient and don't go too fast. Typing massive projects is a very long process that, at Facebook's scale, takes years. Trying to do that in a week or month can have dire consequences.

## Type your code now
This post is a general call for action. I see a lot of open-source projects out there which are not typed yet. And it is a pain to use those projects. Proper typing makes working with your code or library so much easier. Modern IDEs use this typing information for code completion. You know exactly what to expect from a particular function so that you can think about types as some sort of documentation as well. And finally, it makes you a better programmer by teaching you to have a clear interface for your functions. Typing code which is poorly written and can accept and return types randomly is almost impossible, so in the end, you resort to writing nice code, which is easier to understand and support.

After reading this article, you have absolutely no reasons left not to do typing on your project. Run `pyre infer -i` right now. It is a super low-hanging fruit, and you must pick it.
