This folder contains data to be used for training ML to classify running steps as left or right.


***OVERVIEW***

StepData:
The StepData folder contains sensor data that has been synchronized with video.  This is the most 
reliable data currently available.  Because it is synced with video we can visually verify whether 
each step is left or right.

BlindStepData:
The BlindStepData contains sensor data recorded before we were able to sync with video.  We cannot 
verify with absolute certainty whether a step is left or right.  I have visually inspected the graph 
of each data set and manually sorted steps into left or right based on the shape of the graph.  I 
tried to be as accurate as possible, but there is a chance I may have mis-identified some of the 
data.

StepData_collate:
I have extracted all of the left and right step files into just two folders to make it easier to 
load into ML training.  These files are duplicates of the the "leftsteps" and "rightsteps" data 
files included in the StepData folder.

BlindStepData_collate:
I have again extracted all of the "leftsteps" and "rightsteps" from BlindStepData for convenience.



***DETAILS***

StepData:
  
  basicstats:
    This file contains basic information about this run's dataset.  
    The first line is the UserName, RunID to access the original data from the database, and the URL 
    of the original data file.  This information can be used to pull data from the server.

    "stepdiff" refers to the number of samples between each step.  This is useful to quickly check
    to make sure there are no obvious errors in the step detection.  For instance if a step was not 
    detected then the max stepdiff will be approximately double what it should be.  If a false peak 
    was detected then the min stepdiff will show a number smaller than a typical step.  Most runners 
    have 30-40 samples per step.

  peaklr:
    This file is a copy of the original "peaks.csv" from the database, but has an extra column added 
    to identify if the peak represents the beginning of a right or left step.

  leftsteps:
    Contains the y-axis data for each left step.  Each row is an individual step.

  rightsteps:
    Contains the y-axis data for each right step.  Each row is an individual step.

  originaldata:
    Contains a copy of the data files from the database.  These files can also be accessed using 
    the information from the first line of the "basicstats" file.

    rotation_matrix.csv:
      The rotation matrix used to correct for the orientation of the sensor on the runner.
  
    rawdata.csv:
      Original raw data recorded by the sensor.  Has not been rotated.

    rotateddata.csv:
      Data has been rotated with the rotation matrix and clipped to only include 25 strides.

    peaks.csv:
      Peaks detected by the app.


  video:
     A side and rear view video of the runner synchronized with with a force portrait animation.  
     This can be used to verify whether a step is left or right.  Video is 100fps, so only 2 seconds 
     are shown to save space.  The FP animation shows a little light blue "tail" of 4 samples 
     indicating the most recent path of the FP.  Because of this the first frame of the video
     is 4 samples after the first peak of the dataset.


  graphs:
    plotpeaks.png
      Shows "odd" and "even" peaks as red and green.  This image can be used as a quick verification 
      to check that peaks have been identified correctly.  Steps are divided from the middle of 
      the "flight" phase of the running gait cycle.  Peaks here are determined as the minimum points 
      of the x-axis (up/down).

    leftStepsGraph.png
      A plot of all the y-axis (left/right) data of steps identified as "left" in this dataset.  This 
      can be used to see the general pattern of left steps, and also to verify that there is not a 
      mistake in the step detection.  All steps should follow a similar pattern unless there is a 
      mistake.  A mistake can be seen if some steps appear to be mirrored which means left and right 
      have been mixed up. 
      
	
   rightStepsGraph.png
      Same as above but showing y-axis data of steps identified as "right".


BlindStepData:
  This folder contains data similar to the above descriptions, but it was collected before we were 
  able to sync with video and before we had implemented rotations to correct for the angle of the 
  sensor placement on the runner.  Rotations shouldn't make much difference for left/right step 
  identification because the sensor is primarily rotated forward and there shouldn't be much rotation 
  left or right.  Since this data is older, we also were not saving peak data to the database, so I 
  have used my own peak detection method which may not exactly correspond to the app.  But again it 
  should be pretty close.  I have manually sorted through this data and identified whether the first 
  peak is "left" or "right".  If all the peaks appear to be correctly identified then the "odd" and 
  "even" steps correspond to "left" and "right" depending on which side.  If there was some problem 
  with the step detection then "odd" and "even" become mixed up and no longer directly correspond 
  with "left" and "right".  In this case I have labeled the dataset as "bad".  I also labeled some 
  sets as "bad" if I was not confident I could identify left or right with just visual inspection 
  of the graph.


  This is the list of each run number and how I coded it:

  {{1, "left"}, {2, "right"}, {3, "bad"}, {4, "left"}, {5, "right"}, {6,
   "left"}, {7, "left"}, {8, "left"}, {9, "left"}, {10, "bad"}, {11, 
  "right"}, {12, "left"}, {13, "left"}, {14, "bad"}, {15, 
  "left"}, {16, "left"}, {17, "left"}, {18, "bad"}, {19, "bad"}, {20, 
  "right"}, {21, "left"}, {22, "right"}, {23, "right"}, {24, 
  "right"}, {25, "right"}, {26, "left"}, {27, "right"}, {28, 
  "right"}, {29, "bad"}, {30, "left"}, {31, "right"}, {32, 
  "left"}, {33, "bad"}, {34, "bad"}, {35, "left"}, {36, "right"}, {37,
   "right"}, {38, "right"}, {39, "bad"}, {40, "right"}, {41, 
  "right"}, {42, "left"}, {43, "bad"}, {44, "right"}, {45, 
  "left"}, {46, "bad"}, {47, "left"}, {48, "left"}, {49, 
  "right"}, {50, "left"}, {51, "left"}, {52, "bad"}, {53, "bad"}, {54,
   "left"}, {55, "left"}, {56, "left"}, {57, "right"}, {58, 
  "left"}, {59, "right"}, {60, "left"}, {61, "left"}, {62, 
  "bad"}, {63, "bad"}, {64, "right"}, {65, "left"}, {66, 
  "right"}, {67, "right"}, {68, "right"}, {69, "left"}, {70, 
  "right"}, {71, "right"}, {72, "bad"}, {73, "bad"}, {74, 
  "left"}, {75, "right"}, {76, "left"}}


StepData_collate:
  I have taken only the good left and right data from the StepData folder for convenience.  It should 
  be possible to easily load up all the left files and all the right files to train ML.  Again, each 
  row of each .csv file is 1 step.

BlindStepData_collate:
  As above, only the good data has been pulled from BlindStepData.  All data coded as "bad" has been 
  omitted.