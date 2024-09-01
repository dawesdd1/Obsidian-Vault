Tag: [[Nvidia]], [[ROS]]

This command spins up a Docker container `nvcr.io/nvidia/isaac/ros` and copies in a given dir (also known as a volume) into the container:
```
docker run -it --rm \
-v /home/dawesdd1/repos/hexcopter_ws:/home/catkin_ws/src \
nvcr.io/nvidia/isaac/ros:x86_64-ros2_humble_45d368cdbbe4a484643464d0d492c764 \
/bin/bash

docker run -it --rm \
-v /home/dawesdd1/repos/hexcopter_ws:/home \
nvcr.io/nvidia/isaac/ros:x86_64-ros2_humble_45d368cdbbe4a484643464d0d492c764 \
/bin/bash
```

` - v` : points to the - directory (volumes) to include in container
	e.g.  `-v [local machine dir path]:[container destination path]`

`/bin/bash` : is used to directly enter the container's shell, rather than running the default command.



to spin down the container:
```
exit
```


