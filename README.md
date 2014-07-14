# HandWave
========
> An Android library that allows developers to add touch free capabilities to their mobile applications.

## Overview 
HandWave is a library that allows developers to enable touch-free interactions in their apps. 
HandWave uses the built-in, forward-facing camera on a device to recognize users’ in-air gestures. The API provides developers with access to a variety of touch-free gestures which invoke callback functions when detected. 

Example apps using the HandWave library can be found [here] (https://github.com/kritts/HandWave-Sample-Apps).
Details about how to use the library in your code are included below. 

A video demonstrating capabilities of the library can be found here:
[![Video demonstrating HandWave's function](http://img.youtube.com/vi/ws8UipMmJLE/0.jpg)](http://youtu.be/ws8UipMmJLE)


## Setting up the HandWave library 
I primarily use Eclipse for development therefore the instructions below are for Eclipse.
I plan to add instructions for Android Studio later.

1. Clone the repo: `git clone https://github.com/kritts/HandWave.git`
1. Import `TouchFreeLibrary` **as a library**
    1. Click **File | Import | Android | Existing Android Code into Workspace**
    1. Select the `TouchFreeLibrary` project
    1. Click **Finish**
    1. Right-click on `TouchFreeLibrary`, then click **Properties**
    1. In the project properties window, click the **Android** section
    1. Check the **Is Library** checkbox
    1. Add a reference to the `TouchFreeLibrary` project (click **Remove** to remove any broken references, then click **Add** to add the correct one)
	1. You will also need to need to add opencv as a library. Detailed instructions on how to do so can be found [here](https://github.com/Itseez/opencv/blob/master/doc/tutorials/introduction/java_eclipse/java_eclipse.rst).
		1. It's up to you which version of OpenCV you'd like to use (all of the recent versions should work just fine), but the 2.4.3 is the version I used during development. 


## Using the HandWave library 
There are a few different ways in which the library can be used. 
Complete examples of each can be found [here](https://github.com/kritts/HandWave-Sample-Apps).

### Setting up an activity
You will need to initialize the OpenCV library in your code. To do this, you can add the following lines of code:
I'd recommend adding this line in `onCreate`: 

`OpenCVLoader.initAsync(OpenCVLoader.OPENCV_VERSION_2_4_3, this, mLoaderCallback);`

And then the following lines of code elsewhere:

```java
	/** OpenCV library initialization. */
	private BaseLoaderCallback mLoaderCallback = new BaseLoaderCallback(this) {
		@Override
		public void onManagerConnected(int status) {
			switch (status) {
				case LoaderCallbackInterface.SUCCESS: { 
					mOpenCVInitiated = true; 
					CameraGestureSensor.loadLibrary(); 
					mGestureSensor.start(); 	// your main gesture sensor object 
					 
				} break;
				default:
				{
					super.onManagerConnected(status);
				} break;
			}  
		}
	}; 
```
**For your app to use the front facing camera, you need to add the follow permission to your application's AndroidManifest file:** 

`<uses-permission android:name="android.permission.CAMERA" />`  


For an activity to detect left, right, up, and down gestures, the activity needs to implement a CameraGestureSensor listener.
This can simply be done as follows:  
`public class MainActivity  extends Activity implements CameraGestureSensor.Listener {`

When you implement a CameraGestureSensor listener, there are 4 methods that must be implemented.
The methods are: `onGestureUp`, `onGestureDown`, `onGestureRight`, and `onGestureLeft`. 

As you may expect, those methods are called when their corresponding gestures are detected.

HandWave also supports hands-free clicks. Clicks are generated by hovering a hand over the front-facing camera. 
This functionality can be most easily seen in the video in [the overview](#overview).

For an activity to detect clicks, it must implement a ClickSensor listener. 
As you may expect, this can be done as follows: 

`public class MainActivity  extends Activity implements ClickSensor.Listener {`

When you implement a ClickSensor, there is one method you must implement: `onSensorClick`. 

### Implementing a CameraGestureSensor

Creating an instance of CameraGestureSensor can be done as follows:

`CameraGestureSensor mGestureSensor = new CameraGestureSensor(this);`


For your activity to be a gesture or click listener (which I'd recommend), you should include the following lines of code:

`mGestureSensor.addGestureListener(this);` or `mGestureSensor.addClickListener(this);`

To start the instance of the CameraGestureSensor, use the start method: `mGestureSensor.start();` 

I'd recommend doing this in `onResume`.


To stop the instance of the CameraGestureSensor, use the stop method: `mGestureSensor.stop();` 

I'd recommend doing this in `onPause`.


## Acknowledgements
The code for this library was initially created by Leeran Raphaely (leeran.raphaely@gmail.com). 
It has since been modified to fix bugs in the code and improve the overall speed of the algorithms.  
The changes were made by Krittika D'Silva (krittika.dsilva@gmail.com) and Nicola Dell (nixdell@cs.washington.edu).


