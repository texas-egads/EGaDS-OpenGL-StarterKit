
# EGaDS! Super Simple OpenGL CMake Starter Kit

Hey everyone! This is going to be a very simple way of setting up a basic 3D application using **C++** and **OpenGL**. This should give you a quick start in basic computer graphics concepts and allow you to start making cool games/projects using these concepts!

# Installation & Setup

In this step we're going to set up the project so we can start working.

Start by cloning this repository using the below command and follow the instructions below!

```
git clone https://github.com/texas-egads/EGaDS-OpenGL-StarterKit.git
```
> **Note:** Support for **Mac** builds is coming soon, for now it is not supported.

## For Linux
Make sure your system has the **necessary drivers** for your graphics card. Installation instructions for those depend on your distribution. Also make sure you have a **C/C++ compiler**. You can verify this by doing a check to see if you can see the compiler information.
```
g++ --version
```
Next you want to install some **dependencies** on your system, use the below commands to install those third-party libraries!
```
sudo apt update
sudo apt install -y build-essential cmake mesa-common-dev mesa-utils freeglut3-dev libassimp-dev
```
Now navigate to the local repository's main project folder and run these commands to **build** and **run** the project.
```
  mkdir build
  cd build
  cmake ..
  make
  ./EGaDS-OpenGL-StarterKit
```
The last command should do nothing since the **main.cpp** file in the template just returns 0. So you are good to go!

## For Windows
### Visual Studio
- **Install Visual Studio**: If you haven't already, download and install Visual Studio from the [official website](https://visualstudio.microsoft.com/).

- **Install CMake support**: During the installation of Visual Studio, make sure to select the workload "Desktop development with C++", which includes CMake support. If you've already installed Visual Studio without this workload, you can modify your installation by running the Visual Studio Installer again and adding the workload.

- **Open your project folder in Visual Studio**: Open Visual Studio and either create a new project or open an existing one where your CMakeLists.txt file resides. You can do this by selecting "Open a project or solution" from the Visual Studio start page or by going to the "File" menu and choosing "Open" > "Folder...".

- **Configure CMake settings**: Once your project is open in Visual Studio, go to the "CMake" menu and choose "Change CMake Settings". This will create or open a CMakeSettings.json file where you can configure your CMake project settings. Make sure to set the "cmakeExecutable" to the path of your CMake executable if it's not automatically detected.

- **Configure your build target**: In the CMakeSettings.json file, specify the configuration you want to build (e.g., Debug or Release) and the architecture (e.g., x64 or x86).

- **Build your project**: After configuring CMake settings, you can build your project by going to the "Build" menu and selecting "Build All" (or pressing `Ctrl+Shift+B`). Visual Studio will generate the build files using CMake and build your project according to the specified settings.

- **Run and debug your project**: Once the build is complete, you can run and debug your project directly from Visual Studio using the built-in debugger. Set breakpoints in your code, choose the desired startup configuration (e.g., executable), and start debugging by pressing `F5` or clicking the "Start Debugging" button.

### Visual Studio Code
- **Install Visual Studio Code**: If you haven't already, download and install Visual Studio Code from its [official website](https://code.visualstudio.com/).

- **Install CMake Tools extension**: Open Visual Studio Code, go to the Extensions view by clicking on the square icon on the sidebar or pressing `Ctrl+Shift+X`, then search for "CMake Tools" and install it.

- **Open your project folder in Visual Studio Code**: Open Visual Studio Code and either create a new folder for your project or open an existing one where your CMakeLists.txt file resides.

- **Install C++ extension (optional)**: If you haven't already, you might want to install the C++ extension for Visual Studio Code to get C++ language support. You can find it in the Extensions view as well.

- **Configure CMake Tools**: Once you have your project open in Visual Studio Code, press `Ctrl+Shift+P` to open the command palette, type "CMake: Quick Start" and select it. Follow the prompts to set up your project. You'll need to specify the source directory (where your CMakeLists.txt is located) and the build directory (where your build files will be generated).

- **Build your project**: After configuring CMake Tools, you should see the CMake extension appear in the bottom status bar. Click on it and select "Configure" to configure your project, then select "Build" to build it.

Cool! Now you should fully be able to build and run your application! At it's current state it should do nothing since all it does is return `0` in **main.cpp**

<p align="center">
  <img src="images/1/Final-Setup.png" alt="Final Setup" width="500" height="auto"/>
</p>

# Creating a Window

Now that we have our project ready to go, let's start by creating a window. For that, we will be using the **GLFW** library.

