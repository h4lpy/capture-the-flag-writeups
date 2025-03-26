# File Access Analysis via Jumplists

In this lesson, we will discuss what jumplists are and how they can be used to determine the files accessed, the corresponding programs used, and the timestamps of these activities. This amount of information gives us the upper hand during investigations and determining who can access what and when.

The jumplists feature was first introduced with Windows 7 and continued in later versions of Windows systems including Windows 11. The feature is designed to provide the user with quick access to recently accessed application files and common tasks. 

The records maintained by jumplists are considered an important source of evidentiary information during investigations. The analysis of jumplist files can provide valuable information about users’ historical activity on the system such as file creation, access, and modification. Examiners can utilize data extracted from jumplist files to construct a timeline of user activities. What makes this artifact more valuable is the fact that the information is maintained on the system long after the source file and application have ceased to exist.

This means that even if the USB was inserted long ago, Jumplists maintain the information on the host system where the USB was connected.

There are 2 types of jumplists in Windows:

- Automatic destinations
- Custom destinations

Each file consists of a 16-digit hexadecimal number which is the AppID (Application Identifier) followed by automaticDestinations-ms or customDestinations-ms extension. Note that these files are hidden and going through Windows Explorer will not reveal them even if you turn on hidden items in Windows Explorer. They can be viewed by entering the full path in the Windows Explorer address bar.

The AutomaticDestinations jumplist files are located in the following directory:

- **`C:\%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations`**

The `CustomDestinations` jumplist files are located in the following directory:

- **`C:\%UserProfile%\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations`**

Let's start analyzing this. We will be using the “JumpList Explorer” tool by Eric Zimmerman to analyze jumplists. You can download the tool from here(It's free.):

**JumpList Explorer** : [https://ericzimmerman.github.io/#!index.md](https://ericzimmerman.github.io/#!index.md)

Run the tool as administrator and navigate to the path wherever your custom and automatic destination files are. In the case of live analysis of the system, they would be located in the path mentioned above. In the lesson lab, we will provide you with these acquired files so their path will be changed and mentioned in the lab portion.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_1.png)

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_2.png)

Select all these files and click open.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_3.png)

Now do this again for custom destination files.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_4.png)

If you face any error like this, ignore this and click ok.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_5.png)

In our case, all of the custom destination files were empty and were not loaded automatically. It is normal behavior.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_6.png)

Here we see detailed information.

We can see jumplist files for different applications.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_7.png)

Here, we can observe the quick access jumplist, which gives general information about all recently accessed files on the system. If we click Notepad we see all the files that were accessed using Notepad.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_8.png)

Here we can see an interesting file. Previously we identified that USB was assigned the “E:” Drive. Now we can see that a file called “Dumped_Passwords.txt” was opened using Notepad from the drive. This means that the suspect opened the USB drive, navigated to the directory “Secret_Project_LD” and accessed the “Dumped_Passwords.txt” file.

To see a more focused view, click on the relevant entry on the left.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_9.png)

Now our information is more clear.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_10.png)

We can see the timestamp when this file was accessed.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_11.png)

We can also see the Local full path of the file.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/5.File+Access+Analysis+via+Jumplists/image5_12.png)

In this part of the course, file access analysis with jumplists for USB forensics is mentioned. The next part of the course will cover " **Automated USB Parsers Tools** ".

-----

75fdacd8339bac18

Note: Use the "C:\Users\LetsDefend\Desktop\QuestionFiles\Acquisition.zip" file for solving the questions below.

1. What is the name of the binary that was executed from the USB?

```
Entry_fix21.exe
```

2. The SOC team confirms that this binary is a legitimate RMM tool. Can you find the original app name?

```
AnyDesk
```

3. When was this binary executed on the system?

Answer Format: `YYYY-MM-DD HH:MM:SS`

```
2023-11-13 08:33:15
```