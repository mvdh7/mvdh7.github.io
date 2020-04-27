---
title: From MATLAB to Python - reflections after a year
tags: [Python, MATLAB]
style: outline
color: primary
description:
comments: false
---

For the first ~5 years of my scientific life I did all of my data wrangling in MATLAB. More recently, I began looking into the alternatives. Partly I was worried about the consequences of all of my code requiring an expensive piece of software to run, partly I was just curious, and partly it seemed like a very defensible bit of procrastination. After many months of unrealised good intentions I finally took the plunge about a year ago and started using Python in earnest. What follows is an account of my experience: the surprises, the frustrations, and the delights. I hope this may be useful to anyone thinking about making the same transition. I've ended up using a hybrid of the two, preferring to use one language for some things, and the other for others.

As I am an oceanographer, this is written from that perspective, focusing on issues that come up during the data import, analysis and visualisation tasks that I most often use MATLAB and Python to do.

This is the first of a series of two or three. Here, I mostly discuss differences in how you go about writing and implementing your code, and focus on 'pure' Python. The subsequent posts will look at the Python packages that you need to add in for scientific analysis (numpy, scipy, etc.), at data visualisation and figuresmithing, and at getting the two languages to work together.

Code for the examples is, as always, [on GitHub](https://github.com/mvdh7/mvdh-xyz).

## The coding environment

### Environment managers

MATLAB is a monolith. When it gets updated, it all gets updated together, from one source, and it all works together.

Python is a patchwork. Different bits get updated by different people at different times. Then things break.

By extension, some Python packages only play nicely with certain versions of other packages. If you tried to install every package you need for every project, you would be unlikely to find them all compatible. To get around this problem, we use an environment manager. This allows you to create multiple different versions of Python, each containing a unique set of packages that do actually work together. You can then happily update or install a new package in your current project without worrying that it's going to break all of your old code.

The main environment manager/Python distribution aimed at data scientists (rather than software developers) is [Anaconda](https://www.anaconda.com/). Like Python it is free and open source. It doesn't take too much getting used to, and 99% of my usage is covered by [a single page of the documentation](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

### Spyder IDE

MATLAB is both a coding language and and integrated development environment (IDE) that executes the code. Python is just a coding language, for which several IDEs have been built. There is no need to use an IDE to write and run Python, but it does make things easier.

I use the [Spyder IDE](https://www.spyder-ide.org/). It can be set up to look like MATLAB to ease the transition (View > Window layouts > MATLAB layout). If you're using Anaconda, you can [easily install Spyder](https://anaconda.org/anaconda/spyder) into each environment that you work in.

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/02/matlab-spyder.png" caption="Top: my MATLAB setup. Bottom: Spyder IDE in MATLAB layout." %}

### Packaging big projects

If you are building a complex package of tools that uses lots of different functions together, in MATLAB this is just a matter of making a folder with a lot of files in it, using long and convoluted function names to keep track of what's what and avoid clashing with existing functions, with no real in-built organisation or hierarchy. Installing a new 'package' is a matter of downloading the files and adding them to your path. Updating the package means deleting the old files and replacing them with new ones. I find it all a bit inelegant.

In Python, setting up a collection of code to work as a single package is intuitive and feels like a built-in expectation from the start. Distribution is also a lot better with Python. Upload to the [Python Package Index (PyPI)](https://pypi.org/) is straightforward, and then anyone can install your code with a simple `pip install mypackage`, keep track of what version they're using, and apply updates with ease.

## Pure Python

### Please don't shout at me, MATLAB;

In MATLAB, you need to put a semi-colon at the end of almost every line to stop it from printing out the result of the calculation to the Command Window. This is unnecessary in Python: things only get printed out if you ask for them.

### Indexing

To get data out of an cell or matrix or whatever in MATLAB, indexing starts at one. The indices generally go in (normal brackets), unless you're unpacking things from a cell, in which case you use {curly brackets}:

```matlab
letters = {'a' 'b' 'c'};
disp(letters{1}) % prints out 'a'
```

In Python, indexing starts at zero, and is done with [square brackets]:

```python
letters = ['a', 'b', 'c']
print(letters[1]) # prints out 'b'
```

### The perils of `=`

In MATLAB, when you use an equals sign, the thing on its left becomes a new variable that's a copy of whatever was on its right.

```matlab
x = [2 4 6 8 10]; % define x
y = x;            % copy x into y
x(3) = 99;        % change 3rd value of x to 99
disp(x) % prints out  2  4 99  8 10
disp(y) % prints out  2  4  6  8 10
```

Try the same thing in Python, I dare you:

```python
x = [2, 4, 6, 8, 10] # define x
y = x                # try to copy x into y
x[2] = 99            # change 3rd value of x to 99
print(x) # prints out [2, 4, 99, 8, 10]
print(y) # prints out [2, 4, 99, 8, 10], oh no
```

The same thing would happen if we changed `y[2]` instead. How to get your head around this behaviour? When you 'define' `x` at the start, think of it as creating a list containing `[2, 4, 6, 8, 10]`, and then telling Python: "When I say `x` I want you to get me this list." When you then declare `y = x`, that is not saying, "Make me another list like `x` but called `y`." You are actually saying: "Remember that list I called `x`? Well, it also has the nickname `y`."

Once you do develop a feel for how this works it can actually be very useful, and it saves memory by avoiding making unnecessary copies of things. *Or so I've read - I'm definitely not 100% there yet...*

There are several ways to make an actual copy. The most generic is probably using the in-built `copy.deepcopy`:

```python
from copy import deepcopy
x = [2, 4, 6, 8, 10] # redefine x
y = deepcopy(x)      # actually copy x into y
x[2] = 99            # change 3rd value of x to 99
print(x) # prints out [2, 4, 99, 8, 10]
print(y) # prints out [2, 4,  6, 8, 10]
```

### Default/optional function arguments

If you write a MATLAB function and you want some of the arguments (inputs) to be optional, I find it quite a pain, with lots of fiddling around with things beginning with `narg`. I'm not going to write an example script, it's too much effort.

In Python, it's a dream. Just put the optional argument(s) in, along with their default values, when you declare the function:

```python
def myfunc(x,y, z=3):
    return x + y + z

print(myfunc(5,6))   # prints out 14
print(myfunc(5,6,7)) # prints out 18
print(myfunc(5,6, z=7)) # as above
```

Here `z` is an optional input that takes the value of `3` if you don't specify it.

## To be continued...

The points above seem to be mostly neutral or pushing in Python's favour, and indeed the 'hybrid' coding approach I've fallen into tends to prefer Python for data analysis, but MATLAB for visualisation.

Next time(s), more on: Python packages like Numpy and Scipy that you need to use to replicate MATLAB's higher-level functionality; data visualisation; and my experience of trying to get the two languages to play with each other.

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/from-matlab-to-python-reflections-after-a-year';  // Replace PAGE_URL with your page's canonical URL variable
// this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://mvdh7.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
