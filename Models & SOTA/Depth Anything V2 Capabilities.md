Tags: [[DepthEstimation]], [[ComputerVison]]

Retrieve a depth map 


Depth anything use of camera intrinsic
```
def pixel_to_point(dav2_map, camera_intrinsics):
	return x, y, x

```

however bc depth_anything_v2 is relative, the scaling factor for the depth map is off


##### Fine Tuning Depth Anything v2 (metric distance)
[2D Image to (metric) 3D Point Cloud with DepthAnything: Live Course

Train a universal relative mode (e.g. Depth Anything V2), then fine-tune it on a good dataset for predicting absolute depth, then measure quality on the test set of the training datasets (or datasets in the same domain i.e. NYU-indoor-dataset).