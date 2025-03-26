# USB Registry Key

In this lesson, we will discuss few registry paths that store USB-related information.

The first location we will discuss is the USBSTOR key which only holds information about external storage media/drives like USB, hard drive, etc.

**Registry Key:** `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_1.png)

USBSTOR is one of the first pieces of evidence in common forensics analysis including USB devices in the incidents. We can get information regarding the USB like its model and version name, the Windows-assigned serial number, the last connected timestamp, etc. These elements are crucial and can significantly contribute to determining the root cause of incidents, shedding light on the potential role of the USB, if any, in the occurrence.

Let's open up the SYSTEM hive with Registry Explorer. If you don't know how to use Registry Explorer, it is recommended to take this course:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_2.png)

This subkey is located under the USBSTOR key and is generated after connecting a USB to the system. Upon expansion, this reveals a randomly named key, which corresponds to the device's assigned serial number by the USB.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_3.png)

If we click on this we see a wealth of information regarding this device.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_4.png)

We can see the Friendly Name of the attached USB. This can be important in investigations, especially insider threat cases. Another thing to note is the container ID which can be pivotal in contextual analysis from different data sources.

Next, let's expand this key further, then navigate to the 'Properties' key. From there, expand the key that begins with '83daxxx'.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_5.png)

This subkey will contain additional subkeys. Look for the key labeled '0064,' which holds the timestamp indicating when the USB was connected to the system.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_6.png)

In the data section, we can see the timestamp in UTC.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_7.png)

In the “0066” key we can find the timestamp when the USB was disconnected from the system.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_8.png)

This is very important as it will help create a forensics timeline for analysis.

There's another Registry key we want to touch briefly on. The path to this is:

**Registry Key:** `HKLM\SYSTEM\CurrentControlSet\Enum\USB`

This key contains information about all the devices connected through USB ports (For example keyboards, adapters, etc.)

Let's show an example from our case. This key follows the same hierarchy as discussed before.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_9.png)

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_10.png)

As we can see this is a Bluetooth adapter. Similarly, we can confirm the type of device from the Service Value. Here in this example it's BTHUSB which speaks for itself.

In the previous USB example, the service type was disk which confirmed whether it was a mini USB or an external drive attached via a USB port.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/2.USB+Registry+Key/image2_11.png)

In this lesson, we have discussed artifacts from the Windows registry that could be found as part of the analysis. In the next lesson, we will talk about artifacts found in Windows events logs.

-----

**Note:** Use the "`C:\Users\LetsDefend\Desktop\QuestionFiles\Acquisition.zip`" file for solving the questions below. 

1. What is the serial number assigned by Windows to the USB device?

```
5639311262174133917&0
```

2. What is the `ClassGUID` value for the USB device in question?

```
{4d36e967-e325-11ce-bfc1-08002be10318}
```

3. When did the USB Device connect first to the system?  
  
Answer Format: `YYYY-MM-DD HH:MM:SS`

```
2023-11-13 08:32:23
```