## Video Annotation Software for Manual Tracking

Currently runs in Python versions 2.7 and 3.6. Common libraries used as dependencies (numpy, pandas, etc.) Also uses tkinter and PIL. Rendering completed annotations uses command-line based ffmpeg. 

Everything runs out of jupyter notebooks, there is a separate notebook for preprocessing/setting up proper directories, annotating the video (for tracking lcoation), and then generating the annotated frames/video. There is also a separate notebook for annotating the video for groups of people. 

All the notebooks live in the top directory. In this top directory, there is a `data` subdirectory. Within the `data` subdirectory, there should be a directory that contains all the video clips.  

*All commands on the command-line should be run from the top directory*

### Preprocessing
`Preprocessing` notebook assumes that all the video clips are stored in one directory and named sequentially (CLIP\_000, CLIP\_001, etc.), as specified above. For each of these video clips, `Preprocessing` will create a directory named `CLIP_###` according to the clip number. Each clip directory will contain the following (empty) subdirectories: `FRAMES`, `DATA`, `VIDEO`, `TRACKED_FRAMES`. 

The last cell of the `Preprocessing` notebook contains a command to save the frames at a framerate of 1FPS from a specific video clip as .png images placed into the corresponding `FRAMES` subdirectory for that clip. *Note: for time/space sake, the clips themselves are never copied/moved from the video directory. Only the frames are saved into the subdirectories, and they must be done one at a time from the command line* 

### Tracking
All manual position tracking is done from the `Tracking-3` notebook, the tracking is done one clip at a time, one person at a time. In the second cell, the user must specify the clip number to be tracked. Naming should follow convention specified above. All the cells above the line (where it says "Do not edit above") can be run without modification. 

After this point, the user can edit parameters for the tracking program as necessary. If the user has already tracked at least one person in this clip, set the `started` flag to `True`. Otherwise, set to `False`. `start` and `end` in the next cell specify the frame numbers between which the user wishes to track an individual. The default is 1, and `n` (where `n` is the total number of frames in that clip as determined automatically in the cells above), respectively. These can be changed if the user knows that a person is only in the clip for the first half, or for a few seconds in the middle, for example. Once the parameters are set, running the next cell will start the tracking. 

#### Doing the tracking  

Once the cell is running, a new window will open with the first desired frame. On this frame, click to specify the location of the individual that is going to be tracked this iteration. If others have been tracked in this clip already, their locations/IDs will show up in grey on this first frame. Once the new target of the tracking has been found, click the spacebar to continue. 

The next frame that opens will be the last desired frame. Find the target again on this frame and click to identify. Once location has been specified, click the spacebar to continue. 

On every subsequent frame, there will be a red circle that appears on the image when the image opens as an approximation, according to a linear interpolation of known locations, as to where the target is. If the circle accurately captures the location of the individual, click the spacebar to continue. Otherwise, use the mouse to click where the individual is and move the circle. Click the spacebar to continue. 

If at any time the user needs to see the first frame again (to verify the individual being tracked, for example), press the 'h' key to show/hide a new window with the first frame. 

The program will automatically open the images necessary and will close by itself upon completion. 

Run the next cell to save the data. The data is saved to a .csv file called `positions.csv` that is placed in the `DATA` subdirectory of the clip's directory. 

### Manual clustering  

`Manual Clustering` allows the user to track groups of individuals in conversation instead of single individuals. The tracking is done one group at a time, one clip at a time. Usage of `Manual Clustering` is similar to that of `Tracking-3`. In the second cell, the user can specify the clip they want to track. 

The main difference between the individual tracking and the group tracking is that the group tracking is done chronologically with a specific step size as a user parameter. Also the user is able to change the size of the oval specifying the group location and size at each frame. 

#### Doing the tracking  

The procedure for tracking the locations and sizes of conversation groups is similar to that of the location tracking specified above. In the parameter specification before the tracking, there is a `stepSize` parameter. This controls the number of frames that are skipped in between the frames that are shown to track. On every frame after the first frame is shown, there will be a "guess" for the location and size of the cluster that is just the exact location and size from the previously indicated frame. 

Once running, the user is also able to change the shape and size of the oval indicating where the cluster is. Similarly to the tracking location, using the mouse click the user is able to move the location of the circle/oval indicating the cluster. The arrow keys can be used to change the shape and size of the oval. The space bar is used to accept the location and move onto the next frame. 

Run the last cell to save the data. The data is saved to a .csv file called `clusters_positions.csv` that is placed in the `DATA` subdirectory of the clip's directory. 


### Generating tracked video  

`Generating Tracked Video` can create and save frames for three types of videos from the data: 
	1.  individual locations 
	2.  group locations
	3.  duplicate tracking verifications (using individual locations) 

1. The individual locations frames are circles overlayed onto the original video's frames identifying the locations of all the individuals tracked using `Tracking-3`. The data is from `positions.csv`, which `Tracking-3` creates. These frames are saved into the `TRACKED_FRAMES` subdirectory, which was created by `Preprocessing`. 
2. The group locations frames are ovals overlayed onto the original video's frames identifying the locations of all the groups tracked using `Manual Clustering`. The data is from `clusters_positions.csv` which `Manual Clustering` creates. These frames are saved into the `CLUSTERED_FRAMES` subdirectory which is created within `Generating Tracked Video`. 
3. Duplicate tracking verifications are used when one or more individuals in the video may have been tracked more than once. These new frames/video can be used to verify which version of the tracking the user would like to keep, assuming that one is better than the others. This requires a list of IDs corresponding to each of the duplicate trackings, and an a priori guess as to which trackings were good and which were bad. The data is from `positions.csv` which `Tracking-3`creates. These frames are saved into the `DUPLICATES` subdirectory which is created within `Generating Tracked Video`. 

For all of the three types of videos, once the new frames are saved, an ffmpeg command is used on the command line to render the actual video. 

In the second cell of `Generating Tracked Video`, the user must specify the clip number they wish to generate new frames/video for. The next two cells can be run without modification. 

Then, the user can run the specific cells corresponding to the type of frames/video they would like to generate. Under each subheading, the appropriate function is called and the frames are generated. There is also a corresponding ffmpeg command that can be run to take the new frames and render them into a video. All the videos will be saved in the `VIDEO` subdirectory. The default video format is .mp4. 
