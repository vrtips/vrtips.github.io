+++
author = "Sesilaso"
title = "Weighting (Skinning) limits for meshes in VRChat"
date = 2022-10-20T16:00:00+02:00
description = "The problem you often don't notice until you test in-game and how to solve it"
tags = [
  "avatar",
  "weighting",
  "skinning",
  "assets"
]
+++

# Weighting (Skinning) limits for meshes in VRChat
Have you ever wondered why the new piece of clothing you just created in Blender, you worked on it for 2 weeks
and have tested the most weird positions before even exporting it, then you tested in Unity and it was still good
but when you upload it to VRChat... it's broken! It clips!!!! WHY!?  
You rush towards the nearest blanket, grab your Blahaj and question your life decisions...  
But what exactly causes this behaviour? Can you do something about it?

Well, today we're going to explore what's happening behind the scenes and why this annoying behaviour exists :D

## Whose fault is it
Is it Blender exporting weird? But in Unity the mesh was still working perfectly fine so it can't be!  
Is it VRChat's dev team being too lazy? Well, usually yes but no, not in this case! In fact, the answer to what's
going on comes right from [their documentation](https://docs.vrchat.com/docs/vrchat-configuration-window#graphics-quality),
even if it a indirect form...  

The reason for this is that the game is compiled with a pre-set limit for "Skin Weights" of "4 Bones"
(2 for VR Low and Quest versions of the game).  
This is done mostly because otherwise Unity would have a limit of 255 weights PER VERTEX, and for the performance
savvy people out there reading this, it would mean that FOR EACH vertex the game would have to check and
do the transformation operation 255 times!!! This is a sure way to create a crasher, either on purpose or not!  
Imagine a very low poly mesh with 20k polygons, each polygon (unity counts tris) has 3 vertices,
20 * 3 * 255 operations = 15300000 against the current 20 * 3 * 4 = 240000 !!! That is a MASSIVE difference! (And it might be worse)

## One suggested way to test this in Blender
Ok so here's my personal solution to this, there might be better ways and you can also set these limits in Unity while
you're testing to also check if all is good before uploading. Of course, there might be better ways to do this, in that case
please feel free to tell me! I'd be glad to know better or more efficient ways to weight paint things and maybe add them to this
post!  
But without losing more time, here's the simple solution:  

Unity strips all the extra weights except the highest 4, or by [their own words](https://docs.unity3d.com/ScriptReference/ModelImporter-maxBonesPerVertex.html)
"vertices with more than this number will have the lowest weighted bones discarded."

It also just so happen that Blender has the exact same function: "Limit Total"! (Yes, 4 is exactly the default, this is because this is a common value
in game engines c:)
![Limit Total option in Blender](https://i.imgur.com/tl9ePvy.png)

**Warning**: After you use Limit total, the weights on your mesh will NOT be normalized! You can solve this by using the "Normalize All" button in the same menu c:
Unity's would be normalizing them as well as they specifically state that [The sum of all weight values must be 1](https://docs.unity3d.com/ScriptReference/BoneWeight.html)

And yeah, that's it! You can test the mesh now in Blender and it should behave the same in-game AND in Unity... With a little exception...  
Most avatar bases actually don't do this :P Therefore when you test this in Blender you might notice that it clips now but not in VRChat!  
This is confusing... except that you can also do the same process to the base file as well! You don't really lose anything as this is what
VRChat would be doing anyways! (or you can only use that mesh to test and not to import into Unity, your choice...)

## References
[1] https://docs.unity3d.com/ScriptReference/ModelImporter-maxBonesPerVertex.html  
[2] https://docs.vrchat.com/docs/vrchat-configuration-window#graphics-quality  
[3] https://docs.unity3d.com/ScriptReference/QualitySettings-skinWeights.html  

## Curiosities
- Weight values are stored with a precision of [16-bit normalized integers](https://docs.unity3d.com/ScriptReference/Mesh.SetBoneWeights.html).
This is probably cause the total size of 4 weights is then 64 bits and the value can be fetched from memory all together to optimize cache misses c:
