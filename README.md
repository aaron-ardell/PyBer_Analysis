# PyBer_Analysis

## Overview of the Analysis

Our client at PyBer has challenged us with creating a summary DataFrame of ride-sharing by city type. We'll utilize Pandas and Matplotlib to create a multiple-line graph that shows the total weekly fares for each city type. Our goals for this assignment are as follows. First, create a ride-sharing summary DataFrame by city type. Second, making a multiple-line chart of total fares for each city type. Lastly, we'll complete this written report providing analysis.

We've been provided with two databases from the client: city_data_df and ride_data_df. By combining the two DataFrames, we'll be able to produce one DataFrame.

```
# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])

# Display the data table for preview
pyber_data_df.head()
```

Using .groupby, .count() and .sum() functions, we're able to create series containing data that will be helpful. Combining them into their own summary DataFrame through dictionaries as below

```
pyber_dict = {'Total Rides': total_rides, 'Total Drivers': total_drivers, 'Total Fares': total_fares, 'Average Fare per Ride': avg_fares_rides, 'Average Fare per Driver': avg_fares_driver}
pyber_summary_df = pd.DataFrame(pyber_dict)
pyber_summary_df
```

Now we're onto creating a new DataFrame with dates as the index. Resetting our index in preparation to creating a pivot table that will be the basis for our line graph. We're starting with type and date using the groupby function, calculating the sum of the fares. We're going to be making fares the value of the DataFrame(and soon, the graph). Then, indexing the time, and switching it to a weekly timeframe using the resample function.  

```
new_pyber_df = pyber_data_df.groupby(['type', 'date']).fare.sum().to_frame()
new_pyber_df

new_pyber_df = new_pyber_df.reset_index()
new_pyber_df.head()

pyber_pivot = new_pyber_df.pivot(index='date',columns="type", values="fare")
pyber_pivot.head(10)

pyber_2019 = pyber_pivot.loc['2019-01-01':'2019-04-28']
pyber_2019.head()

pyber_2019 = pyber_2019.reset_index()
pyber_2019['date'] = pd.to_datetime(pyber_2019['date'])
pyber_2019.set_index("date", inplace=True)
pyber_2019.head()

weekly_pyber_2019 = pyber_2019.resample('W').sum()
weekly_pyber_2019.head(10)
```


## Results 

![This is an image](https://github.com/aaron-ardell/PyBer_Analysis/blob/main/2022-11-23%20(1).png)

![This is an image](https://github.com/aaron-ardell/PyBer_Analysis/blob/main/2022-11-23.png)

![This is an image](https://github.com/aaron-ardell/PyBer_Analysis/blob/main/Pyber_fare_summary.png)


## Summary 

From our analysis, there are several conclusion that we can make based on the data. First, Urban City fares had the highest total ahead of Suburban City fares. Rural Fares had the lowest total fares. The driver behind those results is the higher number of drivers and rides. Second, Rural fares had the highest average per driver and ride, followed by Suburban. COnsidering these first two revelations, further analysis on the length of trips for Rural City drivers may reveal why their average fares are so high but their overall productivity is struggling with the quantity of fares. Third, late February has a spike in all three city types to be one of the most active times for ride-sharing in the timeframe analyzed. Rural City's had their highest spike in activity in April.
