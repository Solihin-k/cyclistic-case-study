# Cyclistic Case Study
Capstone project for Google Data Analytics Certificate.

## Backgroud Information

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

![bike_sharing](/images/bike_sharing.jpeg)

## The Problem

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, the executive team believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, there is a very good chance to convert casual riders into members with targeted marketing.

## The Objective
- To better understand how annual members and casual riders differ.

- To determine why casual riders would buy a membership.

The insights generated from this analysis would allow Cyclistic to design marketing strategies aimed at converting casual riders into annual members.

## Data

Data on all rides by both members and casual riders from Apr-2020 to Mar-2021 were used. Data can be downloaded [here](https://divvy-tripdata.s3.amazonaws.com/index.html).

## EDA

### Rides per Month
![month](/images/rides_by_month.png)

- There were more rides by members for each month during the 12-months period.
- Jan-2021 had the largest relative difference between both groups, approximately 77%.
- Jul-2020 had the smallest relative difference between both groups, approximately 4.4%.

### Rides per Day of Week
![DoW](/images/rides_by_DoW.png)

- Total rides during weekends is almost distributed equally among members and casual riders.
- Except Saturday, there were more rides by members for each day of week during the 12-months period.

### Rides per Hour of Day
![HoD1](/images/rides_by_HoD_2.png)

- The number of rides between the 2 groups is almost equally distributed during weekends.
- The largest relative difference are for rides that started between 0500hrs and 0800hrs during weekdays.

### Rides by Bike Type
![bike_type](/images/rides_by_bike_type.png)

- There are more rides by members across all three bike types.
- Casual riders prefer docked and electric bikes as compared to classic bikes.
- 81% of rides by casual riders are with a docked bike, as compared to 72% of rides by members.

## Findings

The main difference between casual and member riders are:

- Members tend to use the bikes as their primary mode of transportaion for work, as there were significantly higher percentage of rides by members between 0600hrs to 0800hrs and 1700hrs to 1900hrs on weekdays.
- Casual riders prefer docked bke over the other 2 options. This is likely because they do not have an end destination and wouldn't mind having to return the bike to the original pickup point.
- Casual riders tend to avoid cycling during winter.

## Recommendation

![membership](/images/membership.jfif)


Based on the above findings, here are my top three receommendations:

- Provide discounted rides for new members during weekdays morning and evening rush hour (for a certain period like the first month), to push towards making the bicycles as their primary mode of transportation.
- Form a partnership with a sportswear brand (Decathlon) and provide discounts on cycling gears for members, with special emphasis on winter cycling gears.
- Have a campaign where members can collect points once they cycled a particular route for a certain number of times (e.g. home -> office or office -> home). These points can then be used to redeem rides.
