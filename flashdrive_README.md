###Getting started 

_Use this the everytime you plug in the flash drive and restart the program._ 

1. Open terminal (cmd + space, then type 'terminal' and hit enter). 
2. Once in terminal, type `flash	` and hit enter. This should take you to the directory containing the flash drive. 
	4. If you get an error when you typed `flash` and it didn't take you anywhere. You can type this instead: `cd ~/../../Volumes/`
3. 	Once you're in this directory, type `cd CLIP_00#` (replace # with the clip number you're working on, it's written on the flash drive if you forget; also if you type `cd CLIP` and hit tab, it should autocomplete)
4. Type `jupyter notebook` to open the flash drive within jupyter and get started. This should open in safari/chrome/etc. in a new tab. 
	5. If you accidentally close this tab, you can re-open it by typing `localhost:8888` into the search bar of the browser that you're in. 
	6. In the terminal window that you typed all the above commands into, you'll see some active logging regarding the status of the jupyter notebook. Don't edit this terminal or close out this terminal. 	

---

### Tracking

#####Some more getting started notes
  
1.  To start the tracking, from the `jupyter notebook` click on **Tracking.ipynb**. 
2. Run all the cells above the part that says **DO NOT EDIT ABOVE THIS LINE**. 
	3. To run a cell, highlight it and press shift+Enter. If it ran correctly without any errors, on the left of the cell you should see `In [#]` where `#` increases as you go through the page. 
4. Once you are ready to start and have run all the cells above the "Do not edit" line, run the cells below. 
5. For now, leave the first cell that has `start = 1` and `end = n` as is and run it
6. Run the next cell. This is going to be the cell that displays the frame(s) from the video where you will do the tracking. 
	7. If you don't see the frame pop-up, check behind the window that you're in. 

##### Instructions

Once you run the program from the cell (described above), you should see a frame in a new window pop up. _Note: If you've already tracked some people, they will **not** show up with the circle to show they've been tracked in this view._  Once you see the frame, pick someone you want to track and click their head. You can click within the frames as many times as you want until you are satisfied with the placement of the circle. For the first few frames, try to get the circle as close to centered on the person's head as possible. Once you're happy with the placement, click the spacebar to continue. You'll see another blank frame, do the same thing on this frame (for the same individual). 

Then, for every subsequent frame that pops up, there will be a red circle drawn on it already that is the program's guess as to where the person is. If this guess is correct (within reason, it doesn't have to be perfect), click the spacebar and continue. If it's not, use the mouse to correct the location of the circle and then click the spacebar when you're ready to continue. General rule of thumb is that the more a person has moved over the course of the video, the more corrections you're going to have to make and the worse the guesses are going to be at the beginning. *Note: if the person hasn't moved all that much, the first guess may be correct. For the purposes of double checking, don't click the spacebar to continue on the first few guesses, try to make some small corrections on the first few at least, however minute*

The program will automatically finish when it's done. Ideally, the frames will close themselves out and you will be taken back to the window with the jupyter notebook. But if you don't see a new frame pop up and just see the rainbow spinning wheel of death, manually head back to the jupyter notebook. If the cell that contains the program code (first line is `positions = {}` ) has finished running, you should see `In [#]` to the left of the cell. If the program hasn't finished running and is actually just being slow, it'll say `In [*]`. 

Once you've finished tracking an individual and are happy with it, go to the next cell in the notebook and run it (it says `saveData`). Once you've run this cell you're all set for that person (you won't see anything beyond the cell running successfully to indicate that you've finished tracking that person). You can now go back to the previous cell and run it to track another person. Rinse + repeat! 

##### Out of the frame

If the person goes out of the frame in the middle of the portion you are tracking them for, click somewhere in the bottom right triangle of the frame that is black/blocked by the ledge. If the program guesses at some point they are out of bounds (by placing the circle in this corner), click spacebar to continue if it's correct and the person is indeed out of the frame. 

