---
layout: post
title: "Registry: from the dark past"
date: 2014-02-02 22:10
comments: true
categories: [windows, ms, c++]
---

That is my thoughts circa 2006-2008. Time has proven that **simple** and **robust** Unix way won.
Today almost nobody uses Windows Registry for keeping internal data. 
How come that good intention led to bad results?

{% img http://imagewz.winzip.com/static/images/ss-reg-opt-2.png %}

#Why Registry failed?
Main reasons are:

* Lack of robustness.
* Registry is difficult to use.
* Registry is not a something you can't live without.

# Long answer 

1) Registry has 40+ functions (3-7 parameters) to deal with. You have to memorize some of them + understand how they work. Hey, have you ever used *RegUnLoadKey*, *RegQueryMultipleValues* or *RegConnectRegistry* functions?

2) Registry has Keys and Values (with some awkward data types built in).
- In unix: key is a path ("folder") and value is the data inside. 

3) Registry has some weird predefined "folders" like HKLM/HKCU. 

4) Registry has Links for that mounts. For example:  HKEY_CLASSES_ROOT is linked to HKEY_LOCAL_MACHINE\Software\Classes.

5) Registry uses some obscure mechanisms: Redirection + Reflection + Virtualization ))) Oh crap!
How it works:

* Redirection: write from 32-bit app, and data will be redirected to WOW6432Node. This helped to separate 32/64 bit apps automatically.
* Reflection: write to 64-bit key and data will be propagated to WOW6432Node automatically. This helps to register OLE/COM classes only once, for example. Then Microsoft turned that off.
* Virtualization: if old app writes to 'bad' location (that is now read-only) -> it will be redirected to good one.

6) We have no special software to work with Registry. Try to change permissions on multiple keys for example...You can't get statistics, perform optimizations (defragmentation?), move, unmount, repair, clean, bulk: rename, etc. 

7) Registry is a 'single point of failure'. 
> Critics labeled the Registry in Windows 95 a single point of failure, because re-installation of the operating system was required if the Registry became corrupted.However, Windows NT and later versions use transaction logs to protect against corruption during updates. Current versions of Windows use two levels of log files to ensure integrity even in the case of power failure or similar catastrophic events during database updates

8) Registry is monolithic. You have to move (or backup) data from Registry "all at once". Even though Registry can contain many hives, there is still no good granularity.

# Conclusion
Registry rot started with 'Registry-free COM' configurations and manifest files. Even Microsoft was about to stop its registry usage.
And then dozens of Cross-Platform applications that never used Registry to store settings (colors, preferences, etc) emerged. They all use filesystem because libraries are trying to make behaviour as equal as possible on different platforms. 

The good thing is that you are not forced to use Registry. So don't do that.

