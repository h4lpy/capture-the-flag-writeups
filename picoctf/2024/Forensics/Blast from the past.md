# Blast from the past

The judge for these pictures is a real fan of antiques. Can you age this photo to the specifications? Set the timestamps on this picture to `1970:01:01 00:00:00.001+00:00` with as much precision as possible for each timestamp. In this example, `+00:00` is a timezone adjustment. Any timezone is acceptable as long as the time is equivalent. As an example, this timestamp is acceptable as well: `1969:12:31 19:00:00.001-05:00`. For timestamps without a timezone adjustment, put them in GMT time (+00:00). The checker program provides the timestamp needed for each. Use this [picture](https://artifacts.picoctf.net/c_mimas/90/original.jpg).

Additional details will be available after launching your challenge instance.

-----

Our task is to change all of the timestamps within the file's metadata to `1970:01:01 00:00:00.001+00:00`.

Using `exiftool`, we see a number of timestamps are set. We can change these with the following:

```
$ exiftool -AllDates='1970:01:01 00:00:00.001 original.jpg
```

Connecting to the instance passes 3/7 tests, failing on timestamps `SubSecCreateDate`, `SubSecDateTimeOriginal`, and `SubSecModifyDate`. We can add these in with `exiftool`:

```
$ exiftool -SubSecCreateDate='1970:01:01 00:00:00.001' -SubSecDateTimeOriginal='1970:01:01 00:00:00.001' -SubSecModifyDate='1970:01:01 00:00:00.001' original.jpg
```

When connecting to the instance this passes 6/7 tests with the final referencing a `Samsung: Timestamp`. Running `strings` on the file identifies the following line which matches the failed test.

```
Image_UTC_Data1700513181420
```

We can change this to `Image_UTC_Data000000000001` within a hex editor.

To get the flag, we upload our modified picture and check the testing instance:

```
MD5 of your picture:
412331ca77b633d2529dc0e0ab5ad6eb  test.out

Checking tag 1/7
Looking at IFD0: ModifyDate
Looking for '1970:01:01 00:00:00'
Found: 1970:01:01 00:00:00
Great job, you got that one!

Checking tag 2/7
Looking at ExifIFD: DateTimeOriginal
Looking for '1970:01:01 00:00:00'
Found: 1970:01:01 00:00:00
Great job, you got that one!

Checking tag 3/7
Looking at ExifIFD: CreateDate
Looking for '1970:01:01 00:00:00'
Found: 1970:01:01 00:00:00
Great job, you got that one!

Checking tag 4/7
Looking at Composite: SubSecCreateDate
Looking for '1970:01:01 00:00:00.001'
Found: 1970:01:01 00:00:00.001
Great job, you got that one!

Checking tag 5/7
Looking at Composite: SubSecDateTimeOriginal
Looking for '1970:01:01 00:00:00.001'
Found: 1970:01:01 00:00:00.001
Great job, you got that one!

Checking tag 6/7
Looking at Composite: SubSecModifyDate
Looking for '1970:01:01 00:00:00.001'
Found: 1970:01:01 00:00:00.001
Great job, you got that one!

Checking tag 7/7
Timezones do not have to match, as long as it's the equivalent time.
Looking at Samsung: TimeStamp
Looking for '1970:01:01 00:00:00.001+00:00'
Found: 1970:01:01 00:00:00.001+00:00
Great job, you got that one!

You did it!
picoCTF{71m3_7r4v311ng_p1c7ur3_83ecb41c}
```