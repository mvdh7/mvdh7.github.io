---
title: Speedy variable switching
tags: [MATLAB, Dataviz, Workflow]
style: outline
color: light
description:
---

In oceanographic research we often want to produce plots of various variables either against each other or against depth, time or location.

The main structure of these figures will be very similar regardless of exactly which variable(s) you want to look at, but a whole bunch of settings (axis limits, labels, ticks...) might be different in each case.

Here, we'll look at a way to set up a figure in MATLAB such that you can very efficiently switch backwards and forwards between the different variables that you might want to visualise, all within a single script. You can use these techniques all the way from initial data exploration up to making full publication-quality figures.

The code in the examples is all available in a [.m file on GitHub](https://github.com/mvdh7/mvdh-xyz/blob/master/matlab/speedyVariableSwitching.m).

## Have your data in a structure or table

The first thing you need is to have your data to plot in a structure or table. Take a look at [my previous blog post](/blog/matlab-structures-and-tables) for more information on these MATLAB variable types.

Having our data in this format allows us to "dynamically reference" which field we want to use. Let's take a simple example. Say we have a dataset of salinity and nitrate values, measured across a river estuary. We can simulate a dataset to play with as follows:

```matlab
%% Simulate dataset to play with
clear ts
ts.dist = (0:50:1000)'; % distance along the estuary
ts.sal = 1 + ts.dist * 34/1000 + 0.1*randn(size(ts.dist)); % salinity
ts.no3 = 10 * exp(-ts.dist/3e2) + 0.05*randn(size(ts.dist)); % nitrate
ts = struct2table(ts);
```

## Make a basic figure

We can visualise the data as follows, starting [as always](/blog/making-matlab-figures-1-basic-workflow) with a `figure(X); clf`:

```matlab
%% Super basic plotting
figure(1); clf
scatter(ts.dist,ts.sal) % data points
```

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/blog0.png" caption="What an exciting figure." %}

To change which variable we want to plot, we could just switch between using `ts.sal` and `ts.no3` as the input to `scatter`.

## Now make it a bit more complex

Now let's make the figure a bit more complicated; we'll add a mixing line, and a second subplot that shows deviations from the mixing line.

```matlab
%% Now add a mixing line and second subplot

% Calculate gradient and intercept of mixing line
mixLineGradient = diff(ts.sal([1 end])) / diff(ts.dist([1 end]));
mixLineIntercept = ts.sal(1);

% Create mixing line function of distance
mixLineFunction = @(dist) mixLineIntercept + mixLineGradient * dist;

% Draw figure
figure(1); clf

subplot(2,1,1); hold on
    scatter(ts.dist,ts.sal) % data points
    plot(ts.dist([1 end]),ts.sal([1 end])) % mixing line

subplot(2,1,2)
    scatter(ts.dist,ts.sal - mixLineFunction(ts.dist)) % residuals
```

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/blog1.png" caption="Now we have a more complex plot with lots of repetition of `ts.sal` in the code." %}

Now, if we want to switch to plotting nitrate instead of salinity, we have to change `ts.sal` to `ts.no3` in *five* different places! Ain't nobody got time for that, so this is where the aforementioned dynamic referencing comes in.

## Make it *dynamic*

We define a variable (say `fyvar`) that is just a string containing the name of the variable we want to plot on the y-axes. We then replace `ts.sal` with `ts.(fyvar)` each time it appears:

```matlab
%% As above, but with dynamic y-axis variable

% Decide which variable to plot
fyvar = 'sal';

% Calculate gradient and intercept of mixing line
mixLineGradient = diff(ts.(fyvar)([1 end])) / diff(ts.dist([1 end]));
mixLineIntercept = ts.(fyvar)(1);

% Create mixing line function of distance
mixLineFunction = @(dist) mixLineIntercept + mixLineGradient * dist;

% Draw figure
figure(1); clf

subplot(2,1,1); hold on
    scatter(ts.dist,ts.(fyvar)) % data points
    plot(ts.dist([1 end]),ts.(fyvar)([1 end])) % mixing line

subplot(2,1,2)
    scatter(ts.dist,ts.(fyvar) - mixLineFunction(ts.dist)) % residuals
```

Now all we have to do to switch between plotting salinity and nitrate is change the value of `fyvar` from `'sal'` to `'no3'`, and everything else sorts itself out automatically. The figure doesn't look any different from in the previous example.

## Use a switch block for other settings

We may have other settings that we want to be different for the different variables. The best way to implement this is to put a `switch` block towards the start of your code (but after the value for `fyvar` is set).

In this case we would write the following:

```matlab
%% Add axis labels, and a switch block to set their values

% Decide which variable to plot
fyvar = 'no3';

% Variable-specific settings
switch fyvar
    case 'sal'   
        fylabel = 'Salinity';
    case 'no3'
        fylabel = 'Nitrate';      
end %switch

% Calculate gradient and intercept of mixing line
mixLineGradient = diff(ts.(fyvar)([1 end])) / diff(ts.dist([1 end]));
mixLineIntercept = ts.(fyvar)(1);

% Create mixing line function of distance
mixLineFunction = @(dist) mixLineIntercept + mixLineGradient * dist;

% Draw figure
figure(1); clf

subplot(2,1,1); hold on
    scatter(ts.dist,ts.(fyvar)) % data points
    plot(ts.dist([1 end]),ts.(fyvar)([1 end])) % mixing line
    xlabel('Distance')
    ylabel(fylabel)

subplot(2,1,2)
    scatter(ts.dist,ts.(fyvar) - mixLineFunction(ts.dist)) % residuals
        xlabel('Distance')
    ylabel([fylabel ' residuals'])
```

This code executes as follows (noting that we've switched to looking at nitrate, not salinity):

  1. The value of `fyvar` is set as `'no3'`;
  1. The `switch` block tests the value of `fyvar`, finds that it is `'no3'`, so sets `fylabel` to `'Nitrate'`;
  1. When we reach `ylabel(fylabel)`, the correct y-axis label of `"Nitrate"` is applied.

## Tell me more about `switch` blocks

A `switch` block is essentially a shorthand way of writing a sequence of `if ... elseif ...` statements, where the thing being tested by the `if` is the same every time. So the block in the example above:

```matlab
switch fyvar
    case 'sal'   
        fylabel = 'Salinity';
    case 'no3'
        fylabel = 'Nitrate';
end %switch
```

could alternatively, but less elegantly, be written as:

```matlab
if strcmp(fyvar, 'sal')
    fylabel = 'Salinity';
elseif strcmp(fyvar, 'no3')
    fylabel = 'Nitrate';
end %if
```

You can add as many lines of code as you like after each `case` statement, and a single `case` statement can match multiple values if you group them using curly brackets: `{}`.

## I want to see both variables at once!

As a final flourish, once we've set up all the coding infrastructure above, we can now very easily draw the plots for both salinity and nitrate side by side, with all of the settings that we've defined, by using a `for` loop.

```matlab
%% Use a loop to plot nitrate and salinity side by side

% Define list of variables to plot
fyvars = {'sal' 'no3'};

% Draw figure
figure(1); clf

% Loop through the variables in fyvars
for V = 1:numel(fyvars)

% Pick which variable to plot from fyvars
fyvar = fyvars{V};

% Variable-specific settings
switch fyvar
    case 'sal'
        fylabel = 'Salinity';
    case 'no3'
        fylabel = 'Nitrate';
end %switch

% Calculate gradient and intercept of mixing line
mixLineGradient = diff(ts.(fyvar)([1 end])) / diff(ts.dist([1 end]));
mixLineIntercept = ts.(fyvar)(1);

% Create mixing line function of distance
mixLineFunction = @(dist) mixLineIntercept + mixLineGradient * dist;

subplot(2,2,(V-1)*2+1); hold on
    scatter(ts.dist,ts.(fyvar)) % data points
    plot(ts.dist([1 end]),ts.(fyvar)([1 end])) % mixing line
    xlabel('Distance')
    ylabel(fylabel)

subplot(2,2,2*V)
    scatter(ts.dist,ts.(fyvar) - mixLineFunction(ts.dist)) % residuals
    xlabel('Distance')
    ylabel([fylabel ' residuals'])

end %for V
```

{% include figure.html image="https://mphumphreys.files.wordpress.com/2019/01/blog2.png" caption="An automated side-by-side plot can now be generated with little effort." %}

The changes that we've made to the previous version are:

  1. Added a cell of strings containing all the variables we want to plot (`fyvars`);
  1. Added a `for` loop that cycles through this cell of strings;
  1. Changed the subplots to a 2 by 2 grid, and used a bit of mental arithmetic to figure out the correct subplot number from the value of `V` (the loop counter).

## A note on workflow

[In a previous post](/blog/making-matlab-figures-2-gcf-and-print) I discussed figure creation workflow when you are setting something up with publication quality (in the final section before the summary).

In that context, all of the code I've described above (and probably still a fair bit more) would come in the first section of the workflow, when you're still setting up the figure and watching it in MATLAB, and before you add in the `print` statement to output the results to a file.

## Summary

  * Prepare your data in a table or structure and you can very easily change the variables that are plotted on complex figures;
  * Use a `switch` block to control variable-specific settings;
  * Put the whole lot in a `for` loop to display multiple variables at once.
