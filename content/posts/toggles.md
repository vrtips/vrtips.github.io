+++
author = "Sesilaso"
title = "Avatar Toggles"
date = 2021-08-10T14:31:33+02:00
description = "SDK3 Avatar Toggles"
tags = [
    "avatar",
    "sdk3",
    "toggles",
]
+++

# Avatar Toggles
In this post I want to showcase 2 ways to make avatar toggles for SDK3 avatars.

## Methods
There are 2 ways to make toggles, one is the most common ones if you look at tutorials online and follows the VRChat request/standard to not use "Write Defaults" in animations. The other is an alternative way that goes a bit against the standard put in by VRChat but is, in my opinion and only for simple toggles, a little bit faster and less prone to errors.

## Requirements
- Knowing how to setup your avatar for SDK3 or an already setup project with an existing FX Layer and Menu system setup
- Around 10 minutes

## What is "Write Defaults"
Let's first understand what the option of `Write Defaults` is...

Write defaults does this:
- Animator enters new state
- State has Write Defaults
- Game then sets all the defaults values to any parameter changed in the animation. This means that if by default something is off then at the beginning of the state this will be off again, doesnt matter what happened in the previous state
- Plays the state animation

So for example let's take this animation layer:

![Deagle Aniamtion](https://i.imgur.com/uGXrYAA.png)

All the states with the white X on a red background have the Write Defaults OFF, the rest have it ON (only the Idle state).  
What does this do and why?  
The reason for my choice of assignments is that I want the animations to occur one after the other keeping the changes from the previous state.  
The first state moves the weapon's slide backwards and the second holds it there until I release the slide. The last state before the exit makes it slide back.  
If I had Write defaults enabled on every state I should have manually added the changes of the previous one.  
For example in the hold state I would have needed to tell it to stay back.  
With them __disabled__ I can just set the slide to go back on the first and play the sound, have it transition to the next state at the end of the animation and only have to turn the sound off and wait for the transition condition to happen so to move to the last state which will enable the sound and move the slide to the original position, then exit and go to the default state that has the Write Defaults **ON** which means everything will return to how it was setup originally.

At the end of the day, as the [VRChat documentation says](https://docs.vrchat.com/docs/avatars-30#write-defaults-on-states) it's all a matter of convention. You can choose which ever you prefer or mix match them (unless you're working in a team, then please only stick to one). In my case I was lazy and didn't want to make an extra node to disable the last sound I had on the last state before the exit so I just enabled Write defaults on the Idle state but you can achieve the same result in all ways.

## Common Steps
Create a new animator layer in your `FX Animator`, name it as you like possibly following some sort of convention, in my case it's usually `ToggleObject`  
**REMEMBER** to set the weight of the animator layer to 1

![new layer](https://i.imgur.com/s7xLZ7h.png)

Then we prepare the Animator parameter by adding it to both the Animator and the Avatar Parameters (Use `bool` as type for on/off toggles)

![paramter](https://i.imgur.com/f7b6xV7.png)

![menu paramter](https://i.imgur.com/84slqEx.png)

Add a toggle to your Quick menu

![quick Menu toggle](https://i.imgur.com/FVU3OCm.png)

## First Method - off/on state
Since we are not using "Write Defaults" we will have to setup 2 different animations: On and Off.  
For this post I will always setup my object as being be default off, so not visible, but if you want the opposite effect just invert everything I say.  

### Step 1: Animations
Select your avatar (or a duplicate of it, for safety reasons) and assign to it your FX layer:

![avatar select](https://i.imgur.com/FSyB7OZ.png)

![FX layer assignment](https://i.imgur.com/YmHjQSz.png)

Then open the animator and animation windows

![windows to open](https://i.imgur.com/mHYmQXs.png)

And with your avatar (the one with the FX Layer set in the animator) still selected create 2 new clips:
- Animation for the object being ON
- Animation for the object being OFF

![new clip](https://i.imgur.com/pU4nbCf.png)

![new clip](https://i.imgur.com/xsa7TJJ.png)

Name them however you want, possibly referencing if it's the off or on version:

![naming](https://i.imgur.com/uAKzp2f.png)

In case it isn't obvious, to switch from one animation to the other, once created, you click here:

![animation switch](https://i.imgur.com/j34ftWs.png)

For the ON version, start by __disabling__ the object by clicking on it in the scene and disabling it from the inspector:

![disabling](https://i.imgur.com/zwmX5zh.png)

Then in the animation window click on the record button:

![record](https://i.imgur.com/DhufiEH.png)

And now Re-enable your object from the inspector. You should end up with something that looks like this:

![enabled](https://i.imgur.com/VilDMva.png)

**REMEMBER** to stop the keyframe recording by clicking on the record button again!!!

Now do the same for the OFF animation but starting with the object toggled ON from the inspector without being recorded, then record and disable the object.  

We now have created the animation files, but they are automatically added to the default Layer on the animator. You can either drag and drop them from the Explorer window to the correct layer in the Animator window or simply `CTRL + C` `CTRL + V` the nodes from the base to the one you created earlier.

![angery](https://i.imgur.com/UAi1iIM.png)

![kalm](https://i.imgur.com/ZLUZagi.png)

### Step 2: Nodes setup
We will now setup our nodes and transitions.  
I'm still assuming that my default state, that is when I load the avatar for the first time and haven't used any toggle, is with the item OFF. If you need the opposite simply invert the nodes.

Set both the nodes (OFF and ON) with Write Defaults off:

![Write Defaults](https://i.imgur.com/46iAWNb.png)

Assign the OFF animation node as default state:

![Set default](https://i.imgur.com/KHqmxBk.gif)

Set the transitions between states:

![transitions](https://i.imgur.com/Xm1qa2m.gif)

Click on the one that goes from OFF to ON and set it as in the picture

![Off to on](https://i.imgur.com/lGNH1eE.png)

![set](https://i.imgur.com/xbEGVNj.png)

Do the same for the other transition (ON to OFF)

![set](https://i.imgur.com/h5IMi8S.png)

And you're done!

## Second Method - Write Defaults ON
In this case we only need 1 animation, the one that happens when the toggle is pressed.  
In this post's case, that's the ON animation but as usual, invert it if you need the opposite behavior.  
This method also has the advantage of not showing up in the avatar selection since the object will be disabled by default not from the animation but from the engine itself!

### Step 1: Animations
Select your avatar (or a duplicate of it, for safety reasons) and assign to it your FX layer:

![avatar select](https://i.imgur.com/FSyB7OZ.png)

![FX layer assignment](https://i.imgur.com/YmHjQSz.png)

Then open the animator and animation windows

![windows to open](https://i.imgur.com/mHYmQXs.png)

And with your avatar (the one with the FX Layer set in the animator) still selected create a new clip:

![new clip](https://i.imgur.com/pU4nbCf.png)

![new clip](https://i.imgur.com/xsa7TJJ.png)

Name it however you want, possibly referencing if it's the OFF or ON version:

![naming](https://i.imgur.com/uAKzp2f.png)

In case it isn't obvious, to switch from one animation to the other, once created, you click here:

![animation switch](https://i.imgur.com/j34ftWs.png)

__Disable__ the object by clicking on it in the scene and disabling it from the inspector:

![disabling](https://i.imgur.com/zwmX5zh.png)

Then in the animation window click on the record button:

![record](https://i.imgur.com/DhufiEH.png)

And now Re-enable your object from the inspector. You should end up with something that looks like this:

![enabled](https://i.imgur.com/VilDMva.png)

**REMEMBER** to stop the keyframe recording by clicking on the record button again!!!

We now have created the animation file, but it's automatically added to the default Layer on the animator. You can either drag and drop it from the Explorer window to the correct layer in the Animator window or simply `CTRL + C` `CTRL + V` the node from the base layer to the one you created earlier.  
You should have a layer setup like this:

![kalm](https://i.imgur.com/N4RQYSF.png)

### Step 2: Nodes setup

Create a node WITHOUT any animation assigned to it and set it as default state. Additionally I usually rename this state to `Idle`

![default empty](https://i.imgur.com/bobeSEI.gif)

Then create transitions from the empty state to the ON state and from the ON state to the EXIT node:

![empty transitions](https://i.imgur.com/DmYzF2m.gif)

And set the transitions like this

![set trans](https://i.imgur.com/mYF1uK7.png)

![set trans2](https://i.imgur.com/aSrlYob.png)

Now, before uploading your avatar, make sure that the object is in the default state you want it on, in my case it will need to be OFF (disabled). This way once I go to the menu and enable the toggle, the animation will turn it on and once I disable the toggle it will revert to the default state (which was disabled)

## Conclusions
I hope this guide helps you set up toggles correctly and maybe choose which way to set them up. For toggles that don't require anything more than enabling and disabling an object I always use the second method since its faster to setup and leads to less errors by my side so that's my general suggestion. The final word is still yours on your way and neither is wrong or right.  
