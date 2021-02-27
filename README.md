# DSLR
The DSLR project uses the Wolverine components on an open board that allows for a larger camera to be used because there are no stock Wolverine mechanical constraints such as clearance to the front panel,  access to the gate etc.
Here is a brief instruction on how to use the system:

Setup the DSLR rig with using teh Wolverine components.
Set the camera to video mode. Adjust the camera alignment and focus.
Start video recording and start the scan in 2 FPS mode.
Once done copy the video to a work directory o a widows machine.

Rename the video to something short like test.mp4
Create the pics directory in work directory
Then run the following ffmpeg command  create a sequence of images in the pics directory.

ffmpeg -i "test.mov" "pics\%03d.jpg" -r 30
ffmpeg -i "1.mov" -pix_fmt rgb24 "clip1\%03d.tiff" -r 30

Note that the FPS is set to 30. Change that to whatever you camera video capture FPS was used.
Now, go the pics directory and look through the sequence. It will consist of frame images and transitions.
Find the first stable image and note what sequence number it is.

Go back to the work directory and ctrate the destination directory and call it something like pics1 or whatever convenoent.
Now create the following dos shell copy script:

===============
SET Input_PATCH1=pics
SET Input_PATCH2=pics1

SET srcstart=86
SET dststart=1
SET count=1200


FOR /L %%i IN (%srcstart%,15,%count%)  DO (CALL :reorder %%i)
GOTO :eof

:reorder
set /a j=(%1-86)/15
copy  %Input_PATCH1%\%1.jpg %Input_PATCH2%\%j%.jpg 
GOTO :eof
===============
Create video from images in the pics1 directory.

ffmpeg -i pics1\%d.jpg -r 20 -vcodec mpeg4 -b:v 8M test1.mp4

You can do the final crop as necessary.
ffmpeg -i test1.mp4 -vf "crop=280:360:740:590" test1_cropped.mp4
 
Further processing is also posible but it is beyond the scope of this short instructions.

It is also possible to use the camera trigger mode for frame-by-frame transfer.
Use the DSLR.zip gerber file to order the  PCB. The cabling instructions are enclosed here in this repository.