## Setting up GLFW
We can first start by including **GLFW** and **GLAD** at the top of our **main.cpp** file and let's include **iostream** as well!
```cpp
#include <iostream>
#include "glad/glad.h"
#include "GLFW/glfw3.h"
```
For those unfamiliar with C++ include formatting, 
- `#include <filename>` makes the preprocessor search in an implementation-defined manner, normally in directories predefined by the compiler. It's usually used for items that are in the standard library and other header files associated with the target platform. 
- `#include "filename"` also makes the preprocessor search in an implementation-defined manner, but it is normally used to include our own header files and third-party libraries. Which in this case, is **GLFW**

Once that's settled, the first thing we need to do is initialize **GLFW** so we can properly use its functionality! 
```cpp
glfwInit();
```
So how **GLFW** works is it creates an **OpenGL** context, but we need to provide it with some information. This done using **Window Hints**. Let's give it some!
```cpp
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
```
What we are doing here is providing the version of our **GLFW** context and specifying our **OpenGL** context. From here let's create a **GLFWwindow*** pointer for our window!
```cpp
GLFWwindow* window = glfwCreateWindow(800, 800, "EGaDS OpenGL Starter Kit", NULL, NULL);
```
`glfwCreateWindow()` Takes in several parameters that define your typical application window. You might be familiar with some of these settings in your everyday use of desktop applications!
- **Width**: This specifies the width of the window in pixels.

- **Height**: This specifies the height of the window in pixels.

- **Title**: This specifies the title of the window, displayed in its title bar.

- **Monitor**: This specifies the monitor to use for full-screen mode. You can pass a pointer to the desired monitor. If you are creating a windowed mode window, pass `NULL`.

- **Share**: This specifies the context to share resources with. You can pass the context of another window if you are sharing resources. For our purposes, this isn't important, so we pass `NULL`.

After this, we can add a bit of error logging and just print a little message to tell us if something goes wrong when creating the window. If that does happen, we can terminate **GLFW**.
```cpp
if (window == NULL) {
	std::cout << "Error creating window" << std::endl;
	glfwTerminate();
}
```

Now, in order to actually use the window we just created, we can use `glfwMakeContextCurrent()` and pass in our **GLFWwindow** pointer. This introduces the window object to the current context in **OpenGL**
```cpp
glfwMakeContextCurrent(window);
```

Then at the end of our application, we can destroy our window and terminate **GLFW**. This just cleans things up!
```cpp
glfwDestroyWindow(window);
glfwTerminate();
```
Now your **main.cpp** file should look something like this!
```cpp
#include <iostream>
#include "glad/glad.h"
#include "GLFW/glfw3.h"

int main(void) {
	glfwInit();

	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

	GLFWwindow* window = glfwCreateWindow(800, 800, "EGaDS OpenGL Starter Kit", NULL, NULL);
	if (window == NULL) {
		std::cout << "Error creating window" << std::endl;
		glfwTerminate();
	}
	
	glfwMakeContextCurrent(window);

	glfwDestroyWindow(window);
	glfwTerminate();
	return 0;
}
```

Let's run our first window! If you have really good eyes, you might notice a window pop up for just a fraction of a second and immediately disappear! This is expected, so don't worry!

<p align="center">
  <img src="images/2/Create-Window-Flash.gif" alt="Create Window Flash" width="500" height="auto"/>
</p>

The reason this happens is because as soon as the window is created, we destroy it and terminate the program. This is why we will need a loop to keep our window open and running!

## Keeping the Window Open

So let's fix that! We can add a while loop at the end of our setup that will check if the window should close, and poll for any additional events triggered by the user.
```cpp
while (!glfwWindowShouldClose(window)) {
    glfwPollEvents();
}
```
<p align="center">
  <img src="images/2/Create-Window-Show.png" alt="Create Window Show" width="500" height="auto"/>
</p>

Now this window is a little boring, so let's apply a bit of color to it! First, let's load the **OpenGL** context and set up our **OpenGL** viewport to match the size of the **GLFW** window. In this case, that is `800` by `800`!
```cpp
gladLoadGL();
glViewport(0, 0, 800, 800);
```
Then We can define the **OpenGL** clear color. This is the color uses to clear the screen when we clear the buffer bit. I'm going to choose some random rgb values, but feel free to pick whatever color you enjoy!
```cpp
glClearColor(0.07f, 0.28f, 0.55f, 1.0f);
```
Now that we set the color, we can clear the **OpenGL** color buffer. This is the call that clears our window with our specified color!
```cpp
glClear(GL_COLOR_BUFFER_BIT);
```

Lastly we need to instruct **OpenGL** to swap the front and back buffers, and to understand this we need to talk a bit about how our graphics are rendered to the screen!

So how screens display our game is it renders images really fast to give us the illusion of motion. These images are called frames and are constantly being drawn.

