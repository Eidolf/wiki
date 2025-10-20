---
title: steeleye
description: 
published: true
date: 2023-12-31T14:34:49.637Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:46.232Z
---

# SteelEye

# <span class="mw-headline" id="bkmrk-hyper-v-mit-steeleye-1">Hyper-V mit SteelEye Standard</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SteelEye&action=edit&section=1 "Abschnitt bearbeiten: Hyper-V mit SteelEye Standard")<span class="mw-editsection-bracket">\]</span></span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-cluster%C3%A4hnlich">Clusterähnlich</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SteelEye&action=edit&section=2 "Abschnitt bearbeiten: Clusterähnlich")<span class="mw-editsection-bracket">\]</span></span>

Eine Virtuelle Maschine mit gleicher Konfiguration über zwei Server Synchronisieren mit SteelEye Standard.

**How to manually add a VM Configuration to Hyper-V**

  
With Hyper-V you cannot simply „Add“ a configuration file to your Hyper-V machine as you could with Virtual Server. Mainly because of the use of Live Snapshots. The recommended way is to use Export and Import.

If you have a given VHD it is way easier to create a new VM and simply add the VHD to it. If you used snapshots, see my earlier blog on how to merge them to a new VHD.

However, recently a colleague had a customer issue, where we only had the flat directory structure and needed to restore that. Here’s how we managed to get this running:

We start from a folder that contains the VM in v:\\manualrecover

.

In this folder you usually have:

1\. The machine VHD

2\. The Virtual Machines Folder with

```
           a: The <GUID>.xml File holding the machine Configuration

           b: The <GUID> Folder 
```

3\. The Snapshots Folder with a &lt;Snapshot GUID&gt;.xml

Hyper-V uses a new Feature in Windows 2008 called Service SIDs. To access files from a VM, these files need to give this Service SID access permissions.

This SID is a combination of the Service SID ""NT VIRTUAL MACHINE" and the VM GUID

Example:

"NT VIRTUAL MACHINE\\2F855D88-F990-47BA-95D6-0029BCD8C059"

Note, The GUIDS we used here are from our VM, You will need to adjust to the GUIDs used on your installation

The first step to make this machine known to Hyper-V is to create a Symbolic link to the &lt;GUID&gt;.xml configuration files, in the following folder:

"%systemdrive%\\programdata\\Microsoft\\Windows\\Hyper-V\\Virtual Machines"

We use the mklink command built into cmd.exe

C:\\&gt;mklink "%systemdrive%\\programdata\\Microsoft\\Windows\\Hyper-V\\Virtual Machines\\2F855D88-F990-47BA-95D6-0029BCD8C059.xml" "V:\\manualrecover\\Virtual Machines\\2F855D88-F990-47BA-95D6-0029BCD8C059.xml"

The VM Name should now already appear in Hyper-V Manager. When using Windows Server 2008 R2, you might need to restart the vmms service to make the VMs visible at this point.

We need to add the Service SID to this Symbolic link so that Hyper-V is allowed to access it

C:\\&gt;icacls "%systemdrive%\\programdata\\Microsoft\\Windows\\Hyper-V\\Virtual Machines\\2F855D88-F990-47BA-95D6-0029BCD8C059.xml" /grant "NT VIRTUAL MACHINE\\2F855D88-F990-47BA-95D6-0029BCD8C059":(F) /L

Note the /L parameter to indicate we work on a symbolic link

We also need to give the Service SID access to all files of our VM

C:\\&gt;icacls v:\\manualrecover\\ /T /grant "NT VIRTUAL MACHINE\\2F855D88-F990-47BA-95D6-0029BCD8C059":(F)

Note for simplicity we give Full Control, whereas the default is more granular

  
If your machine had Snapshots, we also need to create another symbolic link

The snapshot itself has yet another guid.xml found in the snapshots folder.

C:\\&gt;mklink "%systemdrive%\\ProgramData\\Microsoft\\Windows\\Hyper-V\\Snapshots\\7DD74401-C2B4-4BD9-8079-3D48D8A78B32.xml" "V:\\manualrecover\\Snapshots\\7DD74401-C2B4-4BD9-8079-3D48D8A78B32.xml"

Also give the Service SID access here too:

  
C:\\&gt;icacls "%systemdrive%\\ProgramData\\Microsoft\\Windows\\Hyper-V\\Snapshots\\7DD74401-C2B4-4BD9-8079-3D48D8A78B32.xml" /grant "NT VIRTUAL MACHINE\\2F855D88-F990-47BA-95D6-0029BCD8C059":(F) /L

  
You will need to do the above for each individual snapshot!

  
Before starting the VM, open the settings of the VM and assign the Network Adapters to the correct Switches, as those need to be created newly on the switch.

  
NOTE:

This is not a supported way of adding a VM to Hyper-V. Use this just for disaster recovery, and once you are able to run the VM, backup your VM and recreate it from scratch.

If you restore to a different drive/ or folder, you may need to manually adjust the path to the VHD, and having Snapshots will make this far more complicated .

### <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=SteelEye&action=edit&section=3 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://blogs.msdn.com/b/robertvi/archive/2008/12/19/howto-manually-add-a-vm-configuration-to-hyper-v.aspx" rel="nofollow">http://blogs.msdn.com/b/robertvi/archive/2008/12/19/howto-manually-add-a-vm-configuration-to-hyper-v.aspx</a>
```