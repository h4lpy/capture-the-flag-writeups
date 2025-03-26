# USB Event Logs

While registry artifacts offer substantial value in USB device investigations, we'll delve into lesser-known log sources native to Windows, enriching our analysis with additional context. In forensics it's always a good practice to have multiple data sources pointing to the same fact/information, minimizing room for error.

We will discuss these event logs:

- 1. Partition
- 2. Kernel-PnP
- 3. NTFS

Open the event viewer and go to: “Application and Services Logs” -> Microsoft -> Windows.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_1.png)

Now if you scroll you will find many different log sources, a few of which we will be discussing here.
## Partition

Now scroll to the partition log source from the above-mentioned log sources.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_2.png)

We are interested in event ID 1006:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_3.png)

If we go to the details tab of the event corresponding to the timestamp identified in the registry artifacts we can see detailed information about the connected USB. Furthermore, the timestamp of this event aligns with the time of the USB connection, further affirming the timestamp is 08:05:07 AM on October 23, 2023.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_4.png)

We can see all the detailed information such as the Disk size of the USB in bytes, the serial number, the manufacturer, and the model.

## Kernel-PnP

Go to the relevant log source.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_5.png)

We are interested in Event IDs 400 and 410:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_6.png)

Let's look closer into the event id 400:

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_7.png)

This event is logged when an external device like a USB is configured(backend term for connected) on the system.

We can see that the Device name is the same as we found in our USBSTOR key. Another artifact that aligns with our registry findings is the Class Guid. If you compare, you'll notice it matches the Guid present in the registry.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_8.png)

The timestamp of the event is exactly the same as the previous event log we discussed, as well as the registry.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_9.png)

This further provides evidence of when and which USB was connected to the system, and the metadata related to the USB device.

In the next lesson, we are gonna talk about the Directory paths inside the USB device. These can be used to understand for what purpose it was used and what kind of data contains.

## NTFS

Open the NTFS operational event log and filter for event ID 142. Then look for this event at the time when the USB was connected to the system, a time previously identified from the registry artifacts and the event logs we previously examined.

The NTFS event log is useful for us as it allows us to identify the Disk drive letter that was given to the USB. This helps in further investigation; if we encounter file paths starting with the disk letter assigned to the USB drive, we can get the context and understand that these files belong to that specific USB device.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_10.png)

We accessed the event with ID 142, occurring at the time of the USB connection, as previously identified.

Looking at the general tab of this event, we can note the volume name: "E:". This indicates that the "E:" disk letter was assigned to this specific USB device, and any file paths beginning with "E:" are associated with this device.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/USB+Forensics/3.USB+Event+Logs/image3_11.png)

This will help us in the next lessons as we will be dealing with file and folder paths.

-----

**Note:** Use the "C:\Users\LetsDefend\Desktop\QuestionFiles\Acquisition.zip" file for solving the questions below.  
  
1. Analyze the Kernel PNP event logs to identify the relevant events for the USB device discussed in previous lesson questions. What is the timestamp indicating when the USB device drivers were configured on the system?  
  
Answer Format: `YYYY-MM-DD HH:MM:SS.MS`

```
2023-11-13 08:32:23.69
```

2. Analyze the NTFS operational event logs. What is the lowest value for free space in bytes?

```
9850167296
```