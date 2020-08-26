---
layout: post
title: Predicting Flight Delays
image: /img/hello_world.jpeg
---


## Predicting Flight Delays

![](https://cdn-images-1.medium.com/max/2000/1*eIjMz7BfU07FtTXX9zI0VA.jpeg)

## Introduction

One of the biggest challenges in the aviation industry is the unpredictability of flight delays. However, with modern Data Science tools and the right collected data we can make an effort to predict the unpredictable.

We’ll be using a dataset from 2015 flight delays, covering nearly 6 million flights and each flight having 31 columns worth of values, which ends up being around 180 million observations total. This is far too much for the scope of this project, so we’ll be using 10% of that data instead.

## Data Exploration/Wrangling

Many of the features of this data set are post-mortem by nature, meaning they give information as to reasons a flight was delayed, which will only serve as leakage for the random search model I’ll be building. In the spirit of predicting flight delays based on information obtained before the flight takes off, we will be dropping any post-mortem features.

This leaves us with a handful of features to predict our flight delays. Before running this model, I believe the most notable will be the actual departure time and the scheduled time/distance the flight will cover.

Other features to note are the Airline, Departing Airport, and Arrival Airport. Our objective is to detect whether or not specific Airports and Airlines have a higher chance to have a delayed flight.

![](https://cdn-images-1.medium.com/max/6000/1*0wvtGQPJBB_qFj7PAewK1w.jpeg)

Now that the data is all cleaned up and ready to go, let’s split the data into our train and test sets (80/20% respectively). Now we can set the target to the delay on arrival, meaning how far off was the flight’s arrival compared to the scheduled arrival. Because there is a relatively infinite amount of time a flight could be delayed (think cancellations), we’ll be removing any cancelled flights. At the same time we’ll be changing our delay metric to a classification problem rather than a regression problem; all we really care about is whether the flight will be delayed, on time, or early.

Having changed the model to a classification problem we can use our majority class baseline of 62% belonging to on-time flights. We’ll try to beat that.

## The Model

Now that we’re ready to split our data into its X and Y, we can run our Random Search model using a Random Forest Classifier, an Ordinal Encoder and a Simple Imputer. We’ll be using a cross-validation with 4 folds and 10 iterations, totaling 40 fits.

The model scored a 69.7% on our Train dataset, and a 70.2% on our Test dataset. I’m quite happy with these results, so lets analyze the importance of each feature.

Features 6: Scheduled Departure
Feature 7: Departure Time
Feature 8: Taxi Out
Feature 9: Scheduled Time
Feature 10: Distance
Feature 11: Taxi In

![](https://cdn-images-1.medium.com/max/2000/1*In4hc3-aQIYq0SCeJq8qxw.jpeg)

From our feature importance bar chart we can observe that Scheduled Departure was the most important for predictions, followed by Departure Time.

If we take a moment to think about it, it makes sense. Flight delays are more likely to happen during high-traffic hours; if more flights are happening at any given hour then they are more likely to be met with random events causing delays.

Almost as important, Scheduled Time (total hours from point A to point B) indicates whether or not a flight has to cover long distances. We can theorize that longer distances cause more delays due to the increased likelihood for something to go wrong within that time frame. The longer the flight spends getting from point A to point B, the more likely it is for something to go wrong on either the departing or arrival airport, or the airplane itself.

A visual of the relation between Scheduled Time and Departure Time, and how it affected the Flight Delay Class:

![](https://cdn-images-1.medium.com/max/2000/1*c6x1e8bD_fQFtedw9-ew5g.jpeg)

## Conclusion

Flight delays can be caused by an outstanding amount of variables, which eventually may be quantified and analyzed individually. With the data I chose, it was evident specific Airlines and Airports were not as big of a factor as the time of day (think ‘rush hour’) and distance the flight has to cover.

Dataset: [https://www.kaggle.com/usdot/flight-delays/data](https://www.kaggle.com/usdot/flight-delays/data)
