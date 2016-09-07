# Linux interfact for an XBox Controller

This is a linux interface, to integrate the Xbox-Controller directly into your favorite project.
Not all buttons or axes are included. If you require different mappings or axes, please see the linux
programs: **jscal** and **jstest-gtk** to change the mapping on the controller.

It is also recommended, to calibrate the controller before using it. This is also done with: **jscal** or **jstest-gtk**.

This library possibly works with other joystick devices but is only tested with the XBox controller.

# Library Dependencies

Because some vector and quaternion based math is required to calculate the joystick movements, I provide the appropriate libraries needed to use Xbox-Controller here:

**Quaternion library:** [https://github.com/MauriceGit/Quaternion_Library](https://github.com/MauriceGit/Quaternion_Library).

**Vector library:** [https://github.com/MauriceGit/Vector_Library](https://github.com/MauriceGit/Vector_Library).

Just place the .c and .h files in the same directory and compile them with your project.

# Functionality

The only interface and functions you should use, are the documented as function heads in the **mtXboxController.h** file.
More documentation about these functions are found above the apropriate function bodies in **mtXboxController.c**.

Function | Description |
--- | ---
`int mtInitJoyCamera(char* name)` | This function MUST be called before any other. It initialises the usb stream connection.
`void mtCalcJoyCameraMovement(double interval)` | This function should be called in very short intervals, top allow a very smooth joystick handling.
`MTVec3D mtGetJoyPosition()` | The current position is retreived (of the camera or object, controlled via the Controller).
`MTVec3D mtGetJoyUp()` | The current up-vector is retreived (of the camera or object, controlled via the Controller).
`MTVec3D mtGetJoyCenter()` | The current center point is retreived (of the camera or object, controlled via the Controller).

# Usage

This library can be used to control a camera or object in a 3D environment.

The following example uses the Controller to move the camera in an OpenGL context.

```c
// This is done right at the start of your program, before entering the main loop.
char* jsSource = "/dev/input/js0";
if (!mtInitJoyCamera(jsSource)) {
    // Error. Initialisation failed.
}

...

// In GLUT or GLFW, there is a timer, allowing us to get the time interval between
// two frames. We need this interval, to update the camera position via the Controller
float interval = ...
mtCalcJoyMovement(double interval);

...

// Later, we want to set our camera position and direction with gluLookAt().
// The result of our functions fit exactly into this scheme.
MTVec3D cam = mtGetJoyPosition();
MTVec3D center = mtGetJoyCenter();
MTVec3D up = mtGetJoyUp();
gluLookAt (cam.x,    cam.y,    cam.z,
           center.x, center.y, center.z,
           up.x,     up.y,     up.z);

...

// Don't forget, to nicely kill the controller ;)
mtFinishJoyControl();
```


