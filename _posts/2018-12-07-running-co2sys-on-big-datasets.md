---
title: Running CO<sub>2</sub>SYS on big datasets
tags: [External Post, Git]
style: fill
color: warning
description: What's the most efficient way to solve a large marine carbonate system dataset using CO<sub>2</sub>SYS in MATLAB?
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
        U_output(I) = CO2SYS(U_input1(I), U_input2(I), ...);

    end %for I

    % Calculate mean output with uncertainty
    output_mean(U) = mean(U_output);

end %for U

% Get uncertainty in output (standard deviation)
SD_output = std(U_output_mean);
```
