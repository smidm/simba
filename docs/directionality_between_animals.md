# Calculate directionality across animals in SimBA

In videos of social interactions between animals it may be valuable to know how much time each animal spend directing towards each other, and we calculate this with appropriate pose-estimation data in SimBA (the pose-estimation tracking of the *ears* and the *nose* of each animal, or the equivalent in other species, is required for this to work. See more information on this pre-requisite below). See the end of this tutorial for visualizations and a better idea of this type of data. For example, we can use SimBA to get measures, in seconds, of much time Animal 1 was directing towards Animal 2, and how much time Animal 2 was directing towards Animal 1 (.. and so on for all the relationships for all the animals tracked in the video). We can also generate visualizations of these metrics in SimBA so that we can confirm that the "directionality" data that we see in the output summary CSV files and dataframes are accurate. 

>Note 1: Firstly, and importantly, SimBA does not calculate actual gaze (this is *not* possible using only pose-estimation data). Instead, SimBA use a proxy calculation for estimating if the animal is directing toward another animal, or [a user-defined region of interest](https://github.com/sgoldenlab/simba/blob/master/docs/ROI_tutorial.md#part-3-generating-features-from-roi-data), using the nose coordinate and the two ear-coordinates (or the equivalent coordinate on any other nimal species). For more information on how SimBA estimates the location of the animals *eyes*, and how SimBA estimates if an object, or other animal, is within the *line of sight*, check out the [SimBA ROI tutorial - Part 3](https://github.com/sgoldenlab/simba/blob/master/docs/ROI_tutorial.md#part-3-generating-features-from-roi-data). 

>Note 2: These directionality mesurements require the pose-estimation tracking data of the *ears* and the *nose* of each animal. See the below image and legend more information 

![](https://github.com/sgoldenlab/simba/blob/master/images/Directionality_ROI.PNG)


# Before analyzing directionality in SimBA

To analyze directionality data in SimBA (for descriptive statistics, machine learning features, or both descriptive statistics and machine learning features) the tracking data **first** has to be processed the **up-to and including the *Outlier correction* step described in [Part 2 - Step 4 - Correcting outliers](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#step-4-outlier-correction)**. Thus, before proceeding to calculate directionality-based measurements between animals, you should have one CSV file for each of the videos in your project located within the `project_folder\csv\outlier_corrected_movement_location` sub-directory of your SimBA project. 

Specifically, for working with directionality between animal in SimBA, begin by (i) [Importing your videos to your project](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#step-2-import-videos-into-project-folder), (ii) [Import the tracking data and relevant videos to your project](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#step-3-import-dlc-tracking-data), (iii) [Set the video parameters](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#step-3-set-video-parameters), and lastly (iv) [Correct outliers](https://github.com/sgoldenlab/simba/blob/master/docs/Scenario1.md#step-4-outlier-correction)

# Calculate what other body-parts and what other animals, are in the line of sight for specific animal.   

1. In the main SimBA console window, begin loading your project by clicking on `File` and `Load project`. In the **[Load Project]** tab, click on `Browse File` and select the `project_config.ini` that belongs to your project. 

2. Navigate to the ROI tab. On the right hand side, within the `Analyze distances/velocity` sub-menu, there are two buttons - `Analyze directionality between animals` and `Visualiza directionality between animals`.

![](https://github.com/sgoldenlab/simba/blob/master/images/Directionality_99.PNG)

3. We will begin clicking on the `Analyze directionality between animals` button. Clicking on this button will calculate how much time each animal spend directing towards each other using within all of your videos represented as CSV files located in your `project_folder/csv/outlier_corrected_movement_location` directory. For more information on how SimBA calculates directionality, check out [this schematic](https://github.com/sgoldenlab/simba/raw/master/images/Directionality_ROI.PNG). This schematic depicts a user-defined ROI, but exactly the same methods are applied here using body-parts rather than a user-defined ROI region. You can follow the progress in the main simBA terminal window. 

Once complete, several new files will be generated in your SimBA project directory: i) if you look within the `project_folder/csv/directionality_dataframes` directory, you should see one file for each of the video in your project. The files contain one row for each of the frames in your video. For each animal and targeted (directing towards) body-part, there will be a total of 5 columns: the **first** of these five columns (column *Simon_directing_JJ_Nose* in the example below) contains 0 and 1 and represents a boolean. If the animal directed towards the body-part in that frame (Simon seeing JJ's nose) then the column reads `1`, if the animal did not direct towards that body-part in that frame (Simon not seeing JJ's nose) then the column reads a `0`. Thus, in this example, animal JJs nose was within the line of sight of animal Simon in frame 23-40, but not within the line of sight in frame 0-22. Next, the following four columns are primarily saved for later visualization purposes. The **second** and **third** column (*Simon_directing_JJ_Nose_eye_x* and *Simon_directing_JJ_Nose_eye_y*) stores the coordinates of animal *Simon's* directing eye when the specific body-part is in the line of sight. If the specific body-part is not within the line of sight (JJ's nose), then the specific row for these columns will read `0`. Finally, the **fourth** and **fifth** column (*Simon_directing_JJ_Nose_bp_x* and *Simon_directing_JJ_Nose_bp_y*) stores the coordinates of the body-part being observed by animal Simon. If the specific body-part is not within the line of sight (JJ's nose), then the specific row for these columns will read `0`. 

![](https://github.com/sgoldenlab/simba/blob/master/images/Directionality_98.PNG)

Finally, SimBA generates a CSV log file containing summary statistics on how much time each animal spent looking at each other. This file can be found in the `project_folder/logs` directory. This filename of this file is time-stamped, and have a name that may look something like this: `Direction_data_20200822151424.csv`. The content of this file may look like this (if you are tracking 5 animals. if you are tracking fewer animals, then the file will contain fewer columns):

![](https://github.com/sgoldenlab/simba/blob/master/images/Directionality_97.PNG)
**(click on image to enlarge)**

In this file there will be one column for each animal relationship, and one row for each analysed video. In the example screenshot above, the first column header reads *JJ_directing_Simon_s* and contains the time, in seconds, the animal with the ID *JJ* spent directing towards the animal with the ID *Simon* (92.2s in Video1). In column F (or column 5), with the header *Simon_directing_JJ_s*, we can read the time, in seconds, the animal with the ID *Simon* spent directing towards the animal with the ID *JJ* (107.3s in Video1). Indeed, styding the directionality results in all of the four of the videos in the example screenshot above, Simon seems a little more intrested in looking at JJ than JJ is intrested in looking at Simon throughout. 

>Note: These summary statistics are calculated based on all body-parts for each animal. If an animal can see any body-part belonging to another animal, then the animal can see the other animal. For example, if the animal with the ID Simon only has the tail-base of the animal with the ID JJ in the *line of sight* in one frame, and all body-parts belonging to the animal with the ID JJ in the *line of sight* in second frame, then both frames are counted as *JJ* being in the *line of sight* of Simon. 

4. Next, we may to visualize these directionality data - for peace of mind - that the metrics seen in the output desriptive statistics and CSV files are plausable. 


<img src="https://github.com/sgoldenlab/simba/blob/master/images/Testing_Video_3_short.gif" width="425"/> <img src="https://github.com/sgoldenlab/simba/blob/master/images/Together_2.gif" width="425"/>


For higher-quality examples of the expected visualization results, visit the [SimBA YouTube playlist](https://www.youtube.com/playlist?list=PLi5Vwf0hhy1R6NDQJ3U28MOUJPfl2YWYl), the examples the gifs above where generated from can be found here ([Example 1 - 2 mice](https://www.youtube.com/watch?v=tsOJCOYZRAA&list=PLi5Vwf0hhy1R6NDQJ3U28MOUJPfl2YWYl&index=19&t=0s), [Example 2 - 5 mice](https://www.youtube.com/watch?v=d6pAatreb1E&list=PLi5Vwf0hhy1R6NDQJ3U28MOUJPfl2YWYl&index=20&t=0s)). 
































