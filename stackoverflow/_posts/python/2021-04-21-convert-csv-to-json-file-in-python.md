---
layout: post
title: Convert CSV to JSON file in python
image: https://i.imgur.com/uJPfSWW.jpg
description: >
  I want to parse CSV file line by line and convert it to JSON and send over websocket. I found some code online which converts CSV to JSON as follows
hide_description: true
hide_image: true
comments: true
last_modified_at: 2021-04-21
categories: [stackoverflow]
tags: [python, csv, json, parsing]
---

Convert CSV to JSON file in python

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/56113592/convert-csv-to-json-file-in-python) by [Rushabh Sudame](https://stackoverflow.com/users/9679298/rushabh-sudame)

[![CSV File][1]][1]

Above csv file which contains nearly 2000 rows. 

I want to parse CSV file line by line and convert it to JSON and send over websocket.

I found some code online which converts CSV to JSON as follows:

```py
import csv
import json

csvfile = open('file.csv', 'r')
jsonfile = open('file.json', 'w')

fieldnames = ("FirstName","LastName","IDNumber","Message")
reader = csv.DictReader( csvfile, fieldnames)
for row in reader:
    json.dump(row, jsonfile)
    jsonfile.write('\n')
```
But the problem with above code is that we need to mention the field names to parse the CSV. Since I have more than 2000 rows, it is not a feasible solution.

 Can anyone suggest how to parse CSV file line by line and convert it into JSON without specifying fieldnames?

  [1]: https://i.stack.imgur.com/vqdsC.png

## [Answer](https://stackoverflow.com/a/66071962/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)
## Python CSV to JSON

To convert CSV to JSON in Python, follow these steps:

   1. Initialize a Python List.
   2. Read the lines of CSV file using `csv.DictReader()` function. 
   3. Convert each line into a dictionary. Add the dictionary to the Python List created in step 1. 
   4. Convert the Python List to JSON String using `json.dumps()`. 
   5. You may write the JSON String to a JSON file.

### data.csv

- For the test I made 100.000 lines in a csv file with copy/paste, and the whole conversion takes about half a second with [Apple's M1 Chip][2] while the presented example took only 0.0005 seconds.

[![enter image description here][3]][3]

```csv
column_1,column_2,column_3
value_1_1,value_1_2,value_1_3
value_2_1,value_2_2,value_2_3
value_3_1,value_3_2,value_3_3
```


### Python Program

```py
import csv 
import json
import time

def csv_to_json(csvFilePath, jsonFilePath):
    jsonArray = []
      
    #read csv file
    with open(csvFilePath, encoding='utf-8') as csvf: 
        #load csv file data using csv library's dictionary reader
        csvReader = csv.DictReader(csvf) 

        #convert each csv row into python dict
        for row in csvReader: 
            #add this python dict to json array
            jsonArray.append(row)
  
    #convert python jsonArray to JSON String and write to file
    with open(jsonFilePath, 'w', encoding='utf-8') as jsonf: 
        jsonString = json.dumps(jsonArray, indent=4)
        jsonf.write(jsonString)
          
csvFilePath = r'data.csv'
jsonFilePath = r'data.json'

start = time.perf_counter()
csv_to_json(csvFilePath, jsonFilePath)
finish = time.perf_counter()

print(f"Conversion 100.000 rows completed successfully in {finish - start:0.4f} seconds")
```

### output: data.json

```sh
Conversion 100.000 rows completed successfully in 0.5169 seconds
```

```json
[
    {
        "column_1": "value_1_1",
        "column_2": "value_1_2",
        "column_3": "value_1_3"
    },
    {
        "column_1": "value_2_1",
        "column_2": "value_2_2",
        "column_3": "value_2_3"
    },
    {
        "column_1": "value_3_1",
        "column_2": "value_3_2",
        "column_3": "value_3_3"
    }
]
```


  [2]: https://tech.ssut.me/apple-m1-chip-benchmarks-focused-on-the-real-world-programming/
  [3]: https://i.stack.imgur.com/brR5v.png