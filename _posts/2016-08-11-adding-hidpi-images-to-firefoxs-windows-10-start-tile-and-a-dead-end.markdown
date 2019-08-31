---
layout: post
title: Adding hidpi images to Firefox's Windows 10 start tile and a dead end
date: '2016-08-11 13:52:05'
tags:
- mozilla
- code
- windows10
---

### <i markdown="1">Part 3 in [a series](http://blog.katiebroida.com/tag/windows10/) on improving Firefox's Windows 10 start menu tile</i>

![Image of eye](/assets/images/2016/08/All-Seeing-Eye-Rays-300px.png)

[After improving the Windows 10 start menu tile for at least some computers](http://blog.katiebroida.com/formating-firefoxs-visualelementsmanifest-file/), I moved on to adding higher resolution images for Windows 10 computers that have hidpi screens. I hoped that specifying which images should display at certain resolutions would solve two issues:

 - Maybe by telling Windows what images should be displayed in another location besides VisualElementsManifest, the custom start menu tile will appear for <i>all</i> Windows 10 machines 
 - Make the Firefox icon look crisper on Windows computers with HiDPI screens 

If you're not familiar with why Windows 10 needs different images at different scales, it's because some laptops nowadays have screens that have more dots per inch (dpi) than computers have in the past. This allows their displays to have more definition and not let individual pixels be visible.

According to Microsoft's documentation, we can provide different images at various scales if we use their naming scheme. Windows will sort out which one to use if we also give it a Resources.pri file that says which images should be used at each scale (step 6):

[https://msdn.microsoft.com/en-us/library/windows/apps/dn393983.aspx](https://msdn.microsoft.com/en-us/library/windows/apps/dn393983.aspx)

![](/assets/images/2016/08/right-pointing-magnifying-glass.png)

Following these docs, I used makepri.exe to try to generate this file. While I was able to create a Resources.pri file for the 100% and 80% scales I wanted, it also created a separate one for both the 140% scale and the 180% scale. It was very mysterious (why didn't 100% and 80% generate their own files??), and I couldn't find the reason for multiple pri files for the longest time. Eventually I found a workaround of setting a default language ("en") for the image files, which allowed makepri.exe to create just one Resources.pri file. If you want to read about it in more detail, you can my find Stack Overflow question on it below:

[http://stackoverflow.com/questions/38506783/why-is-makepri-exe-creating-more-than-one-resources-pri-file](http://stackoverflow.com/questions/38506783/why-is-makepri-exe-creating-more-than-one-resources-pri-file)

And here is my pri file's contents, which I got using `makepri dump`:

### Resources.pri
```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
	<ResourceMap name="firefox" version="1.0" primary="true">
		<Qualifiers>
			<Language>en</Language>
			<Scale>100,140,180,80</Scale>
		</Qualifiers>
		<ResourceMapSubtree name="Files">
			<NamedResource name="150x150Logo.png" uri="ms-resource://firefox/Files/150x150Logo.png">
				<Candidate qualifiers="Language-en, Scale-80" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\150x150Logo.scale-80.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-180" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\150x150Logo.scale-180.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-140" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\150x150Logo.scale-140.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-100" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\150x150Logo.scale-100.png</Value>
				</Candidate>
			</NamedResource>
			<NamedResource name="70x70Logo.png" uri="ms-resource://firefox/Files/70x70Logo.png">
				<Candidate qualifiers="Language-en, Scale-80" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\70x70Logo.scale-80.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-180" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\70x70Logo.scale-180.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-140" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\70x70Logo.scale-140.png</Value>
				</Candidate>
				<Candidate qualifiers="Language-en, Scale-100" isDefault="true" type="Path">
					<Value>browser\VisualElements\en\70x70Logo.scale-100.png</Value>
				</Candidate>
			</NamedResource>
		</ResourceMapSubtree>
	</ResourceMap>
</PriInfo>
```

I was pretty excited to have found this workaround, but I needed to make sure that setting "English" as the tile images' default language wouldn't break the tile for non-English Windows computers. After changing my computer to French, I realized that not only did my Resources.pri file break the tile images for French computers, it did it for English systems as well. In addition to breaking the tile images, it also didn't solve the issue of why some Win10 computers update their Firefox start tile and some don't. 

![](/assets/images/2016/10/try-again.gif)
*Back to the drawing board*

I have another thought on how to implement a start tile image for HiDPI screens, but I think I need to back up and figure out why the start menu tile isn't being updated consistently on all computers. This way at least more people will be able to see the icon, even if not in the most amazing high resolution possible.

If someone has thoughts on why Resources.pri isn't working in this case, be sure to let me know. Even though it was a dead end, I did enjoy sort of reverse engineering what makepri.exe wanted from me to generate one pri file. I found [an interesting book recently on reverse engineering for beginners.](http://beginners.re/) I think I'll dig into that topic more at some point. 
