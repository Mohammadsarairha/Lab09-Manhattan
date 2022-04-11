# **JSON in Manhattan**

## **Introduction**

This project is introducing a service to get data related for Manhattan in new york city like example , zip code and city , address ,neighborhood etc , And display it in different way , And filtering the data , And return specific data.

## **How can use it** 

### **JSON Formatter**

First of all this data about Manhattan it stored in JSON file :

JSON : Is a JSON file is a file that stores simple data structures and objects in JavaScript Object Notation (JSON) format EX:

```JSON
{
    "type": "FeatureCollection",
    "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -73.997177,
          40.750633
        ]
      },
      "properties": {
        "zip": "10001",
        "city": "New York",
        "state": "NY",
        "address": "",
        "borough": "Manhattan",
        "neighborhood": "Chelsea",
        "county": "New York County"
      }
    },
}
```

Now need to covert this JSON file to object and classes to can use it in the project This is example to how convert JSON data to classes

```C#
        public class RootObject
        {
            public string type { get; set; }

            public List<Feature> features { get; set; }
        }
```
features : Is a list of object of anouther properties.

```C#
public class Feature
        {
            public string type { get; set; }
            public Geometry geometry { get; set; }
            public Properties properties { get; set; }
        }
```
Now need to have class for each prop in Feature class
```C#
public class Geometry
        {
            public string type { get; set; }
            public List<double> coordinates { get; set; }
        }
```
properties class

```C#
public class Properties
        {
            public string zip { get; set; }
            public string city { get; set; }
            public string state { get; set; }
            public string address { get; set; }
            public string borough { get; set; }
            public string neighborhood { get; set; }
            public string county { get; set; }
        }
```

All classes is ready to visualize the JSON data.


### **Get Data from JSON file**

Get data from file in the project should but the JSON file in **bin/Debug** file to accesse it from the project .

To can read data from JSON file need to use System.IO.

```C#
string Read = File.ReadAllText("data.json");
```
To Converte data bettween string to JSON need to use DeserializeObject.

```C#
RootObject data = JsonConvert.DeserializeObject<RootObject>(Read);
```
### **Linq Query**

There is some Chaining Linq Queries to get data from JSON file .

1. Get all of the neighborhoods in the JSON data .

```C#
public static void AllNeighborhoods(RootObject rootObject)
            {
                var data = rootObject.features.Select(t => t.properties);
                var allneighborhood = data.Select(n => n.neighborhood);
                foreach (var item in allneighborhood)
                {
                    Console.WriteLine(item);
                }
            }
```
- Output : 147 neighborhoods.

2. Filter out all the neighborhoods that do not have any names 

```C#
public static void FilterEmpty(RootObject rootObject)
            {
                var data = rootObject.features.Select(t => t.properties);
                var neighborhood = data.Where(n => n.neighborhood != "").Select(n => n.neighborhood);
                foreach (var item in neighborhood)
                {
                    Console.WriteLine(item);
                }
            }
```

- Output : 143 neighborhoods. 

3. Remove the duplicates from neighborhoods

```C#
public static void Duplicates(RootObject rootObject)
            {
                var data = rootObject.features.Select(t => t.properties);
                var neighborhood = data.Select(n => n.neighborhood).Distinct();
                foreach (var item in neighborhood)
                {
                    Console.WriteLine(item);
                }
            }
```

- Output : 39 neighborhoods.

## Summary

Standard query operators in query syntax is converted into extension methods at compile time. So both are same.

Standard Query Operators can be classified based on the functionality they provide. The following table lists all the classification of Standard Query Operators:

| Classification    | Standard Query Operators |
|-----------|-----------------|
| Filtering | Where, OfType|
| Sorting OrderBy    | OrderBy, OrderByDescending, ThenBy, ThenByDescending, Reverse           |
| Grouping  | GroupBy, ToLookup|
| Join  | GroupJoin, Join|
| Projection  | Select, SelectMany|