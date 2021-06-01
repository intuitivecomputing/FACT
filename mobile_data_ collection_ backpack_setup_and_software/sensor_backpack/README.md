# **************************PLEASE REVERT BACK COMMIT 9e1ab6a **************************

# Usage:

## BACKPACK SETUP
1. If not already set up, place battery pack in side pocket and connect USB (5V) and 12V cables. Connect RealSense camera to USB hub in the third lowest port (make sure port is switched on by pressing the nearby button)
2. Press the power switch (furthest from top on the left-hand side) on the TX2 to turn on
3. Find IP address of TX2 (currently should default to Hopkins network). The IP address is not static, so it may be necessary to connect a monitor, mouse, and keyboard and run "ifconfig" to find the IP address.

## FULL USER STUDY WITH TWO PEOPLE
1. Set up the host PC and two backpack TX2s on the same Wifi network. Default is hopkins. If the network is changed, make sure to set the ~/.bashrc of all three computers so that the ROS\_IP is correct and the ROS\_MASTER\_URI corresponds to the host PC.

2. On the host PC, start a roscore

3. On the host PC, ssh into the two backpack TX2s:
```bash
ssh icl@10.194.XXX.XXX
```

4. On the first backpack TX2:
```bash
roslaunch sensor_backpack sensor_backpack_full.launch my_name:=person1
```

5. On the second backpack TX2:
```bash
roslaunch sensor_backpack sensor_backpack_full.launch my_name:=person2
```

6. Wait until the RealSense nodes are up (screen output has stabilized), then verify that the cameras are running. On the host PC, run the following. You should see the publish rate being continuously updated. If the realsense (head_cam) messages are not being received, wait a while (camera sometimes takes a minute to start up) and reset the camera USB port with the port power switch. One reset is usually enough. The publishing rate should be 15 hz.
```bash
rostopic hz /person1/head_cam/color/image_raw/compressed
rostopic hz /person2/head_cam/color/image_raw/compressed
```

7. Help the user to put on the chest mount, backpack, and the head mount. 

8. In a new terminal, run the following command to open Rviz:
```bash
rosrun rviz rviz
```

9. Click "Add" in the bottom right, the "By Topic" tab, "/person1", "/body_cam", "/image_raw", select "compressed" from the dropdown menu next to "Image", then press "Ok" at the bottom. 

10. Click "Add" in the bottom right, the "By Topic" tab, "/person1", "/head_cam", "/color", "/image_raw", select "compressed" from the dropdown menu next to "Image", then press "Ok" at the bottom. 

11. Instruct the user to face the presentation screen. Adjust the cameras so that they are facing the presentation screen.

12. Repeat the step 9 to step 11 except choose "/person2" for the other user. 

13. Close out of Rviz and and terminal used to open it. 

14. Have the user slide the myo band onto the forearm muscle with USB port facing the wrist. Have the user flex their wrist in the sync gesture until they feel a vibration start and stop. Wait until the LED indicator stops blinking. If still blinking after a while, readjust the band and try syncing again. ([See Myo Sync Procedure](https://support.getmyo.com/hc/en-us/articles/200755509-How-to-perform-the-sync-gesture))
* NOTE: Make sure that each user is wearing the Myo band corresponding to the right backpack (labeled with colored tape)

15. In a new terminal on the host PC, ssh into the two backpack TX2s:
```bash
ssh icl@10.194.XXX.XXX
```

16. On the first backpack TX2:
```bash
roslaunch sensor_backpack sensor_backpack_myo.launch my_name:=person1
```

17. On the second backpack TX2:
```bash
roslaunch sensor_backpack sensor_backpack_myo.launch my_name:=person2
```

18. Verify that topics are being published (expected 50hz)
```bash
rostopic hz /person1/myo_raw/myo_emg
rostopic hz /person2/myo_raw/myo_emg
```

19. To start data collection on the host PC, run:
```bash
roslaunch sensor_backpack start_study.launch
```
* This launches nodes that listen to the compressed camera topics and republishes them as uncompressed topics. A rosbag record session is also launched. For person1, the camera topics are:

```bash
/person1/head_cam/color/image_uncompressed
/person1/head_cam/depth/image_rect_uncompressed
/person1/body_cam/image_uncompressed
```

20. Verify that a new active bag file has been created on the host PC in ~/logs. The bag file will have the format [study_name]_[date]-[time].bag, where study_name is a start_study.launch argument (default "study")

21. Do data collection. Once finished, Ctrl-C first from the host PC start_study.launch and then from the launches on the backpack TX2s. Collect the bag file from ~/logs.

22. To extract videos of the head and body cam from the bagfile, start recordings of the image topics as follows (each in a separate terminal). Then start playing back the bagfile. 

```bash
rosrun image_view video_recorder image:=/person1/head_cam/color/image_uncompressed _filename:="[FILENAME1].avi"
rosrun image_view video_recorder image:=/person2/head_cam/color/image_uncompressed _filename:="[FILENAME2].avi"
rosrun image_view video_recorder image:=/person1/body_cam/image_uncompressed _filename:="[FILENAME3].avi"
rosrun image_view video_recorder image:=/person2/body_cam/image_uncompressed _filename:="[FILENAME4].avi"
```
```
rosbag play [PATH TO BAGFILE]
``` 

## REALSENSE ONLY:
1. set ~/.bashrc ROS environment variables as desired. To run and record locally, set ROS_HOSTNAME and ROS_MASTER_URI to localhost

2. Launch the camera (also begins bagfile recording):
```bash
roslaunch sensor_backpack sensor_backpack_rs.launch
```
   
* To wait to record bagfile later, run:
```bash
roslaunch sensor_backpack sensor_backpack_rs.launch record:=false
```
   
* And later run:
```bash
roslaunch sensor_backpack sensor_backpack_rs.launch record:=true rs:=false
```
  * Note that the default parameters my_name:=person1, rs_name:=rs will result in topics and tf prefixes /[my_name]/[rs_name]/... i.e. /person1/rs/...

3. Wait until realsense node is up, then verify that topics are being published, e.g. run:
```bash
rostopic hz /[my_name]/[rs_name]/color/image_raw
```

* to check that images are being published (should be ~15hz). If communicating over a wifi network, the receive rate on the listener side may be lower. To address bandwidth issues, use the compressed topic:
```bash
rostopiz hz /[my_name]/[rs_name]/color/image_raw/compressed
```
* If no messages received, wait a while (camera sometimes takes a minute to start up) and reset the camera USB port with the port power switch. One reset is usually enough.

4. Verify that a new active bag file has been created in ~/logs. The bag file will have the format [my_name]_[date]-[time].bag.

5. Do data collection. Once data collection is done, Ctrl-C from the roslaunch(es) to exit, and collect the bag file (e.g. by scp).
