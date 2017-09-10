## Demo of Azure IOTHub SDK, CMake, and Conan.io
This repository holds a demo of how to obtain and use the Azure IOTHub SDK for C within a Cmake project (C or C++) in conjunction with [Conan.io](https://conan.io): the modern C/C++ package and dependency manager.  

[Microsoft Azure IoT Hub device SDK for C](https://github.com/Azure/azure-iot-sdk-c)

## Prerequisites

In short, this process requires that CMake and the Conan client be installed. Also, Conan requires Python to be installed. 

Here are the [Official Conan Instructions](http://conanio.readthedocs.io/en/latest/installation.html)  
Here are the [Official CMake Instructions](https://cmake.org/install/)

## Basic setup

After cloning this repo, use the following standard commands to get into a `build` subdirectory.

	$ mkdir build && cd build
	
Next, invoke the Conan client with the `conan install` command, which will read `conanfile.py` and execute it's instructions.  This includes downloading dependencies and generating a file named `conanbuildinfo.cmake`.  This contains all the CMake variables for the dependencies, with the local paths to the directories where the dependencies were downloaded (in your "local conan cache"). 

    $ conan install ..
	
Next, invoke CMake to generate your build files.

	$ cmake ..
	
Finally, invoke CMake to call your local build system and build the project:

    $ cmake --build .
	
## Running the compiled example

This should have compiled the example binary, which you should now be able to run: 

	$ \Debug\iothub_sample_client.exe

## Advanced Notes

**Compiling Options** : This demo is setup to be simple to get started with little or no experience with Conan or CMake.  However, like many C or C++ libraries, the AzureIOTHubSDK and it's transitive dependencies have many compile time options. Conan and CMake are both capable of passing all the options to dependencies in a straightforward way.

**Visual Studio** : VS2017 now has CMake integration.  This project takes special steps to make that work seamlessly with Conan. The key is the use of the following pieces together:
* include(conan_include.cmake) - Added to the top of the `CMakeLists.txt` file. 
* conan_include.cmake - In the same directory as `CMakeLists.txt.
* conan.cmake - In the same directory as `CMakeLists.txt.
* CMakeSettings.json - In the same directory as `CMakeLists.txt.

In summary, VS2017 integration launches CMake for the user and handles the build folders and files automatically.  In order to have VS see the `conanbuildinfo.cmake` file generated by the `conan install` command, we have VS do the `conan install` command for us while it's running it's cmake build.  `conan_include.cmake` is a special CMake script which the Conan.io team created just for this kind of workflow.  This way, when we switch from Release to Debug and/or x86 to x64, conan is automatically downloads or builds (and caches) the appropriate binaries for our dependencies. 

Other Conan examples on the net are perhaps simpler and don't feature any of these extras.  Visual Studio is so common and likely to be used with the Azure IOTHub, it felt extremely relevant to make this demo project turn-key for that case. 

## Feedback

If you have further questions or suggestions about this sample, please feel free to open an issue.  If you'd like to reach out to us privately, please use bincrafters at gmail dot com We hope to add more examples over time. 

Special thanks to Microsoft, Conan.io, and Kitware, for their respective OSS contributions. 
	
### License
[MIT](LICENSE)
