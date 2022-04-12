# **JSON in Manhattan**

## **Introduction**

This program that brings in data from an external file, reads the data, and can filter the data based on specified values, In this program the external file is JSON file hold all data about Manhattan city.

Manhattan project is introducing a service to get data related for Manhattan in new york city like example , zip code and city , address ,neighborhood etc , And display it in different way , And filtering the data , And return specific data.

## **Usage**

**System.IO** : Contains types that allow reading and writing to files and data streams, and types that provide basic file and directory support,
The function need to use in project is **File.ReadAllText**,
```C#
using System.IO;
```
**File.ReadAllText** :Its method reads the entire contents of a text file 

```C#
string Read = File.ReadAllText("data.json");
```

> [data.json](./Lab09-Manhattan/Lab09-Manhattan/bin/Debug/net5.0/data.json) : Is JSON file saved in the project inside **bin/Debug** folder.

**Newtonsoft.Json** : Is namespace provides classes that are used to implement the core services of the framework,It to converts an object to and from JSON, Need to use JsonConvert.DeserializeObject .
```c#
using Newtonsoft.Json;
```
**JsonConvert.DeserializeObject** : Deserializes the JSON to a .NET object. Return Type object.
```C#
RootObject data = JsonConvert.DeserializeObject<RootObject>(Read);
```

> This is the full implementation for function to read data from external file and return object holding all JSON data.

```C#
public static RootObject getData()
            {
                string Read = File.ReadAllText("data.json");
                RootObject data = JsonConvert.DeserializeObject<RootObject>(Read);

                return data;
            }
```

## **Design the Project models** 

**JSON** : Is known as Javascript Object Notation used for storing and transferring data. In our application, you often need to convert JSON string data to class objects.
Now, to convert the above string to a class object, the name of the data properties in the JSON file must match with the name of the class properties. To convert the above JSON file, the class should be as below:

**RootObject Class**

Contain properties type and List<Feature> claa.

```C#
        public class RootObject
        {
            public string type { get; set; }

            public List<Feature> features { get; set; }
        }
```

Also contain filtering data functions :

1. **AllNeighborhoods**: Its void function to print all Neighborhoods data in console, Get all of the neighborhoods in the JSON data .

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

2. **FilterEmpty** : Its void function to print all Neighborhoods data in console without empty Neighborhoods value, Filter out all the neighborhoods that do not have any names .

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

3. **Duplicates** : Its void function to print all Neighborhoods data in console without Duplicates Neighborhoods value, Remove the duplicates from neighborhoods

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

**Feature class**

Contain properties : 
- string type
- Properties properties
- Geometry geometry

```C#
public class Feature
        {
            public string type { get; set; }
            public Geometry geometry { get; set; }
            public Properties properties { get; set; }
        }
```

Now need to have class for each prop in Feature class

**Geometry class**

Contain properties : 
- string type
- Double list coordinates

```C#
public class Geometry
        {
            public string type { get; set; }
            public List<double> coordinates { get; set; }
        }
```
**Properties class**

Contain properties : 
- string zip
- string city
- string state
- string address
- string borough
- string neighborhood
- string county
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

## Code Reference

[Manhattan Project](./Lab09-Manhattan/Lab09-Manhattan/)