## scrcpy - Mirror Your Device's Screen! (Linux Edition)

## scrcpy???
### What is it and why can't we pronounce it's name?

scrcpy is a neat little software that allows us to mirror *and* control our android device's screen on our PC. It can do a whole lot more than that but let's save that for another post. It is developed and maintained by [Genymobile](https://www.genymobile.com/), creators of Genymotion; the android emulation service.

As to why we probably can't pronounce it, that's by design. Diving down in the [official repo](https://github.com/Genymobile/scrcpy) we'll find the answer provided by the creators;

> A colleague challenged me to find a name as unpronounceable as gnirehtet. `strcpy` copies a string; `scrcpy` copies a screen.


### Alright, how do we install it?
There are a couple of ways to install scrcpy depending on what operating system we're using and how much time we've got on your hands. 

In this post, we're going to install it on Debian Linux using the terminal-based package manager. In case this method doesn't work for you, the aforementioned official repo has an awesome guide for all other systems and installation methods.
 
 *Let's get started!*

#### Prerequisites
We'll need a couple of things to get scrcpy running once we install it.
- An android device with [developer mode enabled](https://www.digitaltrends.com/mobile/how-to-get-developer-options-on-android/)
- A good quality USB cable
- [adb](https://linuxtechlab.com/install-adb-fastboot-ubuntu/)

#### Install scrcpy 
Once we have all the prerequisites we can finally install scrcpy by running the following command in the terminal;
``` 
sudo apt install scrcpy 
```
We'll provide our password and accept any queries if requested.

**That's it!** If everything went well we've successfully installed scrcpy. 
With our device connected we can run the following command and in a few seconds, or less, the screen should popup on our PC.
```
scrcpy
```
This is what success looks like...
![terminal & device screen](https://cdn.hashnode.com/res/hashnode/image/upload/v1596005528020/Te7pWY04a.png)

### But wait, there's more...
Let's make running scrcpy even better by adding a few more options to the main command.

#### Specify device
There are times when we may have more than one device connected to our PC. In this case we have to let scrcpy know which device we wish to mirror. 

To specify the device we run the following command; 
```
scrcpy --serial THE_DEVICE_SERIAL_NUMBER
```
To get **THE_DEVICE_SERIAL_NUMBER** we need to run the following command
```
adb devices -l
```
Let's copy the device serial number of the device we wish to mirror (highlighted in the orange rectangle). To know which serial number belongs to which device we refer to the model (underlined in red). 
![list of adb devices](https://cdn.hashnode.com/res/hashnode/image/upload/v1596007819724/g6EZ0KY11.png)

#### Always on top
Keeping the mirrored phone's screen on top of other windows comes in handy when we are referencing things on that screen for work. A perfect example is when we have it open over and to the side of our code editor while debugging and tweaking our app's UI. 
To keep the mirrored phone's screen on top of other windows we run the following command; 

```
scrcpy --always-on-top
``` 

#### Custom window position and size
When we run scrcpy we notice that the mirrored screen usually opens up in the center of the screen with a default size. We can customize this behavior so that the screen opens up where we usually end up placing it every time and at the right size.

First, let's get the window position and size. Start by positioning and resizing the mirrored screen right where we like it to be. Then we'll get the numbers for its size and position by running the following command;
```
wmctrl -lG
```
This will give us a list of all our windows with a lot of info. A detailed explanation of what all of the fields represent can be found [here](https://askubuntu.com/questions/27894/get-window-size-in-shell). We are only interested in columns highlighted below.

![we are interested in columns 3,4,5,6 and 8](https://cdn.hashnode.com/res/hashnode/image/upload/v1596016437389/EmZXwD5ky.png)

The last column helps us identify which of the info belongs to the window of our mirrored screen due to the matching names (highlighted in the red rectangles).

Copy the values of **columns 3-6**. They represent the window's **x-offset, y-offset, width and height** respectively.

To open scrcpy with the custom position and size we run the following command;
```
scrcpy --window-x X_OFFSET --window-y Y_OFFSET --window-width WIDTH --window-height HEIGHT
```

### Tying it all together
We've done a lot to make scrcpy run the way we want it to. There's just a *small* issue. We don't expect to type out these commands in the terminal every single time. So how do we solve this issue? 

#### The desktop launcher
We'll create a desktop launcher that has a nice icon, that will be available in our application menu and that will run scrcpy with all our options.

First, let's create an empty file anywhere we can and name it `scrcpy.desktop`.

The we'll open this file with a text editor and paste the following text in it;
```
[Desktop Entry]
Type=Application
Name=scrcpy
Categories=Development
Icon=/usr/share/icons/scrcpy.png
Exec=/usr/bin/scrcpy --serial THE_DEVICE_SERIAL_NUMBER --always-on-top --window-x X_OFFSET --window-y Y_OFFSET --window-width WIDTH --window-height HEIGHT
```

After ensuring that all the variables (in all-caps) are replaced with the right values we can save the file and close the editor. 

We just created a desktop launcher for scrcpy. 

- The **Type** value specifies that it is an application
- The **Name** value is the name of the application
- The **Categories** value places it in the *Development* category in our application menu. See [this post](https://specifications.freedesktop.org/menu-spec/latest/apa.html) for all the registered categories that can be used here. 
- The **Icon** value is used to set the icon for the launcher. Any `.png` or `.svg` file will work. Here we used an image called `scrcpy.png` which we placed in the `/usr/share/icons/` folder to make sure it never gets accidentally deleted.
- The **Exec** value is where we specify our main `scrcpy` command along with all our custom options.

**Finally,** to make the launcher appear in the application menu we need to place it in the `/usr/share/applications/` folder. **Note** that we have to open that folder as root to place anything within it. If we've done everything right we should find a scrcpy launcher in the application menu under the *All Apps* or *Development* category.

**Voila!**
![application menu with scrcpy launcher](https://cdn.hashnode.com/res/hashnode/image/upload/v1596027599286/PjWn6vlpm.png)

### The Wrap-up
In this post we installed scrcpy on Linux, the lightweight yet powerful screen mirroring tool. We also applied various options to the `scrcpy` command to let it run the way we like then created a launcher to run scrcpy without the hassle of typing all the commands in a terminal.

Hopefully, this will provide a small productivity boost to get that side project done :D

#### Enjoy your day
\- ArthurDev



