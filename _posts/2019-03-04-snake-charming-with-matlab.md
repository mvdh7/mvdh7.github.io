---
title: Snake charming with MATLAB
tags: [Python, MATLAB, Workflow]
style: outline
color: success
description: Switching data between Python and MATLAB, and using both together.
comments: false
---

This continues a short series about my ongoing transition from MATLAB to Python. Previously I talked about some [fundamental differences between the coding languages](/blog/from-matlab-to-python-reflections-after-a-year), and delved a bit more into [the main Python packages that you need to use](/blog/from-matlab-to-python-package-power) to replicate MATLAB's higher-level functions. Here, I'll look at how you can get the two languages to work together, giving some very basic examples.

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/03/snakecharming.png" caption="Schematic illustration of calling a Python function from within MATLAB." %}

Owing to my reluctance to draw figures in Python, but my preference for Python for data analysis, I have become very interested in how to use Python and MATLAB together, efficiently. I've been doing this on two different levels: first, just saving data from one language and opening it up in the other; and second, directly running Python from within MATLAB.

Code for the examples is available [on GitHub](https://github.com/mvdh7/mvdh-xyz) as always.

## Sharing variables

You may wish to simply transfer some results from Python to MATLAB and/or back again. This is very easy to do: **scipy** includes functions that can read and write variables using MATLAB's preferred .mat file format.

### Python to MATLAB

For example, we might begin our analysis in Python:

```python
#%% Import numpy, and savemat function from scipy
import numpy as np
from scipy.io import savemat

# Create some variables
xvec = np.linspace(0, 10, 100)
yvec = 2*np.sqrt(xvec) + xvec + 3 + np.random.normal(size=np.size(xvec))

pasta = ['spaghetti', 'fusilli', 'orzo', 'penne']

# Save variables to .mat file
matfile = '../data/snakeCharming.mat'
savemat(matfile, {
    'x': xvec,
    'y': yvec,
    'pasta': pasta
})
```

We have saved the Python variables `xvec`, `yvec` and `pasta` as `x`, `y` and `pasta` in the file **snakeCharming.mat**. Opening this in MATLAB is a breeze:

```matlab
%% Import data from Python
matfile = '../data/snakeCharming.mat';
load(matfile)
pasta = cellstr(pasta); % probably a better format to work with

% Plot x and y
figure(1); clf
scatter(x, y)
```

A few quick notes on this code:

  * The Python list of strings in `pasta` is imported as a MATLAB **char** array. The `cellstr` function converts this into a cell of strings, which is probably more useful, depending upon what you want to do;
  * By providing the relative path to the .mat file in the `matfile` variable, we avoid needing to add that folder to our MATLAB search path;
  * [As always](/blog/making-matlab-figures-1-basic-workflow), the MATLAB code for the figure begins with `figure(1); clf`.

After executing the code above, the variables `x`, `y` and `pasta` appear in the MATLAB workspace and are ready to work with:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/02/capture.png" caption="How easy was that?" %}

### MATLAB to Python

If we do some processing in MATLAB and want to get the results back into Python, we can use a similar syntax:

```matlab
%% Do some more calculations
z = x.*y + 4;

% Append new variable to the .mat file
save(matfile, 'z', '-append')
```

And to import this new variable into Python:

```python
#%% Get the variable back in Python
from scipy.io import loadmat

zvec = loadmat(matfile)['z'][0]
```

## Running Python within MATLAB

The workflow described above is probably adequate for many purposes. But we can go one step beyond: we can run Python directly from within MATLAB, and exchange variables between the two without needing to save any files. *The opposite is also possible - calling MATLAB from Python - but I haven't felt the need to try.*

Health warning: I find [MATLAB's documentation](https://uk.mathworks.com/help/matlab/call-python-libraries.html) rather incomplete and hard to follow. Even once you've got it working, things can be a little temperamental. I've that found the steps below work fairly reliably across Windows and Apple operating systems.

### Why bother?

A lot of the code that I work with involves sprawling empirical models packed with convoluted equations and hundreds of coefficients that commonly have 10 significant figures or more. This is typo heaven. Once you've built and tested an implementation of such a model it would therefore be much better to be able to use the same version in different settings rather than having to translate and rewrite it. That way there is a 'single source of truth' for your calculations: any errors that you do find only have to be fixed in one place, and they are fixed everywhere. Once tested in one place, you know the results are reliable everywhere.

### Choose your environment

The first thing to consider is that you will probably have a number of different Python environments set up, each with access to a different selection of packages. You need to choose which one you want, and make sure that MATLAB does the same.

(According to [MATLAB's documentation](https://uk.mathworks.com/help/matlab/getting-started-with-python.html), only Python versions 2.7, 3.5 and 3.6 are supported, so make sure your Python environment is using one of those.)

To this end, you need to identify the Python executable associated with the environment you want to work with. Just start up Python, making sure you are working in the environment of interest, and enter:

```python
#%% Find my Python executable
from sys import executable

# Print it out for copy and paste:
print(executable)

# Or just save it to a .mat file:
savemat('../data/pyexe.mat',
        {'pyexe': executable})
```

This prints out the full path to the relevant Python executable. Copy this as a string into MATLAB, or save it as a .mat file as above. This is the input you need for `pyversion` in MATLAB, to load up a Python kernel.

```matlab
%% Load Python in MATLAB
load('../data/pyexe.mat') % get path to Python executable

% Check if Python already loaded
[~, ~, pyloaded] = pyversion;

% Load Python if not already loaded
if ~pyloaded
    pyversion(pyexe);
end %if
```

A few pointers:

  * You can only run the `pyversion` command once, or it will throw an error - the `if` statement circumvents this issue;
  * If you want to change the Python environment (i.e. use a different executable), you will have to restart MATLAB;
  * If you are on Mac/Linux there won't be ".exe" at the end of the executable's path;
  * You can check the correct Python environment has been loaded by then typing just `pyversion` in the Command Window.

### Import packages

This part is not strictly necessary but it's very helpful to do it next as a quick test that everything is working as it should. Continuing in MATLAB, use the following command to import whichever Python package you want to use. For example, if we wanted to use Numpy:

```matlab
%% Check that we can import numpy
np = py.importlib.import_module('numpy');
```

If this works without any errors, all is well. You'll see the `numpy` module appear in your Workspace:

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/03/numpy.png" caption="A successful load of Numpy." %}

If you get errors, then I'd suggest checking:

  * That you're working in the right Python environment;
  * That you can import the module you're looking at directly from within Python in that environment (i.e. not via MATLAB);
  * The [MATLAB troubleshooting documentation](https://uk.mathworks.com/help/matlab/matlab_external/undefined-variable-py-or-function-py-command.html).

If it's neither of those, there's probably some sort of compatibility issue... good luck!

### Using the Python packages

Using the Python packages is fairly straightforward. If you've imported a package as in the example above, you can do things much as you would in Python:

```matlab
%% Calculate sine of some numbers using numpy
x = [1 2 3 4 5];
sines = np.sin(x);

% or if we hadn't done the previous importlib step
sines2 = py.numpy.sin(x);
```

In the second example above, you can see that we don't need to explicitly import Numpy. Any Python package that you could import in that Python environment can be accessed in this way (i.e. using the full package name) without explicitly importing it in MATLAB.

Some MATLAB variables need to be converted into their Python equivalents to use them with the imported Python packages, while others are just translated automatically. Simple stuff like scalars, one-dimensional vectors and strings tend to work, as in the example above. Matrices and cells and things are not so good. You may need to play around with the functions in Numpy like `array` and `concatenate` to construct your MATLAB variables into a format that Python likes the look of.

### Retrieve results

Finally, we need to turn the results from Python back into variable types that MATLAB can work with. If you look at the sines variable that's been created, it's not just a MATLAB double, it's a numpy array with a bunch of associated extra properties. You can do basic operations (`+`, `*`, etc.) on it using the normal MATLAB syntax, but trying to put it through a MATLAB function will generally throw an error.

Type conversions are sort-of described here in the documentation. In general, to get an array of numbers back into a MATLAB-only format, use this syntax:

```matlab
%% Convert result into a MATLAB-friendly format
sines = double(py.array.array('d', sines));
```

### Performance note

If you are running any calculations through loops, or through a sequence of Python functions, it's better to write the set of calculations entirely in Python, rather than MATLAB. This is because the actual conversion between MATLAB and Python variable types can be a bottleneck for the speed of your code.

## In summary

  * Use the `savemat` and `loadmat` functions from `scipy.io` to swap variables between MATLAB and Python;
  * Or call Python directly from within MATLAB - this is fantastic when you've got it working, but prepare yourself for some faff;
  * Stick to just using MATLAB to provide inputs and extract outputs from the Python, rather than putting Python functions in MATLAB loops.

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/snake-charming-with-matlab';  // Replace PAGE_URL with your page's canonical URL variable
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
