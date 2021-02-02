## Room Impulse Response (RIR) Similarity Calculator

This repo documents my final project for EE 522 (Immersive Audio Signal Processing) in Spring 2020 taken by Prof. Chris Kyriakakis.

**WHAT IS RIR?**

Room impulse response characterizes the behaviour of a room for a particular source and location of microphone/subject. Given a desired frequency response, it is of potential interest to find a room which ‘sounds similar,’ i.e. modifies the source in a similar way.

Rooms differ in properties such as shape and size, which determine their surface area and volume, and also in the materials they contain, which determines its overall ‘absorptivity.’ It is an interesting problem to determine if rooms of different shape, size, and absorption can sound similar to each other as well.

**OBJECTIVES**

<p align="center">
  <img width="400" height="50" src="https://user-images.githubusercontent.com/21968647/106542952-a67efe80-64b9-11eb-8775-f99f770c340d.png">
</p>

1. Create a dataset of realistic rooms with varying size, shape, and absorption coefficient
2. Calculate and store the impulse response in time and frequency domain for the dataset
3. Given a different room, recommend the three most similar rooms from the dataset
4. Analyze the variation of the frequency response with room size and absorption coefficient

**TOOLS**

<p align="center">
  <img width="600" height="150" src="https://user-images.githubusercontent.com/21968647/106542787-52741a00-64b9-11eb-8840-ba607c114625.png">
</p>

pyroomacoustics is a package for "audio signal processing for indoor applications," developed by EPFL. 

<p align="center">
  <img width="350" height="300" src="https://user-images.githubusercontent.com/21968647/106543044-d0d0bc00-64b9-11eb-81f6-b03cf0f6c438.png">
</p>

pyroomacoustics has a ‘room’ class which takes room parameters as input, notably: coordinates for room corners, room height, absorption coefficient, coordinates of source and mic/mic array, etc. It uses an image source model (ISM), for which the maximum order can be specified. In the experiments below, the order was chosen to be the default, which is 8 reflections.

**DATASET**

In order to design realistic rooms, realistic designs were required. For this purpose, floor plans and designs from the following sources were considered: USC Housing, USC ITS Room Finder, and pinterest.

<p align="center">
  <img width="400" height="250" src="https://user-images.githubusercontent.com/21968647/106543333-59e7f300-64ba-11eb-9de8-ac93578d9ad3.png">
</p>

**ROOM DESIGN WITH PYROOMACOUSTICS**

Based on room designs from the above sources, 495 rooms were designed of varying shape, size, and absorption coefficient. The details are described below.

<p align="center">
  <img width="450" height="500" src="https://user-images.githubusercontent.com/21968647/106564705-7435c700-64e2-11eb-92dc-ff69d04e9343.png">
</p>

Classrooms: \
● 6 floor plans were shortlisted from the above sources \
● Each shape could have 5 possible sizes, designed to be proportional \
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

**SIMILARITY CALCULATION**

Aim: Find the three most similar rooms to the 'query room,' which doesn't have an exact match in the dataset. 

To account for varying lengths of the frequency responses, log energy is grouped into bins. More details: \
● Sampling frequency is 16 kHz, therefore Nyquist frequency is 8 kHz \
● If we choose bins of width 100 Hz, we have 80 bins \
● We sum log energy for each of the 80 bins \
● Now, the varying-length frequency response is an 80-dim vector \
● We can calculate Pearson’s correlation coefficient \

**RESULTS & INFERENCES**

For some shapes, such as 1, 2, 3, and 7, the returned ‘similar rooms’ have the same shapes as the query room. For 2, 3, and 7, this may be because of the unique shape of
the room. For room 4, we could conclude that the enclosure for the door may not result in a distinctive impulse response since all its results have different shapes. 

Rooms 5 and 6 seem to have fairly similar impulse responses.

For room 9: the result is surprising because it implies that an auditorium with a unique shape and position of source and mic sounds similar to a regular-sized bedroom/living room with much smaller dimension and varying absorption coefficient. 

This gives reason to believe that rooms which differ in every aspect, shape, size, and absorptivity, may still sound similar to each other.

**OTHER OBSERVATIONS**

Fixed absorption coefficient and increasing size of room: Some peaks in the frequency response become valleys. Increasing room size leads to some frequency ranges being amplified more than others, though overall energy decreases.

Fixed room size and increasing absorption coefficient: Overall energy in frequency bins decreases as expected. 

**FURTHER EXTENSION**

1. Adding rooms of varying height and curved surfaces, as commonly seen in auditoriums. Both features are currently not supported in pyroomacoustics.

2. Using filterbanks other than evenly spaced bins - such as mel, Bark, or gammatone filterbanks, which are more suited to human perceived hearing.

3. Measuring similarity in the time-domain by using highest peak values from cross-correlation of the room impulse responses.

4. Convolve the similar rooms' impulse responses with speech and guitar at close range and calculate the parameters T60, D50, and C80, and then suggest the best potential application of the room.

5. Conduct a MUSHRA listening test with 5 recordings: (i) the shortlisted top 3 rooms (ii) query room (iii) the room least similar to the query i.e. the worst sample. The subjects can be asked, "How similar do they sound?" on a scale of 1 - 5 (1 = not similar at all, 5 = indistinguishable)

6. Build a supervised machine learning model with features like approximate surface area and volume of each room, absorption coefficient and frequency response. Given a desired impulse, predict room parameters.
