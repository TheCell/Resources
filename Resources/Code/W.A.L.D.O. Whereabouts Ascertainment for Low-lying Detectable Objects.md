---
tags:
  - software
  - utility
created: 21.04.2025
up: "[[Code]]"
---
[https://github.com/stephansturges/WALDO](https://github.com/stephansturges/WALDO)

![[68747470733a2f2f692e696d6775722e636f6d2f68476768724c6e2e6a706567.jpg]]

## Welcome to the WALDO v3.0 public release

WHAT IS WALDO?

WALDO is a detection AI model, based on a large YOLO-v8 backbone and my own synthetic data pipeline. **The model is capable of detecting these classes of items in overhead imagery ranging in altitude from about 30 feet to satellite imagery!**

Output classes:

0 -> 'LightVehicle' --> all kinds of civilan cars, including pickup trucks, vans etc... 🚗🏎️🚓🚐🚑  
1 -> 'Person' --> people! all kinds of people including ones that are on bikes or swimming in the sea 🧍‍♀️🕺💃🧜🏽‍♀️🏂🧞  
2 -> 'Building' --> all kinds of buildings 🕌🏛️🏭🏡  
3 -> 'UPole' --> utility poles, power poles, anything thin and sticking up that you should avoid with a drone 🎏  
4 -> 'Boat' --> boats, ships, canoes, kayaks, surf boards... all the floaty stuff 🚢🏄  
5 -> 'Bike' --> bikes, mopeds, motorbikes, all stuff with 2 wheels 🚲  
6 -> 'Container' --> shipping containers, including on the back of an articulated truck... 📦🏗️  
7 -> 'Truck' --> large commercial vehicles including articulated trucks or big box-on-chassis delivery trucks 🚚  
8 -> 'Gastank'--> cylindrical tanks such as butane tanks and gas expansion tanks, or grain silos... pretty much anything that looks cylindrical for storing liquids 🫙  
10 -> 'Digger' --> all kinds of construction vehicles, including tractors and construction gear 🚜  
11 -> 'Solarpanels' --> solar panels ▪️🌞▪️  
12 -> 'Bus' --> a bus 🚌  

--> In general the lower the class number the better-trained you can expect it to be. For users of previous versions of WALDO: note that I removed the military class and smoke detection. This is meant to be a FOSS tool for civilian use and I don't want to pursue making it work for military applications.

---

WHERE IS WALDO?

Right here on HF -> [https://huggingface.co/StephanST/WALDO30](https://huggingface.co/StephanST/WALDO30)

Note there are a couple more models that have slightly better performance over on Gumroad here: [https://6228189440665.gumroad.com/l/WALDOv3](https://6228189440665.gumroad.com/l/WALDOv3) Those are for sale as a kind of sponsorship for the project: if you find value in the free ones here you can buy those for a nice little performance boost... but it's entirey up to you!

![[68747470733a2f2f692e696d6775722e636f6d2f564b61354e4e352e706e67.png]]

In both cases the actual files are MIT license and you can freely share them, so if someone gives you the ones from Gumroad you are free yo use them including commercially. It's really just a way to offset some of the work and compute that went into making this project and keeping it FOSS.

---

WHAT IS IT GOOD FOR?

People are currently using versions of WALDO for:

1. disaster recovery
2. monitoring wildlife sanctuaries (intruder detection)
3. occupancy calculation (parking lots etc..)
4. monitoring infrastructure
5. construction site monitoring
6. traffic flow management
7. crowd counting
8. some fun AI art applications!
9. drone safety (avoiding people / cars on the ground)
10. lots of other fun stuff...

The main reason for me to make WALDO free has in fact been discovering all these cool applications. Let me know what you build!

---

FOR AI NERDS !

It's a set of YOLOv8 model, trained on my own datasets of synthetic and "augmented" / semi-synthetic data. I'm not going to release the dataset for the time being.

The weights are completely open, allowing you to deploy in any number of ways this time!

---

HOW CAN I START WITH WALDO?

Check out the boilerplate code in the repo to run the models and output pretty detections using the wonderful Supervision annotation library from Roboflow :)

---

GOING DEEPER

Of course if you know your way around deploying AI models there is a lot more you do with this release, inclusing:

1. fine-tuning the models on your own data (if you know what you are doing, this is probably your starting point)
2. building a nicely optimized sliding-window inference setup that works nicely on your edge hardware
3. quantizing the models for super-duper edge performance on cheap devices
4. using the models to annotate your own data and train something of your own!

Enjoy!

---

PREVIOUS VERSIONS

I am retiring the old versions, this is the only one that will stay online.