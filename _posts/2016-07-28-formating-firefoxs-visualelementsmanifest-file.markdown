---
layout: post
title: Formating Firefox's VisualElementsManifest file
date: '2016-07-28 22:20:16'
tags:
- code
- mozilla
- windows10
---

### <i markdown="1">Part 2 in [a series](http://blog.katiebroida.com/tag/windows10/) on improving Firefox's Windows 10 start menu tile</i>

![Windows 10 logo](/assets/images/2016/07/win10.jpeg)
*Into the start menu rabbit hole ...*

Once I understood how to get all of my files in the right place ([see part 1](http://blog.katiebroida.com/copying-files-to-firefoxs-root-install-folder/)), I next had to figure out what should go in those files. As I mentioned in my first post, the `VisualElementsManifest.xml` file tells Windows which custom image and style settings to use for the start menu tile. The documentation made it seem straightforward:

[https://msdn.microsoft.com/en-us/library/windows/apps/dn393983.aspx](https://msdn.microsoft.com/en-us/library/windows/apps/dn393983.aspx)

Though I followed the doc's formatting, I didn't always find that my VisualElementsManifest file would affect my start menu tile in a way I expected or even sometimes at all. I found I really had to get into a rhythm of making a change and then triggering a force update of the start menu tile to see the affects. Sometimes I found I was forgetting the force-tile-refresh step, and would draw the wrong conclusion from my changes based on the outdated tile. 

To force your start tile to update, you can use the code snippet below in PowerShell. I found it helped to run it as an administrator, so you may want to as well. 

```
(ls "..\..\app.lnk").lastwritetime = get-date
```

To update the correct tile, make sure you replace the `app.lnk` name with your tile's correct shortcut name. In my case it was `Mozilla Developers Preview.lnk`. For my computer's set up, I found there was two possible locations for where my start menu .lnk files could be, one that applies to all users and one location that was specific to my currently logged in user. I've listed them below in case you have trouble finding them. 

```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs
C:\Users\<User>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs
```

For more info on where start menu files live, you can check out my source on this:

[http://superuser.com/questions/960406/where-are-all-locations-of-start-menu-folders-in-windows-10](http://superuser.com/questions/960406/where-are-all-locations-of-start-menu-folders-in-windows-10)

![Mr Rabbit](/assets/images/2016/07/Down_the_Rabbit_Hole.png)
*Time to dig a little deeper*

Even after establishing a consistent workflow of making a change and refreshing the icon, I still didn't get tile changes that were consistent with Microsoft's documentation. For newer computers, the customized icon worked perfectly. For older Windows 10 computers, there was no change in the tile's appearance at all. 

To figure this out, I looked for some examples of where this was actually working ([à la a recent post on troubleshooting I did](http://blog.katiebroida.com/anatomy-of-a-bug/#doesanyofitworkcorrectly)), and found that Google's Chrome had a nice looking custom icon and I could access their VisualElementsManifest file in Chrome's folder in `Program Files`. I noticed that they had two VisualElementsManifest files, one called `chrome.VisualElementsManifest.xml` and one just called `VisualElementsManifest.xml`. I've listed their contents below:

### VisualElementsManifest.xml

```
<Application>
  <VisualElements
      DisplayName='Google Chrome'
      Logo='43.0.2357.124\VisualElements\Logo.png'
      SmallLogo='43.0.2357.124\VisualElements\SmallLogo.png'
      ForegroundText='light'
      BackgroundColor='#323232'>
    <DefaultTile ShowName='allLogos'/>
    <SplashScreen Image='43.0.2357.124\VisualElements\splash-620x300.png'/>
  </VisualElements>
</Application>
```

### chrome.VisualElementsManifest.xml

```
<Application xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <VisualElements
      ShowNameOnSquare150x150Logo='on'
      Square150x150Logo='51.0.2704.84\VisualElements\Logo.png'
      Square70x70Logo='51.0.2704.84\VisualElements\SmallLogo.png'
      ForegroundText='light'
      BackgroundColor='#323232'/>
</Application>
```

Aside from the name, these two files differ in their formatting. The "chrome" one has the Windows 10 formatting and the more general file has formatting for Windows 8. Having the executable's name at the beginning of the XML file makes the VisualElementsManifest file apply more specifically to the executable with the same name. Otherwise, the plain ole `VisualElementsManifest.xml` file just applies to everything within its same folder. I figured Chrome had used these overlapping files to provide coverage for Windows 8 computers in addition to Windows 10, and possibly address why some older Windows 10 computers don't display our custom start menu tile settings. 

My theory for why Chrome had overlapping settings files sounds pretty good, right? And mimicking their set up didn't break my start menu tile when I created an installer and installed my version with these changes. [However when it went into production for Nightly (Firefox's release channel for testing new changes), it ended up causing a regression](https://bugzilla.mozilla.org/show_bug.cgi?id=1286511). For some Windows 10 computers the correct custom icon rendered in the start menu. For others, tile only rendered with a light blue background, a small icon image and no text for the name of the app to let users know what it was. 


![Broken tile](/assets/images/2016/07/broken-win10-tile.jpeg)
*Our broken friend is in the top right corner*


The solution I found here was to just delete the `VisualElementsManifest.xml` file. This way computers that were detecting and applying `firefox.VisualElementsManifest.xml` would have a new pretty tile, and those that weren't would just have the default (meh) unbroken tile. I was glad to have a solution, but the whole thing struck me as strange since according to Microsoft's documentation, [Windows 10 should have ignored the extra file if it wasn't formatted properly](https://msdn.microsoft.com/en-us/library/windows/apps/dn393983.aspx#code-snippet-12). After finding this solution, I went back and updgraded my copy of Chrome to see what their current configuration is. I found that they just have one file now in the Windows 10 format. 

Long story short, this is what we ended up using for Firefox's VisualElementsManifest file:

### firefox.VisualElementsManifest.xml

```
<Application xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <VisualElements
      ShowNameOnSquare150x150Logo='on'
      Square150x150Logo='browser\VisualElements\VisualElements_150.png'
      Square70x70Logo='browser\VisualElements\VisualElements_70.png'
      ForegroundText='light'
      BackgroundColor='#14171a'/>
</Application>
```


These settings still don't apply to some older Windows 10 computers I've been testing on, though the tile looks good on some newer ones. I have a hunch that what the old computers have in common is that they were upgraded to Windows 10 from Windows 8, whereas the new computers came with Windows 10 installed. My next step is to add a `Resources.pri` file and images at different scales for hidpi screen support. I'm hoping this `Resources.pri` file will fix the tile for older computers as well by making it more explicit to Windows which images we want to use. 

All in all, pretty mysterious. If anyone has any insight into Windows start menu tiles, please leave me a comment. Or several. Just leave as many comments as you need :)
