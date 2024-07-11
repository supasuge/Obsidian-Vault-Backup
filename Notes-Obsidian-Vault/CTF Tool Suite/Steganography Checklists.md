## Table of Contents

- [Steganography](#steganography)

# Steganography
A method to hiding something in something.
**General**
- Usually when organizer gave us Image, Music, Video, Zip, EXE, File System, PDF and other files, it a _steganography_ or _forensics_ challenge. Run `file` command first.
- Metadata is important. Checkout the EXIF data of the file by using `exiftool [filename]` command.
Try issuing `binwalk [filename]` on the file. They may hide another file in the file.
- To extract, use `binwalk -e`.
- To extract one specific signature type, use `binwalk -D 'png image:png' [filename]`.
- - To extract all files, run `binwalk --dd='.*' [filename]`.
- Try file carve using `foremost -v [filename]` command. Foremost support all files.
**Images**
- View the image first
Use `strings` command to that file.
- Try `grep -i [any strings you want to filter]` from the `strings` command output.
- Example `grep -i "flag{"` to filtering the flag format only. `-i` option to unable case sensitive.    
- Google the images, differentiate the `md5hash`. If you found same image but have a different md5 hash, it may probably have been altered.
- Analyse the header and the content of the file using any **hex editor**.
- Know the **file signature**. Maybe they gave us corrupt header! So fix it!

- Maybe **zoom-in** and **zoom-out** method can get the flag.

- to reverse search the image in the internet.
- Use `imagemagick` command tool to do image manipulation.
- Use **Stegsolve.jar** tools. There are so many CTF I've participated that I used this tool to unhide flag from an image.
- File carve using `steghide --extract -sf <filename>`. Try find the password with your own-self. Maybe, the organizer will give hints or the password may in another file.
- Check for any corruption on PNG file by using `pngcheck <filename.png>` command.
- Detect stegano-hidden data in PNG & BMP s by `issuing zsteg -a <filename.png>`.

- Use **SmartDeblur** software to fix blurry on image.
- Use `stegcracker <filename> <wordlist>` tools Steganography brute-force password utility to uncover hidden data inside files.
- Use `tesseract` to scan text in image and convert it to .txt file.

- Another powerfool tool is called `zsteg`.

Steganosuite

- - Extract data from image (-x)
        
    

Some of online stegano decoder :-

​

- ​
    

​

**Compressed file**

Unzip it.

- Use `zipdetails -v` command to display details about the internal structure of a Zip file.

- Use `zipinfo` command to know details info about Zip file.
 

- Use `zip -FF input.zip --out output.zip` attempt to repair a corrupted zip file.

- Brute-force the zip password using `fcrackzip -D -u -p rockyou.txt filename.zip`

- To crack 7z run `7z2hashcat32-1.3.exe filename.7z`. Then `john --wordlist=/usr/share/wordlists/rockyou.txt hash`


**Music file**

- Use `binwalk` first. They may embedded something in the file.
  

- Use **Audacity**.
 

- Use **Sonic Visualizer**. Look at spectogram and other few Pane.
 

- Use **Deepsound**.
 

- Use **SilentEye**.
 

Some of online stegano decoder for music:-

​

- - ​
        
    

**Text**

Use

- that can decode hide message in spam text.
    

**PDF**

- qpdf
    

- PDFStreamdumper
    

- pdfinfo
    

- pdfcrack
    

- pdfimages
    

- pdfdetach
    

- pdf-parser.py -v <file>
    

- pdftotext
    

peepdf -if <filename>

- - object <value>
        
    

pdfid