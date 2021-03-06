---
layout: post
title:  "Handle json files in Java"
date:   2021-03-06
excerpt: "json-simple"
tag:
- java
- tutorial
---

# Intro

JSON (JavaScript Object Notation) is a lightweight data-interchange format. It is easy for humans to read and write. It is easy for machines to parse and generate. It is based on a subset of the JavaScript Programming Language Standard ECMA-262 3rd Edition - December 1999. JSON is a text format that is completely language independent but uses conventions that are familiar to programmers of the C-family of languages, including C, C++, C#, Java, JavaScript, Perl, Python, and many others. These properties make JSON an ideal data-interchange language.

# Json-simple API for Java

The Java API to use is [JSON.simple](https://code.google.com/archive/p/json-simple/)

## Installation

Download the [json-simple-1.1.jar](http://www.java2s.com/Code/Jar/j/Downloadjsonsimple11jar.htm) and make it into the project structure.(If using intellj Idea)

## Writing to files

Writing to files is quite standard, it mainly includes two different classes. `JSONObject` and `JSONArray`

### JSONObject
`JSONObect` uses`put()` method to add into one Json object.

- Example:

```java
import java.io.FileWriter;
import java.io.IOException;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class WriteJSONExample
{
    @SuppressWarnings("unchecked")
    public static void main( String[] args )
    {
        //First JSONObject
        JSONObject fruitDetails = new JSONObject();
        fruitDetails.put("Name", "Banana");
        fruitDetails.put("Color", "Yellow");
        fruitDetails.put("Price", 10);

        JSONObject fruitObject = new JSONObject();
        fruitObject.put("fruit", fruitDetails);


        //Write JSON file
        try (FileWriter file = new FileWriter("fruits.json")) {
            //We can write any JSONArray or JSONObject instance to the file
            file.write(fruitObject.toJSONString());
            file.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Result would be
```json
{    
    "fruit": {
            "Name": "Banana",
            "Color": "Yellow",
            "Price": 10
    }
}
```
### JSONArray
`JSONArray` uses`put()` method to add into one Json Array.

Example:

```java
import java.io.FileWriter;
import java.io.IOException;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class WriteJSONExample
{
    @SuppressWarnings("unchecked")
    public static void main( String[] args )
    {
        //First JSONObject
        JSONObject fruitDetails = new JSONObject();
        fruitDetails.put("Name", "Banana");
        fruitDetails.put("Color", "Yellow");
        fruitDetails.put("Price", 10);

        JSONObject fruitObject = new JSONObject();
        fruitObject.put("fruit", fruitDetails);

        //Second JSONObject
        JSONObject fruitDetails2 = new JSONObject();
        fruitDetails2.put("Name", "Orange");
        fruitDetails2.put("Color", "Orange");
        fruitDetails2.put("Price", 5);

        JSONObject fruitObject2 = new JSONObject();
        fruitObject2.put("fruit", fruitDetails2);


        //Add fruits to list
        JSONArray fruitList = new JSONArray();
        fruitList.add(fruitObject);
        fruitList.add(fruitObject2);



        //Write JSON file
        try (FileWriter file = new FileWriter("fruits.json")) {
            //We can write any JSONArray or JSONObject instance to the file
            file.write(fruitList.toJSONString());
            file.flush();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
Result would be
```json
[
    {    
        "fruit": {
                "Name": "Banana",
                "Color": "Yellow",
                "Price": 10
        }
    },

    {
        "fruit": {
                "Name": "Orange",
                "Color": "Orange",
                "Price": 5
        }
    }

]
```

## Reading from files

To read from files, we will need `JSONParser`, `FileReader`


There are several Examples online such as [GeekForGeek's tutorial](https://www.geeksforgeeks.org/parse-json-java/)

But it only provides a code example for reading a JSON Object. (and array within one Object). It troubles me for a while when dealing with a JSON file contains one big JSON Array. Yet it turns out to be easy.

Consider this example

```java
import java.io.FileReader;
import java.util.Iterator;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.*;

class JSONReadExample
{
    public static void main(String[] args) throws Exception
    {
        // parsing file "fruits.json"
        Object obj = new JSONParser().parse(new FileReader("fruits.json"));

        // could also write separately like

        // JSONParser jsonParser = new JSONParser();
        // try (FileReader reader = new FileReader("fruits.json"))


        // typecasting obj to JSONArray since it is a big Array at init
        JSONArray ja = (JSONArray) obj;

        // crafting iterator
        Iterator itr2 = ja.iterator();

        while (itr2.hasNext()) {
            JSONObject curr = (JSONObject) itr2.next();

            String name = (String) curr.get("Name");
            String color = (String) curr.get("Color");

            // PAY ATTENTION The number is type long after parsing
            long price = (long) curr.get("Price");

            /**
             * TODO
             * Use the fields read, store them in ArrayList || system.out.println
             */
        }
    }
}

```

You could also get number of Objects within one JSONArray using `size()`, so you can use for loop if needed.

```java
    JSONArray ja = (JSONArray) obj;
    final int SIZE = ja.size();
```

### Ending

Thats all for the Json-simple tutorial, hopefully it is helpful :)
