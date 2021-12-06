---
layout: post
title: Socks Code
subtitle: How I developed the code to answer the questions posed in the socks lab
cover-img: /assets/img/Socks.jpg
thumbnail-img: /assets/img/Sock1.jpg
share-img: /assets/img/path.jpg
tags: [code, explanation]
---

# My Socks Code

The Digimon Lab presented the following 2 tasks to be completed:
1. Use the API to obtain the dataset
2. Answer the following questions about the dataset:
    * Which sock has the most variations?
    * How many socks of each color are there?

In this blog post I am going to walk through the methods I used to accomplish the two tasks presented. So, the best place to start is with the first task.

### Using the API to obtain the dataset
The first task was using the API to obtain the data set and to do this we construct a url pull lines of information from a source in order to compile that information. I set up a base url, an endpoint, and a payload, all of which combine to form the url through which we are able to gather our information.

```
index = 0
BASE_URL = "https://hm-cs.herokuapp.com/"
ENDPOINT = "socks"
payload = {"key": "ArtOfDataKEY123", "idx": index}
response = requests.get(BASE_URL+ENDPOINT,payload)
print(response.text)
index += 1
```

We can use this information to build our csv bit by bit. I used a while loop to accomplish this by iterating through the different possibles url's by changing the index as was seen above. 

```
while response.ok:
    payload = {"key": "ArtOfDataKEY123", "idx": index}
    response = requests.get(BASE_URL+ENDPOINT,payload)
    if response.ok:
        print(response.text)
    index += 1 
```

This while loop stops once there are no more lines left to be read. We know this because once there are no more lines we will get an error and response.ok will return false. With all of this our dataset has now been constructed and we can move on to reading it.




### Reading the dataset

First to read the dataset we have to have the ability to iterate through the csv. To do this we import the csv module and open the csv. I then set up a list called data which contains all of the values in the csv. Opening the csv as a list allows for us to iterate through the csv as many times as we want.

```
import csv
with open("datasets/socks.csv", encoding="utf-8-sig") as f:
    data = list(csv.DictReader(f))
```

Thus allows us to then iterate through the csv in the rest of our code to answer the questions asked. 

### Finding which sock has the most variations

After opening the csv and setting up a way to iterate through its content I was able to start trying to answer the questions. I approached this question in two steps. First I created a dictionary which listed how many times each sock name appeared. This told me how many variations each sock had. I did this using an if statement inside of a for loop that iterated through each sock in the dataset. The if statement checked if the sock was already in the dictionary and if it was added a number to the amount of times it appeared. If it was not already in the dictionary it would create a new sock name and give it a value of 1. 

```
names = {}
for sock in data:
    name = sock["Name"]
    if name in names:
        names[name] += 1
    else:
        names[name] = 1
```

My second step was to iterate through the dictionary called names that I had just created. I created a variable called currentNum that kept track of the current greatest number of variations that any sock had. I also created a list called max_key to keep track of the names of each sock that had the greates number of variations. I iteated through for each name and if it had a value assigned to it that was greater than the current greatest value that was being stored, the lits max_key was cleared and was restarted with the name of the sock with the new greatest number of variations. There is also an elif statement so that if the number of variations that a sock had was equal to teh current greatest number of variations it would also be added to the list.

```
max_key = []
currentNum = 0
for name in names:
    num = names[name]
    if num > currentNum:
        max_key.clear()
        max_key.append(name)
        currentNum = num
    elif num == currentNum:
        max_key.append(name)
```

### Finding out how many socks of each color there are

 First, I iterated through the csv to create a dictionary that used the colors and the keys and the number of times that each color appeared as the corresponding value for those keys. I used one if loop to add color1 to the dictionary and made it so that if the color was already in the dictionary another number would be added to that key to keep track of how many times it had appeared thus far.

```
colors = {}
for sock in data:
    color1 = sock["Color 1"]
    color2 = sock["Color 2"]
    if color1 in colors:
        colors[color1] += 1
    else:
        colors[color1] = 1
```
I used another if loop to add in color2 to the dictionary however with this loop I had to consider that I could not add color2 if it was a duplicate to color1. I had the if statement do just this by checking if color1 was equal to color2 and if it was then no value was added to that color in the dictionary.

```
if color2 in colors and color2 != color1:
    colors[color2] += 1
elif color2 != color1:
    colors[color2] = 1
```

### In conclusion

This lab tought me a lot about using the API to craete a csv and the different techniques write datasets. I got a lot of practive with using while loops and how they can be helpful when you don't know how much data you are dealing with because they will stop when something goes wrong. I feel ready to learn how to build csv's within the code itself and not just by doing it in the terminal. I am very excited for what is coming next.
