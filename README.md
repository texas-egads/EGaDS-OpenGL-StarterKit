# EGaDS! Super Simple OpenGL CMake Kit

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
  make -j4
  ./EGaDS-OpenGL-StarterKit
```
The last command should do nothing since the **main.cpp** file in the template just returns 0. So you are good to go!

## For Windows
Make sure your system has the **necessary drivers** for your graphics card. As well you will need these **dependencies** for the project:
- CMake: To download and install click [here](https://cmake.org/download/):
- Visual Studio (or text editor of your choice): To download and install VS click [here](https://visualstudio.microsoft.com/vs/community/):

If you're using Visual Studio, open it and select **Open a Local Folder", and navigate to the local repository's main project folder. Wait for CMake, once it's ready, you can click the dropdown for **Build** and select **Build EGaDS-OpenGL-StarterKit.exe**. You can also run it by selecting the green arrow.

<div style="text-align:center;">
  <img src="images/1/VS-Setup.png" alt="VS Setup" style="display:block; margin-left:auto; margin-right:auto;">
</div>
