# Bahrain Convenience/Availability

"""

This submission will eventually become your Introduction/Business Problem section in your final report. So I recommend that you push the report (having your Introduction/Business Problem section only for now) to your Github repository and submit a link to it.

Describe the data that you will be using to solve the problem or execute your idea. Remember that you will need to use the Foursquare location data to solve the problem or execute your idea. You can absolutely use other datasets in combination with the Foursquare location data. So make sure that you provide adequate explanation and discussion, with examples, of the data that you will be using, even if it is only Foursquare location data.

This submission will eventually become your Data section in your final report. So I recommend that you push the report (having your Data section) to your Github repository and submit a link to it.

"""
## Introduction

I am a foodie and looking for a place stay in Bahrain. I want to study certain areas in Bahrain and the kind of restaurants that surround them.

I think that a lot of people, not just the youth could benefit from this solution, because the problem isn't finding a decent place to stay in Bahrain, but finding one that best serves their culinary interests perhaps. I mean there are obviously much better factors to look at besides food. However for this problem I want to stick to what I can gain from Foursquare with a free license. Thus, by neatly categorizing areas from their surrounding attributes (such as frequency of coffee shops, closeness to malls etc) I can they can make a better guesstimate of where they might stay next. Foursquare allows us to grab information on venues surrounding a given location, and therefore we will look into the most frequent kind of venues surrounding a given area, and cluster areas based on that. 

## Data Requirements
Firstly I would need to scrap data from Wikipedia to lookup towns and cities in Bahrain. After that, I will then geocode those areas to get coordinates using popular geocoders like OpenStreetMap and Map Quest. Futhermore, we'll leverage the Foursquare API to gather the ratings for all venues associated with an area within 500m of that area's location. We'll then look at various food places and restaurants and extract their categories for further analysis.
I will machine learning to further my analysis on this project.

## Methodology
Methodology section which represents the main component of the report where you discuss and describe any exploratory data analysis that you did, any inferential statistical testing that you performed, if any, and what machine learnings were used and why.

