---
title: How to Use Ultralytics YOLO11 for Object Detection and Tracking| How to Benchmark | YOLO11 RELEASED🚀
source: https://www.youtube.com/watch?v=-JXwa-WlkU8&list=PL1FZnkj4ad1P9gulU2Ud6y-1m1fKXTPGW
author:
  - "[[Ultralytics]]"
published: 2024-10-04
created: 2026-06-11
description: Get ready for an exciting first look at the amazing new Ultralytics YOLO11! 🚀  In this action-packed video, we’ll show you why YOLO11 is the most powerful object detection model yet! 🎉 With incred
tags:
  - clippings
---
![](https://www.youtube.com/watch?v=-JXwa-WlkU8)

Get ready for an exciting first look at the amazing new Ultralytics YOLO11! 🚀  
  
In this action-packed video, we’ll show you why YOLO11 is the most powerful object detection model yet! 🎉 With incredible benchmarks, game-changing features, and real-world performance that will blow your mind, we’ll walk you through everything you need to know. From top features and live coding demos to adding object tracking, this video is your ultimate guide to mastering YOLO11 💥  
  
🔗 Learn more ➡️ https://docs.ultralytics.com/models/yolo11/  
  
🎯 Key Highlights:  
00:00 - Introduction: We're exploring the new features of YOLO11 and why it's a major update!  
00:23 - Ultralytics YOLO11 Blog Overview: A brief look at the official YOLO11 blog and its resources.  
00:53 - Highlights of YOLO11 Model: Key updates that make YOLO11 stand out.  
01:15 - Benchmarks of YOLO11 vs. Other Models: See how YOLO11 performs against other models.  
02:04 - YOLO11 Model Features and Documentation Walkthrough: Detailed breakdown of YOLO11’s new features.  
02:17 - Key Features of Ultralytics YOLO11: Why this release is a big improvement.  
02:38 - YOLO11 Supported Tasks and Modes: Learn about the different tasks and modes YOLO11 supports.  
02:59 - YOLO11 Models Usage Examples: Real-world examples to get you up and running in no time!  
03:52 - How to Use YOLO11 Models in Python: A step-by-step guide for using YOLO11 like a pro.  
05:31 - Ultralytics Package Upgrade: Make sure to update your Ultralytics package to use YOLO11.  
07:21 - Object Tracking with YOLO11 Models: Easily add tracking to your models for better results.  
08:11 - Conclusion and Summary: Quick recap and what’s coming next for YOLO11.  
  
🔗 Key Ultralytics Resources:  
\- 🏢 About Us: https://ultralytics.com/about  
\- 💼 Join Our Team: https://ultralytics.com/work  
\- 📞 Contact Us: https://ultralytics.com/contact  
\- 💬 Discord Community: https://discord.com/invite/ultralytics  
\- 📄 Ultralytics License: https://ultralytics.com/license  
  
🔬 YOLO Resources:  
\- 💻 GitHub Repository: https://github.com/ultralytics/  
\- 📚 Documentation: https://docs.ultralytics.com/  
  
Stay updated with our latest innovations in AI and computer vision. Subscribe to our channel for tutorials, product updates, and insights from industry experts!  
  
#Ultralytics #YOLO11 #ObjectDetection #ComputerVision #AI #MachineLearning

## Transcript

### Introduction: We're exploring the new features of YOLO11 and why it's a major update!

**0:00** · so we just released the new YOLO 11 model we have a new member in the YOLO family we now have the 11 so we're basically just going to see an overiew over the new model in this video here how we can run inference training and all of that we're going to go through documentation and also what the new highlights of this new YOLO 11 model and then at the end we're going to see some very awesome results so first of all here let's just start inside the blog post we have created whole block prst around this announcement of YOLO 11 so

### Ultralytics YOLO11 Blog Overview: A brief look at the official YOLO11 blog and its resources.

**0:28** · we can basically just go and read more about the detail and so on we're going to go into the documentation in just a second but you can go over the blog post get to know more about YOLO 11 what are different task and so on that you can do we can both do optic detection instant segmentation classification post estimation or in bounding boxes exactly the same as YOLO V8 we're also going to see later on some comparisons and so on and also the benchmarking results so the new YOLO 11 model is basically state-ofthe-art it has both better accuracy but also faster inference speed

### Highlights of YOLO11 Model: Key updates that make YOLO11 stand out.

**1:00** · compared to all the other YOLO models we have YOLO V8 from altic then we have YOLO V 9 and YOLO V 10 and now we also have YOLO 11 which is released here by Ultra litics so we can use all the different times the whole framework and so on it's just a few lines of code and we can now get started with this new model so let just take a look at the Benchmark results before we go into documentation to see how we can run inference and also train this new model so here we do a comparison with all the other versions in the YOLO family so we have Yol V 8 Yol V 9 10 and now also 11

### Benchmarks of YOLO11 vs. Other Models: See how YOLO11 performs against other models.

**1:29** · but also all the other versions We can see the new model highlighted in white here so it has pretty much better performance compared to all the other Yol models both on the average position on the Cobo data set but also the inference speed as we can see down here on the x-axis so this is basically just the latency how long it takes to process a single image so the number of milliseconds per image we can see we

**1:52** · have the ex large large medium small in the Nana version as well so you can go and choose which one you want to use you can take your pre-train model find tune it on your own custom data set again you should go in and do comparisons with all the other versions okay so let's now jump into the allytics documentation if you go inside the models tab you can find all different versions of the YOLO models that we have so now we have this new YOLO 11 model we have the Benchmark results here that we just talked about and then also the key features so basically will just implements a bunch of new features using some of the

### YOLO11 Model Features and Documentation Walkthrough: Detailed breakdown of YOLO11’s new features.

### Key Features of Ultralytics YOLO11: Why this release is a big improvement.

**2:23** · advantages from Yol 8 Yol 10 and so on and combines all of this so now we have the most efficient the most accurate but also the fastest model that can run inference on your own applications and projects so definitely going to check out this model here again it supports all the task and also the mode so inference validation training and you can export the models to whatever optimization Frameworks that we supporting here at Ultra litics

### YOLO11 Supported Tasks and Modes: Learn about the different tasks and modes YOLO11 supports.

**2:48** · detection inst segmentation key Point Post estimation R bounding boxes and classification all those different tasks are supported as well exactly the same as the u v 8 model and if we go down to the user examples you can see how you can use it with just a few lines of code either in python or from the command line interface we just need to specify a few lines of code we need to specify the model that we want to use so now we just have this yolow 11 n or whatever variation that you want to use.

### YOLO11 Models Usage Examples: Real-world examples to get you up and running in no time!

**3:14** · PT then we can specify the data set if you want to do training so we have our data yl file where we need to specify our data directories we have videos covering all of that it will be the exact same pipeline as all the other versions and all the other models videos and so on that we have so it's just swapping out the model testing it out and that's also why it's so good to use to Alo litics framework because you can pretty much just test out four or five different models at the same time with just swapping out the model path so now we

**3:44** · have a high level overview over the new YOLO 11 model we saw some of the details and also the benchmarking results so now going to see how we can use it in our own custom python script I can just take this code snippet there we go from the USIC examples I'll go inside my code editor I have a bunch of wios here that we're going to run through just to see the results and we're do going to do comparisons remember to compare it on your own data sets as well because the new model might not be better compared to some of the other models it really depends on your use cases and also your own data so you just created a brand new

### How to Use YOLO11 Models in Python: A step-by-step guide for using YOLO11 like a pro.

**4:16** · python script here all the way from scratch we have the video files we can just copy paste it in we create a new instance of our Yola model so we just need to specify YOLO 11 we can choose the Nano small medium large and extra large model as we're used to the the data set if you want to train it or if you just want to run inference we can call the predict method if you want to run optic tracking you can just call track instead of predict and we're pretty much good to go we can use the allytics framework in the exact same way as we're used to so right now let's just skip the training part and just specify

**4:46** · the game file we have videos covering all of that you can pretty much just do it in 5 to 10 minutes and then you'll just take your exported model swap it out with the path here and then you're running inference on your own custom train model so now down and grab one of these videos so let's just go inside this folder I'm just going to take the first one or the ice skating I'm going to copy the relative path we can just throw it in directly here so that will be the source for the first argument we want to save it as well the video and we

**5:16** · also want to show it so we set that equal to true so we can see the results while it's act like doing the inference it's going to return a generator where we can go and extract the bounding boxes the confidence scores class IDs and so on and we also have videos covering that so now we're pretty much good to go make sure when you're using the new YOLO 11 model that you upgrade all to lytics either you payment inst it from scratch or you can go and pip upgraded as well just make sure that you have the latest version so you have YOLO 11 if you run

### Ultralytics Package Upgrade: Make sure to update your Ultralytics package to use YOLO11.

**5:44** · into errors this is most likely the problem where you just need to basically just pip install Ultra lytics and then upgrade should be good to go now we'll download the model automatically and you can use it in your own projects and applications so now we're good to go we can just run this python script I'm inside my con environment I'm just going to activate it and now we can run the python script y 11.

**6:09** · p it will open up the wi stream download the model automatically at the first time if you have your own custom trained models you can just SW out the path and it will take that and run inference with that model there we go we see it downloads the weights and we should be able to see the results this is just running on my MacBook CPU you can see this around 40 50 milliseconds per

**6:30** · image where we act like having this latency so this is pretty cool that is almost 20 frames per seconds just on a CPU if you're using like Nvidia gpus and so on it will be able to like run significantly more frames per seconds you can also export into an optimized framework and you can run even faster get up to like three five times inference Speed without really losing accuracy just by using those optimization Frameworks so these were some pretty cool results we just sort ice skating example if you go inside the runs it will also save the video file because we specified that as an argument

**7:01** · you can go in and play around with the arguments confidence scores intersection over unions and so on there we go we have skis snowboard sometimes the person it pretty much just detects the person here in the foreground a lot but also the second one here and pretty much just keeps track of it even though we're not running optic tracking on top of this so it's going to grab one of the other ones so I'm going to grab this one copy the relative path and this is how easy it is to test out on wi dreams and so on and

### Object Tracking with YOLO11 Models: Easily add tracking to your models for better results.

**7:29** · just swap it out it's good to go we can even try to run tracking on top of this so that will just be model.

**7:35** · track there we go we run a python script again and now we're running optic tracking as well so instead of just doing optic detection on each individual frame we add it over time with this tracker that we are using so now we assign an ID to each individual pounding box as well as long as we're tracking it in the frame so we can see car car person let me just pull up the results so now we have track we see we have a person here two persons and we have a person in the middle cars in the background we have some traffic lights and so on again go in and and tune the

**8:07** · confidence score and so on if you're not getting your predictions or find on your specific data set so these are some pretty cool results this is the new STD art model it's both faster more efficient and also better accuracy this is how easy it is to use go ahead and check it out how we can train it we can just call train we need to have our label data we're good to go you can now use it in your own applications and projects