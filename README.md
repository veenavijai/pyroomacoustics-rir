## Room Impulse Response (RIR) Similarity Calculator

This repo documents my final project for EE 522 (Immersive Audio Signal Processing) in Spring 2020 taken by Prof. Chris Kyriakakis.

**What is RIR?**

Room impulse response characterizes the behaviour of a room for a particular source and location of microphone/subject. Given a desired frequency response, it is of potential interest to find a room which ‘sounds similar,’ i.e. modifies the source in a similar way.

Rooms differ in properties such as shape and size, which determine their surface area and volume, and also in the materials they contain, which determines its overall ‘absorptivity.’ It is an interesting problem to determine if rooms of different shape, size, and absorption can sound similar to each other as well.

**Objectives**

<p align="center">
  <img width="400" height="50" src="https://user-images.githubusercontent.com/21968647/106542952-a67efe80-64b9-11eb-8775-f99f770c340d.png">
</p>

1. Create a dataset of realistic rooms with varying size, shape, and absorption coefficient
2. Calculate and store the impulse response in time and frequency domain for the dataset
3. Given a different room, recommend the three most similar rooms from the dataset
4. Analyze the variation of the frequency response with room size and absorption coefficient

**Tools**

<p align="center">
  <img width="600" height="150" src="https://user-images.githubusercontent.com/21968647/106542787-52741a00-64b9-11eb-8840-ba607c114625.png">
</p>

pyroomacoustics is a package for "audio signal processing for indoor applications," developed by EPFL. 

<p align="center">
  <img width="350" height="300" src="https://user-images.githubusercontent.com/21968647/106543044-d0d0bc00-64b9-11eb-81f6-b03cf0f6c438.png">
</p>

pyroomacoustics has a ‘room’ class which takes room parameters as input, notably: coordinates for room corners, room height, absorption coefficient, coordinates of source and mic/mic array, etc. It uses an image source model (ISM), for which the maximum order can be specified. In the experiments below, the order was chosen to be the default, which is 8 reflections.

**Dataset**

In order to design realistic rooms, realistic designs were required. For this purpose, floor plans and designs from the following sources were considered: USC Housing, USC ITS Room Finder, and pinterest.

<p align="center">
  <img width="400" height="250" src="https://user-images.githubusercontent.com/21968647/106543333-59e7f300-64ba-11eb-9de8-ac93578d9ad3.png">
</p>

**Room Design with pyroomacoustics**

Classrooms: \
● 6 floor plans were shortlisted from the above sources \
● Each shape could have 5 possible sizes, designed to be proportionaln\
● Ceiling height was fixed as 10 units (standard ceiling height is about 10 ft) \
● Their absorption coefficients varied between 0.1 to 0.8 in increments of 0.05 \
● No. of rooms = 6 designs * 5 sizes * 15 coeffs = 450 rooms \
● Source (x, y, z) in front left corner at 5 ft height and mic (x2, y2, z2) in the middle of the room at 2 ft height \
● Surface area and vol calculated for each room, approximated for irregular shapes 

Auditoriums: \
● 3 floor plans were added, each with only one size, and ceiling height at 20 units \
● Their absorption coefficients varied between 0.1 to 0.8 in increments of 0.05 \
● No. of rooms = 3 designs * 1 size * 15 coeffs = 45 rooms \
● Source (x, y, z) in the middle of the stage at 7 ft height and mic (x2, y2, z2) in the middle of the audience at 10 ft height \
● Surface area and vol calculated for each room, approximated for irregular shapes

**Similarity Calculation**

**Results and Observations**

**Further Extension**

**<todo>.ipynb**

My implementation.



