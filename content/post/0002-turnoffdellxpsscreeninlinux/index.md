---
title: "Turn off Dell XPS screen in Linux"
type: post
date: 2020-02-19
categories: Linux
tags:
  - linux
  - dell
  - xps
  - 9500
  - screen
  - visudo
image: bg.png
slug: turn-off-dell-xps-screen-in-linux
---
I recently bought a new laptop, a Dell XPS 9500. I'm happy with it but there are some minor details that I don't like. One of them should be easy to fix with an update, but I don't think that's happening. A shortcut to turn off the screen backlight. This is something that I've seen in any other computer, either with a dedicated Function key (those in the F row that you need to use in combination with the Fn key) or as the lowest level of brightness. The second option was my first approach, trying to modify the lowest level of brightness and set it to 0, but I wasn't able to find out how. So I changed the approach and looked for a way to set the screen backlight to 0 and bind it to a shortcut. This solution if for Linux (tested only on Ubuntu 20.04).  
[Link to solution](#solution-summarized)

#### Turning off the screen backlight via command
I found several ways to turn the screen backlight off, but only one worked for me. One option was to use the command `xbacklight`. This command is really easy to use, just need to pass the brightness percentage: `xbacklight -set 50` or `xbacklight = 50` sets the screen backlight to 50%. I found a way to make it work modifying some Xorg files, but that left the system unstable (some programs working really slow and glitching). Another suggestion that seemed to work was using the comand `xrandr`, but this command only change the image being displayed on the screen, but not the backlight. Setting the brightness to 0 with `xrandr` will just display a black screen, but the backlight may be on.

The solution that worked for me was manually setting the brightness value in the file `/sys/class/backlight/intel_backlight/brightness`. The problem with this solution is that you need some script to set the brightness level and you also need to run this command as root.

#### Script to turn the screen off
With this solution working I created a script to turn it on and off, saving the previous brightness value:
```bash
#!/bin/bash
# 
# Toggles on and off the screen backlight
# 
# It saves the current brightness value, if there is no previous value, 
# defaults to 1/10th of max brightness
# 

brightness_path=/sys/class/backlight/intel_backlight/brightness
current_brightness=$(cat $brightness_path)
max_brightness_path=/sys/class/backlight/intel_backlight/max_brightness
max_brightness=$(cat $max_brightness_path)
prev_on_value_path=$(dirname $(realpath $0))/prev_brightness
log_path=$(dirname $(realpath $0))/log.txt
debug=false

log() {
    if [ "$debug" = true ]; then
        echo $@ >> $log_path
    fi
}

set_brightness() {
    log "Setting brightness to $1"
    echo $1 | tee $brightness_path
}

set_previous_brightness() {
    echo $1 | tee $prev_on_value_path
} 
    
if [ "$current_brightness" -eq "0" ]; then
    if [ ! -f "$prev_on_value_path" ]; then
        set_previous_brightness $(( max_brightness / 10 ))
    fi
    set_brightness $(cat $prev_on_value_path)
else
    set_previous_brightness $current_brightness
    set_brightness 0
fi
```
[Link to script](https://gist.github.com/diego09310/b907cda1d60534b69aee64c07a7cfcac)

#### Superuser bypass and keyboard shortcut
Once I had the script working, next step is to allow my user to execute this script as superuser without password. We need this to make the keyboard shortcut work (if not, it will fail waiting for the password or will request it). To do this we need to modify the sudoers file. The sudoers file contains configurations regarding the use of the `sudo` command. This file is edited using the command `visudo`, which will open a temporal file in your default editor and then will apply the changes. We need to add the following line:

`diego ALL = NOPASSWD: /<path-to-script>/toggleBacklight.sh`

This line allows the user `diego`, can run, in all hosts, the file `toggleBacklight.sh` without password. Once this new rule is applied, we can run the script, using `sudo` without password.

We also need another step regarding security. Right now, the script `toggleBacklight.sh` can be executed as a root user without the password. This is a security problem as if an attacker gains access to this user account, it can modify the file and execute anything with superuser rights. To avoid this we need to change the ownership of the file to the root user and remove writing access to others (this may come by default, but I included the command just in case). We do this with these two commands:

```
sudo chown root:root /<path-to-script>/toggleBacklight.sh
sudo chmod o-w /<path-to-script>/toggleBacklight.sh
```

Once this is ready and working, we can create the shortcut. This is probably different depending on the desktop and maybe on the linux distribution, but in Ubuntu is just going to the `Keyboard Shortcuts` menu inside te settings and adding the following custom shortcut:

```
Name: Toggle backlight
Command: sudo /<path-to-script>/toggleBacklight.sh
Shortcut: Ctrl + Super + F6
```

#### Solution summarized
Create installation folder:  
`mkdir toggleBacklight`  
`cd toggleBacklight`

Download script:  
`wget https://gist.githubusercontent.com/diego09310/b907cda1d60534b69aee64c07a7cfcac/raw/133a3079bc39eabdd90d0365f2ac400f5a249d29/toggleBacklight.sh`

Edit sudoers file:  
`sudo visudo`

And add this line:  
`<username> ALL = NOPASSWD: /<path-to-script>/toggleBacklight.sh`

Change ownership to root:  
`sudo chown root:root /<path-to-script>/toggleBacklight.sh`  
`sudo chmod o-w /<path-to-script>/toggleBacklight.sh`

Create keyboard shortcut (depends on desktop/distro).
