# Using multi-animal pose-estimation data in SimBA

New developments in pose-estimation packages [DeepLabCut (version >=2.2b5)](https://github.com/DeepLabCut/DeepLabCut/releases/tag/v2.2b5) and [SLEAP](https://sleap.ai/) allows users to track similary looking animals, such as multiple mouse littermates of the same coat-color. In prior versions of these pose-estimation tools tracking several similar looking animals could be difficult as a single body-part (i.e., the "Snout") was intermittently attributed to either animal. In this tutorial we will import multi-animal tracking data into SimBA from videos of two black-coated C57BL6J mice. 

### Step 1: Generate Project Config

If you are coming along from [Scenario 1 - Creating a classifier from scratch](https://github.com/sgoldenlab/simba/edit/SimBA_no_TF/docs/Scenario_1_new.md), then you have already completed the current *Step 1* of the current tutorial so please continue to Step 2. 

In this step you create your main project folder, which will then auto-populate with all the required sub-directories.

1. In the main SimBA window, click on `File` and and `Create a new project`. The following windows will pop up.

![](/images/Create_project_1.PNG "createproject")

2. Navigate to the `[ Generate project config ]` tab. Under **General Settings**, specify a `Project Path` which is the directory that will contain your main project folder.

3. `Project Name` is the name of your project. 
*Keep in mind that the project name cannot contain spaces. We suggest to instead use underscore "_"* 

4. In the `SML Settings` sub-menu, put in the number of predictive classifiers that you wish to create. For an example, in Scenario 1 we would like to create a single classifier. We will enter the number 1. Note that in the real world you would probably want to create multiple classifiers concurrently as this would decrease the number of times a video would need to be manually annotated. For simplicity, we will here create a single classifier.

5. Click <img src="https://github.com/sgoldenlab/simba/blob/master/images/addclassifier.PNG" width="153" height="27" /> a single time, and it creates a row as shown in the following image. In each entry box, fill in the name of the behavior (BtWGaNP) that you want to classify. If you click too many times, as long as you leave the extra boxes empty, all is well.

<p align="center">
  <img width="385" height="106" src="https://github.com/sgoldenlab/simba/blob/master/images/classifier1.PNG">
</p>

### Step 2: Define your Type of Tracking

1. In this current Scenario we are using multi-animal tracking. Click on the `Type of Tracking` drop-down menu in the `Animal Settings` sub-menu and click on `Multi tracking`.  

![](/images/Multi_1.png "createproject")

2. Next, click on the `# config` drop-down menu to specify the pose-estimation body-parts you used to track your animals. If the body-parts you tracked on your animals are not listed, then chose the `Create pose config...` option and head to the tutorial on [how to use user-defined pose-configurations in SimBA](https://github.com/sgoldenlab/simba/blob/master/docs/Pose_config.md). 

![](/images/Multi_animal2.jpg "createproject")

**IMPORTANT**. The images of the animals in the menu above shows two black mice. However, this is for illustrative purposes only and is used for hightlighting the expected body-parts and their order or labelling (1-8). If your data contains any other number of animals (e.g., you have 5 animals rather than 2), and your animals look different (e.g., you have white coat-colored animals rather than black coated animals), but your data contains the pose-estimation tracks of the 8 body-parts in the image, then go ahead and select this `# config` setting. In other words, select the `Multi-tracking # config` setting based solely on the expected body-parts and their order or labelling and NOT the species / number of animals shown in the image.  

In this current scenario we will import the tracking data for 8 body-parts on each of the two black-coated mice. In the `# config` drop-down menu we select `Multi-animals, 8bps`. Once selected, we click on `Generate Project Config`. 

### Step 3: Import your videos

1. Next, click on the `[Import videos into project folder]` tab in the `Project configuration` window. If you have multiple videos, use the `Import multiple videos` submenu to select a folder containing the tracked MP4 or AVI video files. If you have a single video, use the `Import single video` submenu to select a single MP$ or AVI file to import into the project. For more details on importing video files into SimBA projects, see the following tutorial documentations: [1](https://github.com/sgoldenlab/simba/blob/SimBA_no_TF/docs/Scenario1.md#step-2-import-videos-into-project-folder)[2](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial.md#step-2-import-videos-into-project-folder).

>*Note*: When importing multi-animal pose-estimation data into SimBA you **must** import your videos prior to importing your tracking data. The videos will be used to interactively re-organise your pose-estimation data to ensure  that the pose-estimation data for each specific individual falls in the same column, and column order, in all your tracked files (see below). 

### Step 4: Import your tracking data

1. After importing your video files, click on the `[Import tracking data]` tab in the `Project configuration` window. In the `Import tracking data` submenu, click on the `File type` drop-down menu to select your pose-estimation tracking data file type. 

![](/images/Multi_animal3.jpg "createproject")

In this tutorial we have multi-animal tracking data in .H5 file format from DeepLabCut, and we select the `H5 (multi-animal DLC)` option. For more information on the generating .H5 multi-animal tracking files in DeepLabCut, consult the [DeepLabCut tutorials on YouTube](https://www.youtube.com/channel/UC2HEbWpC_1v6i9RnDMy-dfA), the [DeepLabCut GitHub documentation](https://github.com/DeepLabCut/DeepLabCut/releases/tag/v2.2b5), or the [DeepLabCut Gitter channel](https://gitter.im/DeepLabCut/community). SimBA will convert these files during the processes descibed below into organised and more readable CSV files that can be opened with, for example, Microsoft Excel or OpenOffice Calc. 

>*Note I*: If you have generated multi-animal tracking data using [SLEAP](https://sleap.ai/) you should select the `SLP (SLEAP)` option from the `File type` menu. This difference in `File type` selection is the **only user difference** between processing SLEAP and DLC data in SimBA. In fact, despite the different file type names (H5 vs. SLP) they both H5 dataframes that will be converted into readily readable and organised CSV file formats in SimBA which are more accessable to neuroscientst unfamiliar with dataframe container file formats.

>*Note II*: SimBA focusesses on using CSV file formats due to its greater accessability, and SimBA converts alterative pose-estimation tracking formats (e.g., H5, JSON, SLP) behind the hood to CSV to enable a greater number of reserachers to take advantage of SimBA. However, if you are tracking a large number of body-parts (e.g., more than 8 body-parts), on a larger number of animals (e.g., more than 3 animals), in longer videos (e.g., more than 20min / 30 fps) than CSV files are non-optimal due to their relatively slow read/write speed and less-than-ideal compression form (they can become gigabytes after calculating [feature sets](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial.md#step-5-extract-features)). We are currently working on offering the tick-box option in SimBA that would allow users to save their data in file formats with better compression ratio and read-write speed.

2. After selecting the File type, enter the number of animals you are tracking in the `No of animals` entry box. In the current scenario we are tracking two animals and enter the number `2`. After entering the number 2, two further rows will appear asking you to name your two animals. In this tutorial, we name our two animals Resident and Intruder. 

![](/images/Multi_animal4.jpg "createproject")

>*Note*: Avoid spaces and commas `,` in your animal names. If you had planned to use spaces in your animal names, we recommend replacing them with underscore `_`. 

3. Multi animal DLC (DLC verson >=2.2b5) offers two tracking methods (skeleton vs. box). In the `Tracking type` dropdown, select the option you used to generate your tracking pose-estimation. 

4. Next to `Path to h5 files`, click on `Browse` to select the folder that contain you H5 files. The selected folder should contain one H5 file for all the videos imported into the project during Step 3. Once you have selected a folder, click on `Import h5`.

### Step 5: Assigning tracks the correct identities. 

When SLEAP and multi-animal DLC predicts the location of body-parts for multiple animals, the animals are assigned *tracks* or *tracklets* (for more information, see the SLEAP and maDLC documentaion), with one track for each animal. In the current scenario, this means that the DeepLabCut assigned *track 1* may be the Resident, and *track 2* may be the Intruder. It could also be the reverse, and *track 2* is the Intruder and *track 2* is the Resident. The identity of the animal representing *track 1* and *track 2* will also likely shift across different videos. We need to organise the pose-estimation data such as *track 1* is Animal 1 (Resident) and *track 2*  is Animal 2 (Intruder) **in all of our tracking files** and SimBA will do this through an interactive interface. 

1. 





























 
