# Clusterknoten hinzufügen zu bestehenden Cluster

<div class="vector-body" id="bkmrk-make-sure-the-third-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Make sure the third node meets the requirements to run Windows Server 2008 (R2) and Hyper-V
2. Connect the third node to power, network and the SAN.
3. Update the BIOS to the latest (stable) version and any relevant hardware firmwares. Afterwards, configure the settings in the BIOS of the third node to allow virtualization.
4. Install the Operating System on the third node. Make sure you install the same edition of Windows Server 2008 or Windows Server 2008 R2. When your current nodes are Server Core installations, make sure the third node is a Server Core installation as well.
5. Change the name of the third node and restart. Configure time and date, network, antivirus, remote desktop and Uninterruptible Power Supply (UPS) settings on the third node in line with the other two nodes. Make sure the settings are correct. Check connectivity after changing settings. Reboot the server once to make sure the settings stick.
6. Configure Windows Update settings. Make sure Microsoft Update is used, when running a Full Installation of Windows Server 2008 or Windows Server 2008 R2. Update the server. Reboot when asked.
7. Install the Hyper-V role and reboot when done. After installation remove any Hyper-V networks and create the virtual networks in line with the other two nodes. Make sure you use the same name (with exactly the same name and capitilzation) for your virtual networks as on the other two nodes.
8. Install the Failover Clustering feature. Optionally (when required) install the MultiPath IO feature.
9. Make the third node a member server in the Active Directory domain, where the other two servers and the cluster are also part of.
10. Connect your node to the SAN. Read the manual of the SAN for steps.
11. Make your node a member of the cluster by issuing the following command line: ```
    cluster.exe /cluster:NameOfTheCluster /add /node:NameOfTheThirdNode
    ```
12. In the Failover Cluster Manager MMC, change your Cluster Quorum settings to resemble the most optimal settings, by right-clicking the cluster name in the left hand pane and selecting the 'Configure Quorom Disk' option.

</div></div></div>Quelle:

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en/winserverhyperv/thread/86ba2313-8bd8-42fd-adfd-b89af38695fe" rel="nofollow">http://social.technet.microsoft.com/Forums/en/winserverhyperv/thread/86ba2313-8bd8-42fd-adfd-b89af38695fe</a>
```