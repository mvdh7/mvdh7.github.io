---
title: From MATLAB to Python - package power
tags: [Python, MATLAB]
style: outline
color: dark
description:
---

<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This is the second in a short series of posts reflecting on my ongoing transition from MATLAB to Python for oceanographic data analysis, visualisation, and scientific software development.

[In the first post](/blog/from-matlab-to-python-reflections-after-a-year), I looked at a few differences between MATLAB and "pure" Python. But to get the most out of Python, you need to use some of the many packages of high level tools. Those I've used by far the most are Numpy and Scipy. There's also Pandas, which I try to avoid, but nevertheless deserves a mention. This post is primarily about those packages. Data visualisation, and integrating Python together with MATLAB, will follow.

hat do these packages do? Pure Python, while powerful, doesn't have much in the way of high-level functionality. You can't, for example, do direct calculations with vectors and matrices of numbers in the same way as in MATLAB. The variable types that can contain more than one piece of information are lists, which are like MATLAB cells, and dicts, which are a bit like structures. (And tuples, which are like lists, but you can't edit them once they've been created.) There are no built-in functions to do things like calculate a median or fit a linear regression to some data. But fear not: Python's packages very ably fill this gap. To put it another way:

<blockquote class="twitter-tweet"><p dir="ltr" lang="en">"Pandas is what happened when someone concluded that the problem with Python was that it wasn't enough like R, so built an R clone on top of Numpy, the library that was written when someone concluded that the problem with Python was that it wasn't enough like matlab."</p>— David R. MacIver (@DRMacIver) <a href="https://twitter.com/DRMacIver/status/1085592698697015298?ref_src=twsrc%5Etfw">January 16, 2019</a></blockquote>

