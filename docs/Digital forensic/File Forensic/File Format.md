# File Format
This page aims to give trick regarding the viewing, extraction and even repairing of specific files.

## The most interesting files format

In the case that you retrieved deleted files but don't have access to their original names, you should in a first time, sort the file by their extension. Once you done that, consult the files which have the following extensions if you want to grab valuables informations

???+ faq "Interesting files format"
	Please note here that the extensions mentionned are specific to some OS. Universally speaking, you should try to locate any images, documents...

	=== "Windows"
		|Name of the format|Explanation|
		|------------------|-----------|
		|doc|Microsoft word document|
		|db|Database file
		|evt|Event file|
		|edb|Microsoft Exchange Server files|
		|wab|Contact Address format|

    === "Mac OS X"
		|Name of the format|Explanation|
		|------------------|-----------|
		|keychain|Keychain file which contains all the login used by the user|
		|pslist|?|

    === "Linux"
		|Name of the format|Explanation|
		|------------------|-----------|
		|?|?|

    === "Android"
		|Name of the format|Explanation|
		|------------------|-----------|
		|?|?|

    === "iOS"
		|Name of the format|Explanation|
		|------------------|-----------|
		|?|?|


## File format list
This sections aims to give explanation regarding the existings differents files formats and tricks on how you can read them to extract information.

### ANI

.bmp format is a proprietary format developed by Microsoft for Windows. the main use is to create icons.

??? note "Viewing .ani file"
	These file can be viewed by any image viewer.

### ASP

.asp (active server page) format is a format developped by Microsoft for its service called Microsoft IIS. It generates the content to display in HTML to the browser on the fly.

??? note "Viewing .asp file"
	These file can be viewed by any text editor

### AVI

.bmp format is a format which is used to contains videos.

??? note "Viewing .avi file"
	These file can be viewed by any video viewer.

### BAT

.bat format is a proprietary type of file developed by Microsoft for Windows. the main use is to create scripts launchable from a windows terminal.

??? note "Viewing .bat file"
	The content of these file can be viewed by using any text editor.

### BMP

.bmp format is a proprietary format developed by Microsoft for Windows. the main use is to create images.

??? note "Viewing .bmp file"
	These file can be viewed by any image viewer.

### BUP

.bup format is proprietary format used by the McAfee antivirus to quarantine dangerous files on the system  

??? note "Viewing . file"
	These file are XOR by the software. to revert the files back to its original state, you need to perform a XOR operation on the file by using the hexadecimal key 0x2A. You can perform this operation by using this (Python script)[] or (Cyberchef)[]

### C

.c format is a format which contains source code written in C. 

??? note "Viewing .c file"
	These file can be viewed by any text editor.

### CAB

.cab (Windows Cabinet Files or Diamond files) format is a compression format developed by Microsoft.

??? note "Viewing .cab file"
	These files can opened by using the software 7-Zip for example.

### CHM

.chm format is a format developed by Microsoft to contains informations saved in a compressed HTML format. It is generally used for Windows program manual.

??? note "Viewing .chm file"
	These files can be viewed by using any PDF viewer such as Okular.

### CLASS

.class file are Java Bytecode file or compiled java source code. 

??? note "Viewing .class file"
	These files may be decompiled by using the following softwares : (TO BE COMPLETED)

### CP_

To be completed
??? note "Viewing .cp_ file"

### CSV

.csv files are comma-separated values files.

??? note "Viewing .csv file"
	They can be open by using any text editor or excel-like software for better visualisation.

### DAT

.dat files are generic data files which can contains anything... They can be used as configuration, storage files for softwares or even videos... You can find them anywhere. 

??? note "Viewing .dat file"
	There is no universal method to view these type of files since they can contains anything, the best way to analyze them is to find in which software they are used in. Else, you can try to view with a text, hexadecimal editor software...

### DB

.db files are, in general, database files which containes informations in a structured database format. 
Please keep in mind that files with a .db extension aren't always databases such as Thumbs.db which contains the icons of pictures files for example.

??? note "Viewing .db file"
	Try softwares such as "DB browser for SQLite", "Microsoft access" or even a simple text editor to view the content of these files.

### DBX

.dbx files are files which contains outlook emails messages. 

??? note "Viewing .dbx file"
	Even though they aren't the most suited tool, you can use a text editor to view the content of these file"

### DLL

.dll files are libraries for exe files. 

??? note "Viewing .dll file"
	These files can be decompiled by using tools such as IDA free, Ghidra...

### DOC, DOCX, ODT

