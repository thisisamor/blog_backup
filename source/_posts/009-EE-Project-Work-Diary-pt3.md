---
title: EE Project Work Diary (pt3)
date: 2023-07-03 10:53:51
categories: Project
tags:
  - college
  - project
description: My day-to-day work diary of my college project. Part 3 (Week 6)
---
<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> Part 3 of my work diary. What a crazy week. </small>

---

## Week 6

### Jun 19th (Mon)

<mark style="background-color: #7099a7;">Tested the whole rover (fixed issue in MPU-6050 connection, the power problem was solved by uploading a permanent file in FPGA, which allowed the rover to be powered by a battery). However it was tested that the MPU-6050 was not as precise as the stepper motor (it is calibrated and synced by integrating and updating the MPU reading). </mark>

<mark style="background-color: #7099a7;">The camera vision was precise but was hugely affected by the shape of the walls (if the lighting stripe curves by a little, the camera would detect it and provide a wrong result). </mark>

---
### Jun 20th (Tue)

<mark style="background-color: #7099a7;">Tested the led detection results, which were correct. </mark>

<mark style="background-color: #7099a7;">Changed the maze solving algorithm in order to solve the camera vision problem as mentioned above. This was crutial as the rover would be stuck somewhere if the detection results differ from the expected values, and it may ends up with re-exploring the same part of the maze again. </mark>

---
### Jun 21st (Wed)

---
### Jun 22nd (Thur)

---
### Jun 23rd (Fri)



---
<p style="opacity: 0.7;">End Notes: 

<small style="opacity: 0.7;"> That is the end of this project. </small>