<p align="center">
  <img src="images/2/Frame-Buffer.png" alt="Frame" width="500" height="auto"/>
</p>

While loading the pixels from the current frame to the screen, the next frame is being drawn in the background and being prepared to load to the screen as the next frame. These frames are stored in things called buffers. As you can see below, the back buffer is drawing the next frame while the front buffer is displaying the current frame on the window.

<p align="center">
  <img src="images/2/Window-Buffer-1.png" alt="Window Buffering 1" width="500" height="auto"/>
</p>

A while later, a swap buffer call is made and the 2 buffers switch jobs and now the formerly back buffer becomes the front buffer, and vice versa. The previous frame is now being overwritten with new information to prepare for the following frame. This process is constantly repeated and is called **buffer swapping**

<p align="center">
  <img src="images/2/Window-Buffer-2.png" alt="Window Buffering 2" width="500" height="auto"/>
</p>

So let's implement this buffer swapping in our C++ application! **OpenGL** actually makes this really simple!
```cpp
glfwSwapBuffers(window);
```

Our resulting **main.cpp** file should now look like this!
```cpp
#include<iostream>
#include<GLFW/glfw3.h>

int main(void) {
	glfwInit();

	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

	GLFWwindow* window = glfwCreateWindow(800, 800, "EGaDS OpenGL Starter Kit", NULL, NULL);
	if (window == NULL) {
		std::cout << "Error creating window" << std::endl;
		glfwTerminate();
	}
	
	glfwMakeContextCurrent(window);

	gladLoadGL();
	glViewport(0, 0, 800, 800);
	glClearColor(0.07f, 0.28f, 0.55f, 1.0f);
	glClear(GL_COLOR_BUFFER_BIT);
	glfwSwapBuffers(window);
	
	while (!glfwWindowShouldClose(window)) {
		glfwPollEvents();
	}
	
	glfwDestroyWindow(window);
	glfwTerminate();
	return 0;
}
```

Awesome! Now if we compile and run this with CMake, we should have a nicely-colored window!

<p align="center">
  <img src="images/2/Final-Window.png" alt="Final Window" width="500" height="auto"/>
</p>

# Drawing a Triangle

Now let's add a triangle to our window! But first let's go over some graphics pipeline concepts in **OpenGL**!

## Graphics Pipeline

So the OpenGL graphics pipeline consists of several steps that essentially takes in a bunch of data, and turns it into a final output frame that our window can display! The data is an array of vertices. However, these vertices are not necessarily just points, they can contain position data, color data, or texture coordinates, etc. 

The first phase of the graphics pipeline is the **vertex shader**. This takes the positions of all the vertices and transforms them, giving you inidividual points.

<p align="center">
  <img src="images/3/Triangle-Graphics-1.png" alt="Triangle Graphics 1" width="500" height="auto"/>
</p>

This position data is then passed to the **shape assembler**, which will take those points and connect them based on the primitive type. A triangle primitive shape assembler would connect 3 points to form a trangle.

<p align="center">
  <img src="images/3/Triangle-Graphics-2.png" alt="Triangle Graphics 2" width="500" height="auto"/>
</p>

Next, we have the **geometry shader**, which can add vertices and create new primitives out of existing primitives. Don't worry, this isn't really important for now!

<p align="center">
  <img src="images/3/Triangle-Graphics-3.png" alt="Triangle Graphics 3" width="500" height="auto"/>
</p>

After our core geometry is laid out, we can enter the **rasterization** step. This turns our neat geometry into a series of pixels that approximates our shape. (Excuse my poor drawing, it's harder than it looks!) As you can see the the diagram below, the boxes represent the pixelated version of the triangle from earlier.

<p align="center">
  <img src="images/3/Triangle-Graphics-4.png" alt="Triangle Graphics 4" width="500" height="auto"/>
</p>

Finally, our shape enters the **fragment shader** process. The rasterized triangle from earlier does not contain any color information. This step maps colors to the pixels either by using color information or by using texture information from texture coordinates.

<p align="center">
  <img src="images/3/Triangle-Graphics-5.png" alt="Triangle Graphics 5" width="500" height="auto"/>
</p>

## Shader Source Code

We will cover shaders and how they work shortly, but for now we will just use a default vertex and fragment shader and store them as `char` pointers.

```cpp
const char* vertexShaderSource = "#version 330 core\n"
"layout (location = 0) in vec3 aPos;\n"
"void main()\n"
"{\n"
"   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"
"}\0";

const char* fragmentShaderSource = "#version 330 core\n"
"out vec4 FragColor;\n"
"void main()\n"
"{\n"
"   FragColor = vec4(0.8f, 0.3f, 0.02f, 1.0f);\n"
"}\n\0";
```