.doc and .docx files are files used by the software Microsoft Word whereas .odt files are used by the softwares LibreOffice Text and OpenOffice Text.

??? note "Viewing these types of file"
	These files can be open by using any Word-like software such as OpenOffice, Libreoffice. 

??? note "Extracting data from theses files without executing them"
	In case you don't want or can't open the file for some reason (Corrupted, malicious file...) you can still extract data from it. Since these files are archive, you can change the current extension to .zip and then proceed to extract its content like you would do with a ZIP file. 

### DXF

.dxf (Drawing Exchange Format) files was developed by Autodesk as a type of universal format for storing CAD models.

??? note "Viewing .dxf file"
	These files can be opened by using (TO BE COMPLETED)

### EDB

.edb files are structured database format file used by Microsoft Exchange Server software to store mailbox data from process and non-SMTP messages.

??? note "Viewing .edb file"
	These files can be viewed with any text, hexadecimal editor.

### EVT

.evt files are files created by Windows Event Viewer to logs system event for debug purposes. Their structure depends on the .dll files of the program that generated these same logs.

??? note "Viewing .evt file"
	These files can be viewed by using any text, hexadecimal editor software. You can also use the following tools available on Github to parse and properly view these types of files. 


### EXE

.exe are Windows Executable files.

??? note "Viewing .exe file"
	These files can be decompiled by using tools such as Ghidra, IDA Free...

### F

No information has been found regarding this extension.

??? note "Viewing .f file"
	You may need to analyze the file before attempting to execute it for security purposes.

### GIF

.gif files are a popular format for animated images.

??? note "Viewing .gif file"
	These files can be viewed by any image viewer software

### GZ

.gz files are compressed files in gz format.

??? note "Viewing .gz file"
	These files can be uncompressed by using software like 7-zip...

### H

.h files are header files for the C,C++,C# programming languages.

??? note "Viewing .h file"
	These files can be viewed with any text editor.

### HDR

.hdr (High Dynamic Range) files are images files.

??? note "Viewing .hdr file"
	These files can be opened by using softwares like Adobe Photoshop for example.

### HTML

.html (Hypertext markup language file) files are the standard web page format type on the format.

??? note "Viewing .html file"
	They can be opened by using web browser, text editor... The choice is yours here.

### ICC

.icc files is a color profile format standardized by the International Color Consortium
??? note "Viewing .icc file"
	They can be opened by using (TO BE COMPLETED)

### ICO

.ico files are image file format for computer icons in Microsoft Windows.

??? note "Viewing .ico file"
	These files can be viewed by using any image viewer software.

### IDF

.idf files are used interoperate between electronic design automation (EDA) software and solid modeling mechanical computer-aided design (CAD) software.

??? note "Viewing .idf file"
	These files can be opened by using (TO BE COMPLETED)

### INF

.inf files are configuration files for Windows programs, drivers...

??? note "Viewing .inf file"
	These files can be opened by using text editor softwares.

### INI

.ini files are message configuration documents for Windows and MS-DOS programs.

??? note "Viewing .ini file"
	These files can be opened by using text editor softwares.

### JAVA

.java files are source code files written in java (programming language).

??? note "Viewing .java file"
	These files can be opened by using text editor softwares.

### JPG

.jpg files is format used to represent images.

??? note "Viewing .jpg file"
	These files can be viewed by using any image viewer softwares.

### JSP

.jsp (Java Server Pages) files are, like .php files, files which returns HTML code once executed by the web server (Except here that instead of PHP, we use java to generate the output of the file).

??? note "Viewing .jsp file"
	These files can be opened by using any text editor softwares.

### LNK

.lnk files are used to create a shortcut to the original files (Document, pictures...), folder...

??? note "Viewing .lnk file"
	These files can be viewed by using any text editor softwares.

### MAX

.max format is used to represent data in a three-dimensional scene for video games, movies...

??? note "Viewing .max file"
	These files may be opened by using Autodesk 3ds Max.

### MDB

.mdb files are database structured format files used by Microsoft Access software up to the 2003 version. This format is replaced by the ACCDB one.

??? note "Viewing .mdb file"
	These files can be opened by using Microsoft Access 2003 or "DB Browser for SQLite database" software

### MID

.mid files are Musical Instrument Digital (MID) interface files, it doesn't include audio but notes, timing, duration and desired loudness for each note.

??? note "Viewing .mid file"
	These files can be opened by using (TO BE COMPLETED)

### MOV

.mov files are Apple QuickTime Movie files which are basically videos.

??? note "Viewing .mov file"
	These files can be opened by using VLC software for example.

### MP3

.mp3 are audio files.

