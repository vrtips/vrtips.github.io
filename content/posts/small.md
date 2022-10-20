+++
author = "Sesilaso"
title = "Avatars under the minimum size limit"
date = 2021-08-11T01:39:35-07:00
description = "Bypassing the minimum size limit of the VRC SDK"
tags = [
    "avatar",
    "sdk3",
    "small",
    "bypass",
]
+++

# Uploading avatars under the minimum size limit
There are a couple of reasons why you might want to upload an avatar under the minimum size: maybe it's a false positive, maybe you just want that extra less centimeters or maybe you're just curious.  
In any case, this post will guide you on how to remove the limit and how to fix yourself in case your avatar won't allow you to switch back to your normal avatar (or open the quick menu completely).

## Requirements
- Unity project setup correctly
- Visual Studio or any text editor
- 10 minutes of your time
- [VRCX](https://github.com/pypy-vrc/VRCX/releases) (for going back to your avatar if the scale is broken)

## Disclaimer
The protections that are going to be removed in this post **are there for a reason!!!** Uploading a too small avatar WILL result in issues like for example the menu not showing up and therefore not being able to switch back to a normal avatar. While I do give you a way to solve this, I am not responsible if this causes more issues than that and everything you do is at your own risk.  
In addition I want to clarify that this was posted as an educational guide or in case someone has a legit reason to need this, I **DO NOT** promote any malicious use of this and my goal with publishing this is just to share some findings and let people have some fun.

## What's the minimum size for an avatar?
If you've ever wondered how small you can make yourself and changed your avatar scale you might have noticed that VRChat starts giving you this error:

![size error](https://i.imgur.com/TSHronT.png)

Well, it turns out that the check for that is only on the client and specifically only in the SDK and therefore can be bypassed. I am 100% certain that the VRChat team is aware of this and this is not an issue nor a vulnerability so it does not need to be fixed. After all, if the user wants to break their own game then they are free to

## Changing the size limit
To remove or change the size limit you'll just need to open the following file `\Assets\VRCSDK\Dependencies\VRChat\Editor\ControlPanel\VRCSdkControlPanelAvatarBuilder.cs` with your favorite text editor (or IDE).  
Now search for this line (at the moment of writing it's at line 299)

![line 299](https://i.imgur.com/9bWng9H.png)

Those of you who can read some code or if you're quick to catch on things you can already recognize what this line does. So let's change it to this

![edited line](https://i.imgur.com/Jp8eRta.png)

Now, no matter the size, that error can never be reached. (yes, you could also just delete the check)  
If we go back to our SDK control panel we can confirm that indeed the size limit is removed and we are now allowed to upload any size of avatar.

## I have made my avatar too small and can't open the menu anymore
Don't worry my dear reader, you are not stuck forever as a subatomic sized player.

### First Method
Go to [the VRChat website](https://vrchat.com/home/avatars) and click on `Reset to default Avatar`

![reset avatar button](https://i.imgur.com/6OCjvCq.png)

### Second Method (using VRCX)
To get back your avatar you can also use [VRCX](https://github.com/pypy-vrc/VRCX/releases), it's a open source program that uses the VRC API to basically do what you can do in-game if not more.  
Download the latest version, extract it and run `VRCX.exe`. It will ask you for your VRChat credentials to login.

Once you're logged in, you will see on the right the list of all your friends with, at the top, your own profile. Click it and follow the images and/or instructions below:  
- Click on your Profile  
- Click on avatars  
- Click on the update button if they are not showing up  
- Click on a working avatar  
- Click on the 3 dots on the top right  
- Click on Select avatar

![click on your Profile](https://i.imgur.com/UY6Mqlv.png)

![Click on avatars](https://i.imgur.com/5iaKWtw.png)

![Click on the update button if they are not showing up](https://i.imgur.com/knOgpvH.png)

![Select avatar](https://i.imgur.com/1hjMBjP.png)

You will now need to restart your game and you'll be on your normal avatar.

## Conclusions
While this is not a vulnerability or anything concerning VRChat, this is still something that the developers did not intended you to do. They are just trying to save the common user from telling them to use an external tool to fix their mistake.  
With this said, the 20cm limit is not exactly when the menu becomes unusable and having a slightly smaller avatar can be, some times, fun.

If you like my content and want to support me, I have a Patreon you can find in the [About](https://sesivr.netlify.app/about/) page.