The code examples below are [on GitHub](https://github.com/mvdh7/mvdh-xyz), as always.

## How do I use a package?

Very easily. Install the package in the environment you want to use it in - just a matter of typing something like `pip install awesomepackage` in a terminal or Anaconda prompt window. You can now use the package in Python as simply as writing `import awesomepackage`.

{% include figure.html image="https://imgs.xkcd.com/comics/python.png" caption="As with many things in science and indeed life, there's an xkcd for it." %}

With little effort, you can also set up your own code so that anyone with an internet connection can install it for themselves just as easily.

## Numpy is for numbers

I virtually always need [Numpy](http://www.numpy.org/), no matter what I'm doing in Python. Most of the things for mathematically manipulating numbers and matrices in MATLAB are found in Numpy. Once you've put your data into a Numpy array you can vectorise your calculations (i.e. do the whole array at once, instead of one value at a time in a loop). All the supporting functions that you need to work with the arrays - things like finding their dimensions, logicals, statistics like averages and variances, matrix maths, random numbers - are part of Numpy too.

But there are a few differences from MATLAB to keep in mind, things that often tripped me up towards the start, and that still lie in wait for me like rakes on a garden floor...

### Dimensions are not implicit

In MATLAB, every number is implicitly an array with infinite dimensions. This code works:

```matlab
x = 15;
disp(x(1,1,1,1,1,1,1)) % prints out 15. I could go on
```

The equivalent in Numpy throws an error. This vexes me.

Similarly, in MATLAB, a vector of values is always either a row (1-by-n), or a column (n-by-1). By contrast, the same vector as a Numpy array is only one-dimensional. The concept of it being a row or a column is meaningless, unless you explicitly add in the second dimension (there are a number of ways... e.g. `np.vstack`, `np.expand_dims`, `np.newaxis`...).

### Rows are subordinate to columns

The rules for accessing values within an array are different too. A two-dimensional matrix in MATLAB is just that: a grid of values, with no hierarchy between rows and columns. In numpy, the equivalent is really a pile of one-dimensional vectors (the rows) stacked up to form columns. The row is a subordinate dimension to the column, not its equal. By way of illustration, here we access a row and a column in MATLAB:

```matlab
x = [1 2 3; 4 5 6];
disp(x(2)) % prints out the second value i.e. 4
disp(x(2,1)) % prints out row 2 column 1 i.e. 4
disp(x(2,:)) % prints out the second row i.e. [4 5 6]
disp(x(:,2)) % prints out the second column i.e. [2; 6]
disp(x(:,2,:,:) % works just like the above
```

And in Numpy (remembering that Python indexing starts at 0, not 1):

```python
import numpy as np
x = np.array([[1, 2, 3], [4, 5, 6]])
print(x[1]) # prints out the second row i.e. [4, 5, 6]
print(x[1,0]) # prints out row 1 column 0 i.e. 4
print(x[1,:]) # as above
print(x[:,1]) # prints out the second column i.e. [[2], [6]]
#print(x[:,1,:,:]) # are you mad?
```

### Matrix maths is not default

In MATLAB, if you try to multiply or divide a pair of arrays or matrices, the default behaviour is to do matrix maths. To do elementwise multiplication or division - which is more often what I want - you have to put a `.`:

```matlab
A = [1 2; 3 4];
b = [9; 10];
C = A  * b; % returns [29; 67]
v = A .* b; % returns  [9 18; 30 40]
```

In Numpy, on the other hand, elementwise multiplication is default with `*` (and division with `/`). Matrix multiplication uses `@`:

```python
A = np.array([[1, 2], [3, 4]])
b = np.array([[9], [10]])
C = A @ b # returns [[29], [67]]
v = A * b # returns [[9, 18], [30, 40]]
```

### False friends

Numpy's `size` works like MATLAB's `numel`.

Numpy's `shape` works like MATLAB's `size`.

```matlab
% x defined in earlier example
disp(numel(x)) % returns 6
disp( size(x)) % returns [2 3]
```

```python
# x defined in earlier example
print(np.size (x)) # returns 6
print(np.shape(x)) # returns (2, 3)
```

Also - not Numpy-specific - as you will have seen in the examples, `print` is to Python as `disp` is to MATLAB. (In MATLAB `print` is for making figures, and in Python `disp` does nothing.)

### Sequences stop too soon

You can quickly create a sequence of evenly spaced numbers in MATLAB with some colons (example below). The equivalent in Numpy is `arange`. MATLAB includes the final value in the sequence, but Numpy does not. *This is consistent with Python's behaviour in other similar situations, like slicing strings and setting ranges.*

```matlab
disp(0:2:10) % [0 2 4 6 8 10]
```

```python
print(np.arange(0,10,2)) # [0, 2, 4, 6, 8]
```

### What happens in Numpy...

Pure Python contains some seductively named functions like `all` and `any` and `max`. If you're working with Numpy arrays, don't use them: stick to the Numpy versions (`np.all`, etc.).

Similarly, when using logical arrays, to avoid heartache you should generate, combine and implement them with Numpy's logical functions (`np.logical_and`, `np.logical_or`, `np.logical_not`, etc.) rather than using pure Python's logical operators.

## Scipy is for more complicated stuff

Moving up to higher-level tasks, you need [Scipy](https://www.scipy.org/about.html). This does fitting and optimisation, more advanced statistics, interpolation, cluster analysis, linear algebra, integration, signal processing, [and more](https://en.wikipedia.org/wiki/SciPy#The_SciPy_Library/Package). It is designed to be used in conjunction with Numpy and other scientific tools in Python.

At the moment, I mainly use the optimisation tools in `scipy.optimize`, for fitting bespoke empirical functions to data. I don't enjoy doing this in MATLAB: the syntax you have to use to define and fit your model is convoluted and not intuitive to me. It introduces entirely new and unmemorable variable types like `fittype` that don't exist in any other context. I usually have to copy and paste code from when I've done it before, or load up the GUI to generate the skeleton of the code, to then modify - both are anathema to my coding style. To illustrate with a very simple linear fitting function:

```matlab
% Simulate some data to fit
t = 1:10;
n = t*2 + 1 + randn(size(t));

% Subsequent code generated by the curve fitting app
[xData, yData] = prepareCurveData( t, n );

% Set up fittype and options.
ft = fittype( 'm*x+c', 'independent', 'x', ...
    'dependent', 'y' );
opts = fitoptions( 'Method', 'NonlinearLeastSquares' );
opts.Display = 'Off';
opts.StartPoint = [0.824590668780614 0.470923348517591];

% Fit model to data.
[fitresult, gof] = fit( xData, yData, ft, opts );

% Show the coefficient values
disp(coeffvalues(fitresult))
```

The equivalent experience is much more pleasant in Python. The actual fit and all of its settings can be executed in a single line of code. The function that you want to fit to is defined as a function just like any other. The syntax is intuitive:

```python
# Simulate some data to fit
t = np.arange(1,11,1)
n = t*2 + 1 + np.random.normal(size=np.size(t))

# Import least-squares fitting function
from scipy.optimize import least_squares

# Do the fit, defining the fitting function in-line
fitresult = least_squares(
    lambda coeffs: t*coeffs[0] + coeffs[1] - n, [0, 0])

# Show the coefficient values
print(fitresult['x'])
```

The `[0, 0]` in the `least_squares` function provides the first-guess coefficient values, like `opts.StartPoint` in MATLAB. The `lambda` is just Python's way of defining a function in-line, equivalent to MATLAB's `@` notation. These do the same thing as each other:

```matlab
ilfunc = @(var) var * 2;
disp(ilfunc(3)) % returns 3 * 2 = 6
```

```python
ilfunc = lambda var: var * 2
print(ilfunc(3)) # returns 3 * 2 = 6
```

Even if I was doing a project entirely in MATLAB, and I needed to fit an unusual function to some data, I would probably now just export the data and do the fitting in Python, rather than struggling through the clumsy MATLAB approach. To ease moving data between languages, `scipy.io` (input and output) contains the wonderful `loadmat` and `savemat` functions that read and create the .mat files that MATLAB likes to interact with.

## Pandas is a pain

(Disclaimer: more likely, "my lack of competence with Pandas is a pain".)

Pandas gives Python a variable type called a **DataFrame** that is like a MATLAB **table**. It's odd that I don't like Pandas because [I love tables and use them all the time](/blog/matlab-structures-and-tables).

For me, Pandas is fantastically useful for importing data from formats more complicated than a text file (which Numpy can read with its `genfromtxt` function) - spreadsheets, for example. But then I get that data out of there ASAP, into a nice friendly Numpy array, and work with that instead.

The part I struggle with most is trying to update the data in a pandas DataFrame. For example, if you import a raw spreadsheet, and want to do some processing and adjust the data in it for your analysis (quality control or calibration or whatever). Hours have been lost trying to get my head around warnings telling me I'm not actually modifying the DataFrame with the commands that intuitively seem like they should do it, and poring over the documentation on [returning a view versus a copy](http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#indexing-view-versus-copy). It seems to me that, rather than just giving a straight answer, this documentation invites you to meditate on the way Python deals with the data deep down, until you can feel it in your waters what syntax you should use.

I'm sure there's a whole bunch of very powerful and useful features that I'm missing out on for a relatively trivial reason, but for now, it's not for me.

## Carbonate chemistry is unloved... for now

This one is very field-specific, so I'll keep it brief. As a marine carbonate chemist I use [CO<sub>2</sub>SYS](https://github.com/jamesorr/CO2SYS-MATLAB) rather extensively to do the relevant calculations. Some people (shameless plug: including myself) are working towards fixing that, but there isn't an established Python equivalent yet.

## Next time(s)

  * Data visualisation;
  * Using Python in MATLAB: how? why? and is it worth the faff?