??? note "Viewing .mp3 file"
	These files can be opened by using VLC, Audacity softwares...

### MPG

.mpg are videos files.

??? note "Viewing .mpg file"
	These files can be opened by using VLC software

### MSG

.msg format is a proprietary type of file developped by Microsoft for Outlook, they can contains email sent from the local pc.

??? note "Viewing .msg file"
	You can use the script msg_extract from github to view .msg files

### PCT

.pct files are Macintosh pict image.

??? note "Viewing .pct file"
	These files can be opened by using any image viewer software.

### PDF

.pdf files are document files format created by Adobe Systems.

??? note "Viewing .pdf file"
	These files can be opened by using any PDF viewer like acrobat reader for example.

### PF

.pf files are files used by Microsoft Windows for its prefetch trace files.

??? note "Viewing .pf file"
	These files can be opened by using any text editor

### PNG

.png files are images files.

??? note "Viewing .png file"
	These files can be opened by using any image viewer, editor software.

### PPT

.ppt files are files used by Microsoft power point software to create presentations.

??? note "Viewing .ppt file"
	These files can be opened by using any "Microsoft power point"-like softwares.

### PS

.ps files are images saved in the Adobe PostScript language.

??? note "Viewing .ps file"
	These files can be opened by using (TO BE COMPLETED)

### REG

.reg files are registration files used by the Windows registry.

??? note "Viewing .reg file"
	These files can be opened by using any text editor software

### RTF

.rtf (Rich Text Format) file is text format which can includes extra informations regarding font style, formatting, images...

??? note "Viewing .rtf file"
	They can be opened by using any "Word"-like or simple text editor softwares

### SVG

.svg files are vectors files used to display two-dimensional graphics, charts and illustrations on web site.

??? note "Viewing .svg file"
	These files can be opened by using any image viewer software

### SWC

.swc files are package of precompiled Flash symbols for .swf files.

??? note "Viewing .swc file"
	These files can be opened by using (TO BE COMPLETED)

### SWF

.swf files are Shockwave Flash Movie file created by Adobe.

??? note "Viewing .swf file"
	These files can be decompiled by using (TO BE COMPLETED)

### TIF

.tif files are images created in the GeoTIFF format.

??? note "Viewing .tif file"
	These files can be opened by using any image viewer software.

### TTF

.ttf files are font files for text.

??? note "Viewing .pct file"
	These files can be opened by using font files editor softwares

### TXT

.txt files are text files.

??? note "Viewing .txt file"
	These files can be opened by using any text editor software

### URL

.url are internet shortcut files.

??? note "Viewing .url file"
	These files can be opened by using any text editor software.

### WAB

.wab files are used by Microsoft Outlook Express software to contains contact data.

??? note "Viewing .wab file"
	These files can be opened by using text editor softwares.

### WAV

.wav (Waveform Audio file format) files are audio files.

??? note "Viewing .wav file"
	These files can be opened by using VLC, audacity softwares...

### WDB

.wdb files are database files used by Microsoft Works software.

??? note "Viewing .wdb file"
	These files can be opened by using text editor softwares.

### WK4

.wk4 files are files used by the software Lotus1-2-3.

??? note "Viewing .wk4 file"
	They can be opened by using any text editor.

### WMA

.wma (windows Media Audio) files are proprietary format developed by Microsoft.

??? note "Viewing .wma file"
	These files can be viewed by using VLC, audacity format...

### WMF

.wmf (Windows Meta File) are proprietary image format developped by Microsoft.

??? note "Viewing .wmf file"
	These files can be opened by using (TO BE COMPLETED)

### WMV

.wmv files are video files.

??? note "Viewing .wmv file"
	These files can be opened by using VLC software for example.

### WPS

.wps files are files created by Microsoft Works. They are almost identical to .doc files except for one or two minor differences.

??? note "Viewing .wps file"
	You can try to convert them into doc files with the right tools.

### XLS

.xls files are used by Microsoft Excel software.

??? note "Viewing .xls file"
	These files can be opened by using "Excel"-like software.

### XML

.xml files are structured text files which can be used for a variety of purposes.

??? note "Viewing .xml file"
	These files can be viewed by using any text editor software.

### ZIP

.zip files are compressed files.

??? note "Viewing .zip file"
	They can be opened by using appropriate software such as 7-zip, winrar...


### Resources
Please note that the informations mentionned earlier were obtained by checking the following websites.

https://en.wikipedia.org/wiki/List_of_file_formats

https://fileinfo.com

https://docs.fileformat.com

https://www.lifewire.com

https://file.org



