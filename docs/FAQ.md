## Friendly Asked Questions (FAQ)

- **I get 'TypeError: cannot convert the series to <class 'int'>' or 'TypeError: cannot convert the series to <class 'float'>' when trying to extract features, generate movies/frames, or when extracting outliers**

>This error comes up when SimBA can't find the resolution and/or pixels per millimeter of your video. This data is stored in the `project_folder/logs/video_info.csv` file. If open this CSV file, make sure that the left-most column named `Video` contains the names of your video files, do not contain any duplicate rows (where video names appear in more than one row), and that the resolution columns and pixels/mm column contains values. 

- **I get a `QHull` (e..g., QH6154 or 6013) error when extracting the features** 

>This error typically happens when a video tracked with DLC/DPK does not contain an animal (or one animal is missing from the video when you are tracking two animals). Because no animal is present in the video, DeepLabCut places all body-parts at the same co-ordinate with a low probability. SimBA tries to use these co-ordinates to calculate metrics from teh hull of the animal, but bacause the coordinates are in 1D rather than 2D, it produces the`QHull` error. To fix it, use the video pre-processing tools in SimBA to trim the videos and discard the portions where no animals are present:

[Tutorial: Batch pre-process videos in SimBA](https://github.com/sgoldenlab/simba/blob/master/docs/tutorial_process_videos.md)

[Tutorial: Video pre-processing tools in SimBA](https://github.com/sgoldenlab/simba/blob/master/docs/Tutorial_tools.md)

- **The frames folder is empty after clicking to extract frames, or my videos have not been generated appropriately**

>Make sure that: 

1. You have installed FFmpeg: [INSTALLATION INSTRUCTIONS](https://m.wikihow.com/Install-FFmpeg-on-Windows)

2. You are running Python 3.6.0 (or python 3.6.10 in conda). 

- **SimBA won't launch - there's an error, with some complaint about Shapely**

Check out these issue threads for potential fixes:

https://github.com/sgoldenlab/simba/issues/12

https://github.com/sgoldenlab/simba/issues/11#issuecomment-596805732

- **SimBA won't start, and there is GPU related errors such as "ImportError: Could not find 'cudart64_100.dll'.**

Make sure;

1. CUDA 10 is installed - https://developer.nvidia.com/cuda-10.0-download-archive
2. cuDNN 7.4.2 for CUDA 10 is installed - https://developer.nvidia.com/rdp/cudnn-archive
3. Tensorflow-gpu 1.14.0 is installed, run: pip install tensorflow-gpu==1.14.0
4. Tensorflow (without GPU support) is *not* installed: run pip uninstall tensorflow
5. Protobuf 3.6.0 is installed: pip install protobuf==3.6.0


- **I get an error when launching the ROI interface - it is complaining about `ValueError: cannot set WRITEABLE flag to True of this array`. It may also have seen `Missing optional dependency 'tables`**

Make sure you are running a later version of pytables(>= version 3.51). Also make sure you have numpy 1.18.1 and pandas 0.25.3 installed. To be sure of this, run:

`pip install tables --upgrade` or `pip install tables==3.5.1`. Pytables 3.5.1 may not be available in conda so do a `pip install tables==3.6.1`. 

`pip install pandas==0.25.3`

`pip install numpy==1.18.1`


- **My videos are very long and can be a pain to annotate in the SimBA annotation GUI, can I skip annotating some frames and still build an accurate classification model based on annotated/not-annotated frames?**

When you first open the SimBA labelling GUI for a new, not previously annotated video (e.g., a video that has **not** gone through [SimBA "pseudo-labelling](https://github.com/sgoldenlab/simba/blob/master/docs/pseudoLabel.md)), SimBA automatically treats all frames in that video as **NOT** containing any of your behaviours of interest. 

If you decide, for example, to only annotate half of the video, then SimBA and any ensuing machine learning training steps will automatically treat the second half of that video as **examples of the absence of your behaviour(s)**. This is all well if you know, for certain, that the second part of the video does not contain any examples of your behavior of interest. 

However, if the second part of your video **does** contain examples of your behaviors of interest, the machine learning algorithm will suffer a lot(!). Because what you are doing in this scenario is giving the machine examples of your behaviors of interest, while at the same time telling the algorithm that it **isn't** your behaviour of interest: finding the relationships between your features and your behavior will be so much more difficult (and maybe impossible) if you give it the computer the wrong information.








