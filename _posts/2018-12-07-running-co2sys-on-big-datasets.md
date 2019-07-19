---
title: Running CO<sub>2</sub>SYS on big datasets
tags: [Marine carbonate system, Seawater chemistry, MATLAB]
style: fill
color: dark
description: What's the most efficient way to solve a large marine carbonate system dataset using CO<sub>2</sub>SYS in MATLAB?
comments: true
---

## What is CO<sub>2</sub>SYS?

Carbon dioxide dissolves in seawater and undergoes a variety of chemical reactions, collectively known as the *marine carbonate system* (MCS). If we measure any two properties of the MCS, we can calculate all of the others. This calculation is called *solving the MCS*. In MATLAB, the function [CO<sub>2</sub>SYS](https://github.com/jamesorr/CO2SYS-MATLAB) can be used to solve the MCS.

A few months ago somebody asked me for advice on speeding up some code they had written that used CO<sub>2</sub>SYS to do just that for a large oceanic dataset - specifically, [GLODAPv2](https://www.earth-syst-sci-data.net/8/297/2016/). They aimed to do a Monte-Carlo uncertainty analysis by adding random offsets to the input values and then observing the distribution of the output. They were interested in the mean of an output variable over the entire input field.

Although set in the context of using CO<sub>2</sub>SYS, the principles discussed here do apply more generally to help speed things up in MATLAB.

## Loops within loops take forever

Their original code was something like this (this is pseudocode, real working scripts [available here](https://www.dropbox.com/s/0bvmhc3z076p1kw/big_co2sys.m)):

```matlab
%% Monte-Carlo the super-slow way

% Set uncertainty values
SD_input1 = 6.3;
SD_input2 = 3.8;

% Loop through uncertainty simulations
for U = 1:number_of_simulations

    % Loop through the arrays
    for I = 1:length(input1)

        % Add random offsets to original data
        U_input1(I) = input1(I) + randn * SD_input1;
        U_input2(I) = input2(I) + randn * SD_input2;

        % Calculate output variable from offset data
        U_output(I) = CO2SYS(U_input1(I), U_input2(I), etc..);

    end %for I

    % Calculate mean output with uncertainty
    output_mean(U) = mean(U_output);

end %for U

% Get uncertainty in output (standard deviation)
SD_output = std(U_output_mean);
```

A time test using a small number of simulations and only a fraction of the dataset suggests that running 1000 simulations with the full dataset (~70,000 data points) would take on the order of 4-5 days on my PC...!

## Vectorisation makes it much faster

The slowness mostly comes down to the internal loop through the arrays (`for I = ...`). We can fairly easily eliminate this loop if we *vectorise* the code as follows:

```matlab
%% Monte-Carlo the faster way

% Set uncertainty values
SD_input1 = 6.3;
SD_input2 = 3.8;

% Loop through uncertainty simulations
for U = 1:number_of_simulations % can change for to parfor

    % Add random offsets to original data
    U_input1 = input1 + randn(size(input1)) * SD_input1;
    U_input2 = input2 + randn(size(input1)) * SD_input2;

    % Calculate output variable from offset data
    U_output = CO2SYS(U_input1, U_input2, ...);

    % Calculate mean output with uncertainty
    output_mean(U) = mean(U_output);

end %for U

% Get uncertainty in output (standard deviation)
SD_output = std(U_output_mean);
```

So, instead of looping through the input arrays and adding a random offset to each value one at a time, we create a vector of random numbers the same size as each input array and add them together in one step. This allows MATLAB to do the computation much more efficiently. Ultimately MATLAB is still looping through each input array and adding uncertainties one by one just as before, but now it's doing that looping deep down inside the computer using lower-level code that runs much more quickly. This new approach would take 6-7 minutes on my computer to do 1000 simulations.

## Parallel computing can be faster still

We can speed things up yet further by using a `parfor` loop. This takes advantage of the fact that most computers have multiple processor cores and it sends off bits of the calculation to them all simultaneously, instead of running sequentially through the loop on one processor. The exact amount of speed-up therefore depends on how many cores your computer has.

The caveat is that it takes a while (~25 seconds on my PC) for MATLAB to sort itself out when you first initialise a *parallel pool*, which is required to run the `parfor` loop. The pool is initialised automatically when you try to run a `parfoor` loop, or you can do it in advance by typing gcp into the Command Window. Once set up, the pool will stay active for half an hour after the last use.

This can be implemented in the previous pseudocode very simply by changing `for U` to `parfor U`.

On my PC, with only 4 processor cores, this speeds up the time for 100 simulations from about 40 seconds to 30 seconds. The speed up would be greater on a computer with more cores, and probably relatively greater for a bigger number of simulations.

## In summary

  * Don't loop through arrays (unless you have to) - vectorise the code instead
  * If you do have to loop, `parfor` loops might go more quickly, but have a set-up time cost
  * The amount of speed-up from a `parfor` varies a lot depending on how many processor cores your computer has
  * The newest version of [CO<sub>2</sub>SYS](https://github.com/jamesorr/CO2SYS-MATLAB) also does uncertainty calculations for you so you probably don't need to do it yourself like this anyway
  * Working scripts accompanying this post are [available here](https://www.dropbox.com/s/0bvmhc3z076p1kw/big_co2sys.m)

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = 'https://mvdh.xyz/blog/running-co2sys-on-big-datasets';  // Replace PAGE_URL with your page's canonical URL variable
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