Since our first and ultimate goal revolves working with data and numerical values such as latitude and longitude values for coordinates, I will leverage the Pandas library to store, load and process this data. We gather our areas and locations in a pandas dataframe for easy processing, and attempt to gather coordinates that can be processed efficiently using such a library.
In order to get the best list of places in Bahrain, we will refer to this [page](http://) as a source to begin. 

TK We can extract those pieces of text using a popular webscraper called Beautiful Soup(page link). After scraping it, we form our first dataframe with that data.

We will need to ask questions such as, "does it cover all areas mentioning in the Wikipedia Page?" or "Are there ambigious and confusing elements to those locations like repeated points or missing locations?". Upon ansering these, we can then proceed further.

After we are confident that we have the names of all major areas in Bahrain, we have to geocode them; that is convert them from a name to latitude & longitude values. We can use popular geocoding APIs such as OpenStreetMap and Map Quest. We use both of them to ensure we don't have any discrepancies, or if the name of a particular location doesn't returns an invalid result. After geocoding, we then store the cooridinates into our existing dataframe. Our process of data collection has now ended.

For our methodology, we must utilize mapping libraries like Folium to plot our areas as points on it, so we better visualize our findings. This will aid us in our analysis as we can better synthesize our results.

For our second part, we will look at Foursquare to get us surroudning venus within 500m (an arbitrary radius to avoid collission with other places in Bahrain).
Foursquare is a service that aids us in obtaining location-based data that details venues, reviews and users' who have reviewed the said venues.
For our use-case, we will look at the list of food places within each area in Bahrain.


For the third part of our project, our business problem relies on segmenting areas based on the most common type of food places within the area. This gives us an idea about the type of area it is from a culinary point-of-view. This will allow us to make judgments on whether the food is ideal to our taste or not as a living inhabitant of such a place. We also want factor in the total number of food places within an area since some places in Bahrain may not be ideal to live in if they don't even have enough places to eat.

We use the foursquare API to retunr all restaurants along with their categories, for all areas in Bahrain, and call this `bh_foods`. 
| Index |  Area | Area Latitude | Area Longitude |                  Venue                  | Venue Latitude | Venue Longitude | Venue Category |
|:-----:|:-----:|:-------------:|:--------------:|:---------------------------------------:|:--------------:|:---------------:|:--------------:|
|   0   | A'ali | 26.154454     | 50.527364      | Costa Coffee                            | 26.157464      | 50.525873       | Coffee Shop    |
|   1   | A'ali | 26.154454     | 50.527364      | Chilis Aali                             | 26.152996      | 50.526268       | Diner          |
|   2   | A'ali | 26.154454     | 50.527364      | Starbucks                               | 26.158186      | 50.528168       | Coffee Shop    |
|   3   | A'ali | 26.154454     | 50.527364      | Hospital Resturant (كافيتيريا المستشفى) | 26.153012      | 50.526232       | Restaurant     |
|   4   | A'ali | 26.154454     | 50.527364      | كفتيريا المستشفى                        | 26.153455      | 50.528375       | Restaurant     |

Using this dataframe, we then form a one-hot encoding of the category field that produces new columns for each category. Each record in this table corresponds to a certain venue and a 1 placed in the catergory field for a given area.

|   |  Area | Afghan Restaurant | African Restaurant | American Restaurant | Arepa Restaurant | Asian Restaurant | BBQ Joint | .. |
|--:|------:|------------------:|-------------------:|--------------------:|-----------------:|-----------------:|----------:|---:|
| 0 | A'ali | 0                 | 0                  | 0                   | 0                | 0                | 0         | .. |
| 1 | A'ali | 0                 | 0                  | 1                   | 0                | 0                | 0         | .. |
| 2 | A'ali | 0                 | 1                  | 0                   | 0                | 0                | 0         | .. |
| 3 | A'ali | 0                 | 0                  | 0                   | 1                | 0                | 0         | .. |
| 4 | A'ali | 0                 | 0                  | 0                   | 0                | 0                | 0         | .. |

 We can then use this table and group by area, and then find the mean of the frequency for each category of restaurant along with the `NumberOfFoodPlaces` 

| Index |      Area | Afghan Restaurant | African Restaurant | American Restaurant | Arepa Restaurant | Asian Restaurant | BBQ Joint | Bagel Shop | .. | NumberOfFoodPlaces |
|------:|----------:|------------------:|-------------------:|--------------------:|-----------------:|-----------------:|----------:|-----------:|---:|-------------------:|
|   0   | A'ali     | 0.0               | 0.0                | 0.0                 | 0.0              | 0.00             | 0.000000  | 0.0        | .. | 19                 |
|   1   | Abu Baham | 0.0               | 0.0                | 0.0                 | 0.0              | 0.00             | 0.111111  | 0.0        | .. | 9                  |
|   2   | Al Daih   | 0.0               | 0.0                | 0.0                 | 0.0              | 0.02             | 0.000000  | 0.0        | .. | 50                 |
|   3   | Al Dair   | 0.0               | 0.0                | 0.0                 | 0.0              | 0.00             | 0.111111  | 0.0        | .. | 9                  |
|   4   | Al Garrya | 0.0               | 0.0                | 0.0                 | 0.0              | 0.00             | 0.066667  | 0.0        | .. | 15                 |

Let's call this this `bh_grouped`. Now that we have this processed information, we can analyze this data more clearly by reordering it so that only the 10 most common type of food places for an area are retained.

A new dataframe that is filled with the 10 most common restaurants, and also the number of food places within it, grouped by area. To accomplish this using pandas, we first find the one hot encoding of each area

| Index |      Area | NumberOfFoodPlaces | 1st Most Common Food Place | 2nd Most Common Food Place | 3rd Most Common Food Place | 4th Most Common Food Place | 5th Most Common Food Place | 6th Most Common Food Place | 7th Most Common Food Place | 8th Most Common Food Place | 9th Most Common Food Place | 10th Most Common Food Place |
|------:|----------:|-------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|---------------------------:|----------------------------:|
|   0   | A'ali     | 19                 | Café                       | Restaurant                 | Cupcake Shop               | Coffee Shop                | Middle Eastern Restaurant  | Diner                      | Food                       | Falafel Restaurant         | Breakfast Spot             | Sandwich Place              |
|   1   | Abu Baham | 9                  | Middle Eastern Restaurant  | Fish & Chips Shop          | Mediterranean Restaurant   | Ice Cream Shop             | Restaurant                 | BBQ Joint                  | Donut Shop                 | Cafeteria                  | Pakistani Restaurant       | Noodle House                |
|   2   | Al Daih   | 50                 | Middle Eastern Restaurant  | Dessert Shop               | Bakery                     | Breakfast Spot             | Cafeteria                  | Restaurant                 | Diner                      | Café                       | Sandwich Place             | Italian Restaurant          |
|   3   | Al Dair   | 9                  | Bakery                     | Fast Food Restaurant       | Restaurant                 | BBQ Joint                  | Italian Restaurant         | Afghan Restaurant          | Pastry Shop                | Pakistani Restaurant       | Noodle House               | Movie Theater               |
|   4   | Al Garrya | 15                 | Mexican Restaurant         | Taco Place                 | Burger Joint               | Restaurant                 | BBQ Joint                  | Bakery                     | Food Stand                 | Lounge                     | Pakistani Restaurant       | Noodle House                |

Now we are ready for further analysis and clustering. We will use the `bh_grouped` dataframe since it contains the necessary numerical values for machine learning.
Our feature set is comprised of each of the food place category (100 features) and the `NumberOfFoodPlaces`
Therfore making it 101 features as input.
Our target value will be the cluster label.


For our machine learning use-case, we will use the simplest clustering algorithm to separate the areas which is **K-Means Clustering**; which is an unsupervised machine learning algorithm to serve our purpose.

Before we feed `bh_grouped` into the mode, we will first 

====IGNORE this======

    Introduction where you discuss the business problem and who would be interested in this project.
    Data where you describe the data that will be used to solve the problem and the source of the data.
    Results section where you discuss the results.
    Discussion section where you discuss any observations you noted and any recommendations you can make based on the results.
    Conclusion section where you conclude the report.
