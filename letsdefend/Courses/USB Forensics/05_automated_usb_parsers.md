# Automated USB Parsers Tools

In this lesson, we will take a look at an automated tool called “USB Detective”. This tool automatically parses all the information and presents it in an organized way for analysis. All we have to do is provide the artifacts(Like the registry hives etc.) to the tool and it parses and presents all the information.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_1.png)

You can download the community edition of the tool from here:

**USB Detective** : [https://usbdetective.com/community-download/](https://usbdetective.com/community-download/)
This tool will be already present in the lab environment for you.

First of all, we will acquire all the artifacts using KAPE. We will not explain this step as this is out of the scope of this course. If you want to learn more about it, please take the Forensic Acquisition and Triage course.

Here we have the acquired artifacts:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_2.png)

Now let's open the “USB Detective” tool.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_3.png)

This is the interface of the tool. We will select the First option “Select Files/Folders”.

This window will appear:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_4.png)

We will enter the case name and add a directory where the results will be stored. At the bottom, we will add the USB artifacts acquisition folder which contains our artifacts.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_5.png)

In our example, we only have 1 USB drive connected to the system for analysis so we only get a single result.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/6.Automated+USB+Parsers+Tools/image6_6.png)

Feel free to play with the tool in the lab if you wish.