**Special case**  
If you are tracking someone who is only in the video for a short period of time somewhere in the middle, find the frame numbers for when they entered and exited the room and change the `start` and the `end` values in the cell right before the program cell to those frame numbers, respectively. 

##### Common errors + troubleshooting

1. If you close out of the program (by closing out the window that contains any of the frames), you'll see an error for that cell. This is fine (provided this is what you intended to do). Just run it again and you'll be all set. 
2. If you see an error when running the program that says something akin to `Image pygame### cannot be found` where ### is any number, then you need to restart the kernel. 
	3. To restart the kernel, in the toolbar of the jupyter page click Kernel. Click "Restart & Clear output". Once the kernel has restarted, go ahead and run all the cells from the beginning. 
4. In the upper right hand corner of the toolbar, it should say "trusted". If you are getting some weird errors or something just isn't running or you get stuck waiting for some cell to run (it says `In [*]` next to it), check to see if the kernel has died. Where it says trusted it will be red and will say "Kernal died". If this happens, just restart the kernel. 
5. If you are tracking someone and can't find them, I recommend watching the rendered video with the frame numbers (you can find this in the flash drive, using finder, it's under `data/CLIP_###/VIDEO`). It's a sped up version of the ten minute clip, and you can scrub through it to find out where your person might be/the general patterns of their movement to know where you want to be looking to find them. If you _really_ can't find them, just say they've gone out of the frame. 

___

### Generating Video 	

There is also code/a separate notebook to generate frames that show the individuals that you've tracked. This whole process takes about 10 minutes so I don't recommend doing it after every time you track someone - I would say re-generating the video after tracking 7-10 people is a nice pace. 

1. There's another notebook in the flash drive called **Generating Tracked Video.ipynb**. Open this and all the cells above the "DO NOT EDIT" line. 
2. The next cell says `saveVideo_MULT`. Run this cell. Wait somewhere between 30-60 seconds, and you should see some output below the cell that says "10". About every minute or so, you should see numbers print out in increments of 10. This is the number of frames that have been generated with all the tracking data that has been collected. 
	3. This cell will be done when you see the number 710 printed. It could take up to 10 minutes, maybe more depending on how much stuff you have running on your computer in the background. Since this is in a different notebook, you can actually do more tracking (in the other notebook) while the frames are being generated. 
4. Once the cell has completed running successfully and you see output all the way up to 710, copy the command in the subsequent cell according to the instructions in the notebook. 
5. Go back to terminal, it should already be running and you should see the jupyter notebook logging in the current tab. Open a new terminal tab (cmd + t). Paste the command here and click enter. You should see a prompt that says `file 'ALL.mp4' already exists. Overwrite? [y/N]`. Type `y` and click enter. It should run without error in about a minute. 
	6. If you get an issue when you paste the command, type `flash`, then enter, and then `cd CLIP_###` (type in the clip num) then enter, and then try pasting the command that you copied again. 

After going through the steps above, you should find the fully rendered video on the flash drive. It'll be in the `data/CLIP_###/VIDEO/` directory, named *ALL.mp4*. This video is the ten minute clip, sped up, and with frame numbers printed in the upper left hand corner. (The quality of the frame number printing isn't great, but good enough that you can make out what number it is if you need to find out when someone is entering/exiting) 

---
### Close up shop

1. Close all the jupyter notebook pages. You don't need to "save" anything, just close 'em out. 
2. Go back to terminal and if you generated some video, close out that terminal tab. 
3. Close out the terminal tab that has the jupyter notebook logging. It'll ask you if you want to terminate the processes. Click yes. 

---
###Other troubleshooting 

If you get to a point where everytihng just freezes and gets stuck, just force quit out of python (the rocketship icon in the dock), close out the jupyter notebook, close out the terminal, and just restart the whole process. 

If you have other errors or there's some glaring feature of the tracking program that is missing that would make your life so much better: email me (caitlyn.19@dartmouth.edu) or text me if you want a faster response (720-879-4236). 
