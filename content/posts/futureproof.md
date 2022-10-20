+++
author = "Sesilaso"
title = "Future Proof Publish SDK Option"
date = 2021-08-11T08:55:37-07:00
description = "An explanation of the 'Future Proof Publish' option in the SDK, what it does and is it giving away your avatar files?"
tags = [
    "avatar",
    "sdk3",
    "analysis",
    "security",
    "content",
]
+++

# What the Future proof doin?
In this post we will examine more in depth what the `Future proof Publish` option does in the VRChat SDK control panel. This post is a bit technical but I will try to put a simple explanation as conclusion for those of you who only wanted to know what and why.

## Where is it?
If you open your VRChat SDK control panel in unity and go to the Settings tab you will see a "Future Proof Publish" option which at the moment of writing is by default off.  
How does VRChat future proof your publish? Let's find out

![checkbox](https://i.imgur.com/Q2NXaGm.png)

## The code
Opening `\Assets\VRCSDK\SDK3A\Editor\VRCSdkControlPanelAvatarBuilder3A.cs` inside of the method `OnGUIAvatar(VRC_AvatarDescriptor)` we can spot in the uploading process this line:

![sussy line](https://i.imgur.com/5KMm89h.png)

The method called on the line under that is from a compiled binary: `\Assets\VRCSDK\Plugins\VRCSDKBase-Editor.dll` which we can start decompiling using dnSpy but the rest of the code is assigned at runtime and I cannot be bothered checking it too much.

![ExportAndUploadAvatarBlueprint](https://i.imgur.com/m42A2Nv.png)

So to confirm we try it and check what the API gives us back

## Testing the API
I have uploaded the same avatar with and without the checkmark on the Future Proof Publish feature and here's the API calls to check the avatars  

With Future proofing OFF  
![Future proof off](https://i.imgur.com/yNEouMw.png)

With Future proofing ON  
![Future proof on](https://i.imgur.com/RlZ29fL.png)

We can notice that when it's on we can indeed download the `.unitypackage` of our avatar!

![download](https://i.imgur.com/qdXOCst.png)

This means that potentially someone could not only obtain our avatar (usually in `.vrca` format) but they could obtain the exact files we've used to create it!

Or so it would seem...

## Testing but with a friend (Thanks Whanos)
Now, those tests were made by me requesting info on my own avatar. Let's try and see if someone else tries to get that info on my avatar.  
I've asked my friend Whanos to help me on this since I needed someone who could make API requests (I've tested this using Insomnia) and would you look at that!

![whanos results](https://i.imgur.com/o70nkIV.png)

- First discovery: You cannot query an avatar directly from that endpoint if it's private. (which is why you see it set as public there)
- Second discovery: If someone else asks for the info, they will not be given the UnityPackage property!

Hooray! Our avatars are safe then!  
Our avatars are safe then, right?

Well you see, the game still have to fetch the avatar somehow to display it to you in game... so...

## Taking a look inside of the game
I will not go too much in detail on how to achieve this since it could be used maliciously and the last thing I want is to give easy access to how to steal avatars from others. Therefore, compared to the rest of my screenshots, I will limit myself to ONLY the part that interests us.

We are interested to see if the in-game object holding our avatar data is holding our Unity Package link or not. So after some digging around the way that VRChat manages this I breathed a sigh of relief when I saw this

![In Game analysis](https://i.imgur.com/lwXxkI5.png)

Fortunately the game, despite having fields for it, does not give out the unity package url to everyone.

## Does this mean our avatars are safe from being ripped?
No, of course not. As I said, the game does have to download the model to show it to everyone in your instance so somehow all the data about it has to reach someone else's PC.  
Now, I would also like to add that what they get is a "compiled" and "compressed" version of your avatar in the format `.vrca` (which turns out being a `UnityFS` file). It is 100% possible to steal the mesh and the textures from this file in addition to animations and animator controllers. Scripts are safe and so are shaders since those are compiled

# Conclusion
So, your avatars are at least protected from being a simple "import into Unity" kind of deal. The future proof option could as well serve as a backup in case you lose your files for some reason since you can always query the API and get the UnityPackage url.  

This feature exists so that in VRC ever needs an update that requires recompiling your avatar they can do it for you (or the SDK can do it since I would expect them to not want thousands of avatars to be recompiled on their servers) and you're free to keep it on or off as you wish since no harm or file leaks come directly from this feature.  

As per usual in security, this does not mean that this file is safe. The file is there and on their server and if someone, somehow, gets the link then they can download it without even authenticating.  

If you like my content and want to support me, I have a Patreon you can find in the [About](https://sesivr.netlify.app/about/) page.
