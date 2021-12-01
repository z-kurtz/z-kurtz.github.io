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
First to answer any of these questions we have to have the ability to iterate through the csv. To do this we import the csv module and open the csv. I then set up a list called data which contains all of the values in the csv. Opening the csv as a list allows for us to iterate through the csv as many times as we want.

```
import csv
with open("datasets/digimon.csv", "r") as f:
    data = list(csv.DictReader(f))
```

Thus allows us to then iterate through the csv in the rest of our code to answer the questions asked. 


### Finding the average speed of all Digimon

After opening the csv and setting up a way to iterate through its content I was able to start trying to answer the questions. At first I thought that I would be able to create a dictionary that gave me all of the desired values, but after testing that method out, I felt that it would be more effective to answer each of the questions in separate blocks of code. To find the average speed of all of the Digimon, I created a list to which I added the "Spd" value of every Digimon that is iterated through. Then I divide the sum of all the values in that list by the number of values in the list (otherwise known as its length). The code below demonstrates how the process described was executed. 

```
speeds = []
for digimon in data:
    speed = float(digimon["Spd"])
    speeds.append(speed)
print(sum(speeds)/len(speeds))
```

### Counting the number of Digimon with a specific attribute

I used a function to count the number of Digimon with a specific trait. By having the inputs of this function be the key and value that would define that specific trait in the csv, I was able to design the function to check whether each Digimon had a value that matched the inputed value in the respective defined key. A trait in this case can be defined as a specific value for the key that is defined by the user of this function. Thus I was able to write an if statement that checks whether this is the case, and if it is the case 1 is added to the variable numberOfTrait which keeps track of the number of Digimon with the specific trait. 

```
def count_digimon(key, value):
    numberOfTrait = 0
    for digimon in data:
        if digimon[key] == value:
            numberOfTrait += 1
    return numberOfTrait
```

In order to run this function you just have to print it as follows: 
```
print(count_digimon("Type", "Vaccine"))
```

### Creating a team of three Digimon with at least 300 attack and up to 15 memory

My goal for answering this question was to create a list of Digimon that when any 3 were added together they wwould form a team that would meet these requirements. First I had to think about what each Digimon in my list would need to have in order for this goal to be accomplished. I determined that each Digimon would need to have a memory of less than or equal to five and an attack of greater than or equal to 100 to qualify for the list. This would mean that If I chose any three digimon in the list, they would have a combined memory of less than or equal to 15 and a combined attack of greater than or equal to 300 which are the requirements of the initial question. In order to do this I first initialized the list where the Digimon that satisfied my requirements would be added. Then I created a for loop to iterate through the csv to check whether each Digimon met my requirements. I created variables for each of the things I would be checking for in each digimon and wrote a if statement that checks whether the Digimon meets the requirements that I outlined. If the Digimon meets the reuirements I would add it to the list.

```
digimonTeam = []
for digimon in data:  
    digimonName = digimon["Digimon"]
    memory = digimon["Memory"]
    attack = digimon["Atk"]
    if (int(memory) <= 5) and (int(attack) >= 100):
        digimonTeam.append(digimonName)
```

### In conclusion

This lab tought me a lot about reading a csv and the different techniques that can be used to unpack and analyze the data within them. I also leaned a lot about how when writing code sometimes that simplist solutions are the best ones and give you the same information/response as the complex solutions. Code can be both simple and elegant at the same time. I feel ready to look at more complex csv's and think about more challenging questions regarding them.