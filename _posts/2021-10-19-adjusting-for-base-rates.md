---
layout: single
title:  "Adjusting for base rates"
date:   2021-10-19
category:
    machine learning
tags:
    calibration
---

A  common issue with predictions from models is that you've trained in a dataset where the base rate (or prevalence) is different to that in your target data. This might be because you had a big case-control dataset available for training where you think you can do well at learning but you actually want to apply it so a more population-based setting where prevalence is much lower. It might also happen if you've deliberately downsampled controls to mitigate class imbalance.

Either way, it's an extremely common problem. So what to do?

# Elkan's transformation

I should start by noting that in clinical prediction modelling circles this is handled a little differently. In those scenarios you're almost always using some form of regression, and so adjusting predictions from a logistic regression is done by changing the intercept. 

For us though I'm going to assume you're working with machine learning models, you have predicted probabilities already and you want a generic method to adjust them to account for this difference in prevalence.

The solution to this was [introduced by Elkan back in 2001](https://cseweb.ucsd.edu//~elkan/rescale.pdf), and it requires very little code. 

If you want to go through it in more detail you can check out the paper. In Python, this looks as shown below. Here i've assumed that you've passed predictions from the classifier in the new data as y_pred, and that base_rate and new_rate are the prevalence in the train and test data. Alternatively you can just pass y_true to these and it will compute the rates. Following that it does the transformation Elkan recommends, and we're done! 

```python
def elkan_transform(y_pred, base_rate, new_rate):
    # getting base_rate and new_rate
    base_rate = base_rate if isinstance(base_rate, float) else np.mean(base_rate)
    new_rate = new_rate if isinstance(new_rate, float) else np.mean(new_rate)
    
    # Elkan's formula
    numerator = y_pred - (y_pred * base_rate)
    denominator = base_rate - (y_pred * base_rate) + (new_rate * y_pred) - (base_rate * new_rate)
    frac = numerator / denominator
    
    return new_rate * frac
```

I should note that how good your new calibration plots look will depend partly on discrimination. Calibration improves at higher discrimination and you can't put predictions which are close to chance in AUC and expect a nice calibration curve after adjusting the probabilities. Otherwise I have always seen good results with this. I should also note that this does not account for any other issues like recalibration that is often needed for tree-based methods. This only adjusts for prevalence differences, not any other issues which might make our training data unrepresentative of the target population for the model. 

I realise calibration, what it is and why it's important aren't covered here - I'll add those into another post :)