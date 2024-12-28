# Trivial Flag Transfer Protocol
## Problem
- Figure out how they moved the flag.
## Thought Process
- `.pcapng` so I opened it with WireShark. 
- Exported Objects as TFTP
- Now I have these Files
![image](https://github.com/user-attachments/assets/42dc766b-061c-483c-9c76-efa438b1694d)
- `instructions.txt` looks like it is encrypted, so I used a simple ROT13 online.
![image](https://github.com/user-attachments/assets/2a4d67b8-5ea9-4956-bdd0-7528be3c371c)
- `TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN`
- This suggests the plan might contain something, so I opened the plan file using vim.
![image](https://github.com/user-attachments/assets/251d877a-144b-462f-8991-3f52a4e73c46)
- Time to check out the `.bmp` files.
- The program contains steghide
- So we use steghide to decrypt the data from the 3 pictures
- The dash between with and duediligence suggests that duediligence is the passphrase
- Didn't work at first, so I tried it in all caps, as the cipher decoded it.
```console
steghide extract -sf picture3.bmp -p DUEDILIGENCE
```
- ![image](https://github.com/user-attachments/assets/2689bb0c-c813-4618-ac49-57c3057f7d7e)
- Time to read the flag.
- Flag: `picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`
# Tunn3l V1s10n
## Problem
- We found this file. Recover the flag.
## Thought Process
- Tried opening the file, big mistake, crashed my WSL.
- Opened it with a hex editor
![image](https://github.com/user-attachments/assets/fdda6f81-f30f-418d-ae57-48b7b0fb326f)
- BM suggests that it is a bitmap file
- Saving it as a `.bmp` file, expecting it to be openable now.
- Of course, it still cannot be opened. Probably contains further corruption in the file header.
- `BA D0` is probably the corrupted hex.
- `0x0a` is the offset, i.e. starting address, of the byte where the bitmap image data (pixel array) can be found.
- Using the most common DIB header, which is BITMAPINFOHEADER, it extends width and height by 4 bytes.
- adding the sizes, we get 56 or 38 in hex. Substitute the value, and at 0e, put the size of the header.
![image](https://github.com/user-attachments/assets/8bbfa94a-ca28-4332-9514-6b07191295cf)
- lets try setting height to same as the width, at the position according to the wikipedia.
![image](https://github.com/user-attachments/assets/b1bfd00e-ad90-4def-9dfe-d4f248e19c23)
![image](https://github.com/user-attachments/assets/91e258c1-4a50-459e-8224-9d8a765c0339)
flag: `picoCTF{qu1t3_a_v13w_2020}`
# m00nwalk
## Problem
Decode this message from the moon.
## Thought Process
- Cool, so we have an audio consisting of some encrypted name. Visualising and all doesn't work. Using the name of the level and the hint with some research, we discover that it uses SSTV. SSTV stands for slow-scan television, a method for transmitting and receiving static pictures over radio waves. It's primarily used by amateur radio operators to share images in monochrome or color.
- https://github.com/colaclanth/sstv using this decoder
- the 2nd hint suggests the use of Scottie RX option, as the Carnegie-Mellon University mascot is Scotty, the Scottie dog.
```console
sstv -d message.wav -o result.png
```
![image](https://github.com/user-attachments/assets/d20bd2bb-e97e-4b3a-afb1-9ed5f1ad5b59)
![image](https://github.com/user-attachments/assets/6c1df230-8933-43f7-9896-0706331e9072)
flag: `picoCTF{beep_boop_im_in_space}`

# Blast from the Past
## Problem
- Question provides an image, we need to change some dates stored in the data of the image.
## Thought Process
- First tried simply uploading the image, it seems like we have 7 challenges. So 7 dates to Modify
- Time to use `exiftool`
```console
~$ exiftool og.jpg
ExifTool Version Number         : 12.76
File Name                       : og.jpg
Directory                       : .
File Size                       : 2.9 MB
File Modification Date/Time     : 2024:12:28 19:19:35+00:00
File Access Date/Time           : 2024:12:28 19:19:49+00:00
File Inode Change Date/Time     : 2024:12:28 19:19:49+00:00
File Permissions                : -rwxrwxrwx
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Exif Byte Order                 : Little-endian (Intel, II)
Image Description               :
Make                            : samsung
Camera Model Name               : SM-A326U
Orientation                     : Rotate 90 CW
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Software                        : MediaTek Camera Application
Modify Date                     : 2023:11:20 15:46:23
Y Cb Cr Positioning             : Co-sited
Exposure Time                   : 1/24
F Number                        : 1.8
Exposure Program                : Program AE
ISO                             : 500
Sensitivity Type                : Unknown
Recommended Exposure Index      : 0
Exif Version                    : 0220
Date/Time Original              : 2023:11:20 15:46:23
Create Date                     : 2023:11:20 15:46:23
Components Configuration        : Y, Cb, Cr, -
Shutter Speed Value             : 1/24
Aperture Value                  : 1.9
Brightness Value                : 3
Exposure Compensation           : 0
Max Aperture Value              : 1.8
Metering Mode                   : Center-weighted average
Light Source                    : Other
Flash                           : On, Fired
Focal Length                    : 4.6 mm
Sub Sec Time                    : 703
Sub Sec Time Original           : 703
Sub Sec Time Digitized          : 703
Flashpix Version                : 0100
Color Space                     : sRGB
Exif Image Width                : 4000
Exif Image Height               : 3000
Interoperability Index          : R98 - DCF basic file (sRGB)
Interoperability Version        : 0100
Exposure Mode                   : Auto
White Balance                   : Auto
Digital Zoom Ratio              : 1
Focal Length In 35mm Format     : 25 mm
Scene Capture Type              : Standard
Compression                     : JPEG (old-style)
Thumbnail Offset                : 1408
Thumbnail Length                : 64000
Image Width                     : 4000
Image Height                    : 3000
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Time Stamp                      : 2023:11:20 20:46:21.420+00:00
MCC Data                        : United States / Guam (310)
Aperture                        : 1.8
Image Size                      : 4000x3000
Megapixels                      : 12.0
Scale Factor To 35 mm Equivalent: 5.4
Shutter Speed                   : 1/24
Create Date                     : 2023:11:20 15:46:23.703
Date/Time Original              : 2023:11:20 15:46:23.703
Modify Date                     : 2023:11:20 15:46:23.703
Thumbnail Image                 : (Binary data 64000 bytes, use -b option to extract)
Circle Of Confusion             : 0.006 mm
Field Of View                   : 71.5 deg
Focal Length                    : 4.6 mm (35 mm equivalent: 25.0 mm)
Hyperfocal Distance             : 2.13 m
Light Value                     : 4.0
```
- Date to set is 1970:01:01 00:00:00.001
- So, I am going to try doing `-AllDates=''` with exiftool to see how it affects the thing
```console
~$ exiftool -AllDates='1970:01:01 00:00:00.001' og.jpg
    1 image files updated
  ```
![image](https://github.com/user-attachments/assets/911719be-9f9d-4d1b-96b5-4516da716344)
- It appears that the following things need to be corrected:
```
Create Date                     : 1970:01:01 00:00:00.703
Date/Time Original              : 1970:01:01 00:00:00.703
Modify Date                     : 1970:01:01 00:00:00.703
```
- To fix this part, we need to use e.g. `-SubSecCreateDate`
- Time to test after changing these 3
![image](https://github.com/user-attachments/assets/f0a1af1e-6d2d-4cf0-a723-f085cb5ab8ca)
- To change the Samsung TimeStamp, we need to modify the epoch of the file
- It is stored following the `Image_UTC_Data`.
![image](https://github.com/user-attachments/assets/61b95c67-99f2-4a23-b9dd-271d3e11b635)
- Using an online converter such as `https://www.epochconverter.com`, we get to know that the last 3 figures are the milliseconds, so we need to set the epoch to all zeros with a singular 1 at the end.
![image](https://github.com/user-attachments/assets/9e28def1-347f-41ce-83f4-80dd84fdb449)
- I am going to use `bvi` to do this
![image](https://github.com/user-attachments/assets/22fed445-0ab3-462c-8048-dc12f4cc3554)
flag: `picoCTF{71m3_7r4v311ng_p1c7ur3_83ecb41c}`

