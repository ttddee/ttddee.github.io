## Building OpenImageIO on Windows

We will be using Windows 10 64 bit but the process is similar for 32 bit or older versions of Windows.

What you will need:

- [CMake GUI](https://cmake.org/download/)
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/). We'll use Visual Studio 2019 Community. Make sure you have the C++ toolchain installed.
- [Git](https://git-scm.com/downloads)
- [vcpkg](https://github.com/Microsoft/vcpkg) to install dependencies.

For CMake, Visual Studio and git use the official installers.

### Install vcpkg

Open a command prompt as Administrator. Clone the git repo and install. For this example we will just put it in **c:\vcpkg**:

```
> git clone https://github.com/Microsoft/vcpkg.git

> cd vcpkg

> .\bootstrap-vcpkg.bat

```

Now you can search and install packages like this:
```
> .\vcpkg search packagename

> .\vcpkg install packagename
```
Pleas note that vcpkg defaults to 32 bit packages. So if you want the 64 bit version you need to do:
```
> .\vcpkg install packagename:x64-windows
```

### Get OIIO

Download the repository from Github. We will put it in **c:\oiio**

```

> git clone https://github.com/OpenImageIO/oiio.git

```

### Install dependencies

The following depencies are required to build:

- Boost
- OpenEXR
- libTIFF

Open a command prompt in c:/vcpkg and install the packages:

```
> .\vcpkg install boost:x64-windows

> .\vcpkg install openexr:x64-windows

> .\vcpkg install tiff:x64-windows
```
This is the bare minimum required to build. You can add your own optional dependencies using the command above.

If you want Python or Qt, use the official installers for those.

The packages have been installed into **c:\vcpkg\installed\x64-windows**. The easiest thing now is to add that directory to your PATH:

```
> setx path "%path%;c:\vcpkg\installed\x64-windows"
```
You have to close and reopen the command prompt for the new PATH value to take effect. Check that the directory has been added with:

```
> echo %path%
```

### Configure CMake

Run CMake GUI as Administrator and point it to **c:\oiio**.

Create a new folder called **build** in **c:\oiio** and set it as output directory in CMake at the top where it says: *Where to build the binaries*.

When you hit **Configure** you can specify a generator. We will use **Visual Studio 16 2019**.

Now we can set the variables that we need. Since we are doing a minimal build of only the C++ library we will use the following setting:

```
USE_PYTHON = false
```
Note: If you don't have Python installed you need to set **USE_PYTHON = false**, otherwise the configuration will fail.

Hit **Configure** again and you will see *Configuring done* at the bottom of the window. CMake will give you wanrings for the optional dependencies it cannot find. Add those if you need them.

Next click on **Generate** and then on **Open Project** to open the Visual Studio Solution that CMake has generated.

### Build

In Visual Studio change the solution configuration at the top from **Debug** to **Release**. Right-click the solution and select **Build**.

After the build has completed you can find your files under **c:\oiio\build** in the directories **bin**, **include** and **lib**.


