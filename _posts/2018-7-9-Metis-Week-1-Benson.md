---
layout: post
title: Metis Week 1 -- Project Benson
---

My first week at Metis started off at a blistering pace with the 4-day project titled 'Benson', named for the New York-based detective on the TV show "Law & Order: SVU". For this project, our teams were assigned the task of figuring out which New York subway stations would be the best places to collect email addresses for promotion of an upcoming Women in Tech Gala.

To do this, we collected data from the New York Metropolitan Transportation Authority, which posts [subway turnstile data](http://web.mta.info/developers/turnstile.html) every week.

# Project Design
Our initial approach was to attempt to contact as many people as possible. We considered this to be the safest way to guarantee a large quantity of respondents, even if response rates to email outreach were low. It was fairly straightforward to calculate the total traffic through a station from the MTA dataset -- in most cases, it was as simple as subtracting the initial count of entries or exits on a given day from the final count of entries or exits on that day.

Here's an example of what that looks like for our dataset:
![Top Ten Stations](https://i.imgur.com/JI4iYLy.png)

The above encompasses all turnstile data for the most recent four months on record (Feb 25-June 30, 2018). Even if you aren't super familiar with New York, you might recognize some of the station names! As you can see, Grand Central Station has the most traffic through its subway entry ponits, at around 27 million entries & exits over the four month period.

From here, we narrowed our approach. We decided to look at how traffic through busy stations changed from day to day. Using our data, we took the counts for each station, on each day, and averaged all of these counts for like days of the week (Mondays, Tuesdays, etc.)
![Traffic by Day](https://i.imgur.com/0oEnrPH.png)

It's pretty clear what's going on here, and we were able to confirm the validity of this by plotting lines for each station vs. day on a single chart. From this, we knew we could recommend workdays over weekends.

To break things up even further, we can take a look at what the average workday looks like for a busy station in 4-hour chunks.
![Traffic by Hour](https://i.imgur.com/eyNZ0tF.png)

This is a little more complicated than the last one, but from this we can see that there are two clear peaks: the morning rush hour from 8AM to 12PM, and the afternoon rush hour from 4PM to 8PM.

Our recommendation, then, was that our client should aim to collect email addresses on workdays (Monday-Friday) between the hours of 4PM and 8PM, from the following stations:
* Penn Station (High traffic)
* Grand Central Station (High traffic)
* Herald SQ (Near Amazon and LinkedIn plus high traffic) 
* Times SQ. (Near IBM and Facebook)
* 8th at NYU  (Near IBM, Facebook and NYU)
* Astor Pl  (Near IBM and Facebook)

# Tools
Most of the data analysis for this project was done in Python. We made heavy use of the pandas and numpy libraries for data manipulation and analysis, and matplotlib for data visualization. We also used Tableau for displaying a set of geographical data comparing station locations to the locations of tech companies.

# Data
The MTA turnstile data was provided in one-week periods, in a comma separated value (csv) format. We downloaded 4 months worth of data (16 data files) and concatenated them into a single pandas dataframe for analysis.

Analyzing the data required extending the dataset with extra features, such as:
* datetime
  * The original dataset provided separate date and time columns, which were too unwieldy to process every time we wanted to sort or filter the data. We created a datetime column using pandas.to_datetime that solved this issue.
* entry & exit deltas
  * The MTA data is provided as *cumulative* entries and exits, counting from an unspecified start time. To use this data in a meaningful way, we calculated entry deltas by subtracting the corresponding cumulative value from the previous row, which represents the amount of traffic between the given time and its most recent neighbor.

# Thoughts on Further Work
If we had more time to work on this project, I would like to achieve a greater level of granularity by sorting the data into given sub-areas within each station, such as control areas or turnstile units, to pinpoint locations where email collection can be done. Currently, I think that our recommendations of whole stations are too broad for small groups of people to manage, and I believe that analyzing smaller regions within the data can drive more effective canvassing.

I also think that the effectiveness of our recommendations could be increased by gathering additional data on the geographical density of tech workers in New York. This could be done by gathering data on the employment numbers of tech companies in the city, and plotting those against their locations. A station with a high proportion of tech workers moving through it would likely have a higher response rate than other stations.
