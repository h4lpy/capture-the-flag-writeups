# Folder Access Analysis via Shellbags

In this lesson, we will discuss the analysis of shellbags to determine what folders were present and accessed in the USB device by the suspect/attacker. This can be very helpful as oftentimes folder names and folder hierarchy can reveal much about the motivation or target of attackers. Let's first learn a bit about shellbags.

Shellbags are artifacts that are created when a user interacts with the shell, the user interface for accessing the operating system, and its file system. Interestingly, in Windows, this is a GUI-based file explorer (don't get confused with shell referring to CLI). Shellbags contain information about the state of a folder, such as its size, position, and the items that it contains. This information is stored so that when the user accesses the folder again, the folder can be displayed in the same state as it was when the user last interacted with it. For example, you may have set your folder view to smaller or bigger or rearranged the order in which folders are displayed in File Explorer. This information is stored in shellbags so this configuration persists.

Shellbag artifacts can be useful in a variety of forensic contexts, including investigations of cybercrime, employee misconduct, data breaches, and especially USB forensics cases. It can provide insight into the user's activities and the folders that they have accessed. For example, if a user has accessed a folder containing sensitive files in a USB, the shellbag for that folder may contain information about the name and location of those documents. They can be particularly useful for tracking the activities of a user who is attempting to cover their tracks by deleting or moving files. In these cases, the information stored in the shellbags may be the only record of the user's activities and can provide valuable evidence.

Shellbags are stored in the registry at the following locations:

- `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`
- `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`
- `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`

We will use ShellbagExplorer by Eric Zimmerman to analyze this artifact. You can download the tool from here(It's free.):

**ShellbagExplorer** : [https://ericzimmerman.github.io/#!index.md](https://ericzimmerman.github.io/#!index.md)

Let's start analyzing and see it in action. First, run Shellbag Explorer as admin. Then go to file and select either active registry or offline hive based on your requirement. For now, we will use active registry but for the lab of this lesson we will provide you with registry hives so you have to select the offline hive and select the “`NTUSER.dat`” or the “`USRCLASS.dat`” file in there.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_1.png)

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_2.png)

Here we are interested in the “`E:`” drive which was assigned to the USB in the initial connection to the system(Identified in the previous lesson). If we expand that:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_3.png)

We see a directory named “`Secret_Project_LD`”. Let's click on this to get more information.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_4.png)

Let's go over this information.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_5.png)

Here we see the name of this item (Directory) and the shell type indicating this is a directory. Below that we see the timestamps like when the directory was first accessed, last accessed, etc. This all can be very interesting in determining when a certain user accessed the directory.

You may be thinking, how do we know which user accessed this directory? Just to clarify, shellbags are stored under the “`NTUSER.dat`” hive and this hive is user-centric, meaning each user on a system has their own unique “`NTUSER.dat`” hive. So, whichever users hive we are analyzing accessed that directory, which in our case is “`letsdefend`”.

Now, here's a bonus for you: Shellbags aren't restricted to folders alone. They also record any zip files found within a folder if accessed via Explorer. Moreover, they record folders contained within a zip file if explored through Explorer and the zip isn't password-protected. To demonstrate, we've reinserted the USB and accessed it in File Explorer.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_6.png)

If we expand the zip we can see the folder inside:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/4.Folder+Access+Analysis+via+Shellbags/image4_7.png)

It's important to note that the folders under the zip only get stored in shellbags if it's visited in Explorer. So the fact that we can see what folders were under the zip files means that the user visited that folder in Explorer and we have evidence of access.

In this part of the course, folder access analysis via shellbags for USB forensics is mentioned. The next part of the course will cover " **File Access Analysis via Jumplists** ".

-----

**Note:** Use the "`C:\Users\LetsDefend\Desktop\QuestionFiles\Acquisition.zip`" file for solving the questions below.

1. What is the name of the directory in the USB device that was malicious and accessed by the user?

```
C2Initialentry
```

2. When was this directory accessed by the user?

Answer Format: `YYYY-MM-DD HH:MM:SS`

```
2023-11-13 08:30:58.000
```