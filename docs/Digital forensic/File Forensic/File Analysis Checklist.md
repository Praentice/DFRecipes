The aim of this page is to provide tips and tricks regarding the analysis of unknown files. 

## Checklist for file identification

Here's a list of actions you can do to attempt the identification of an unknown file : 

1) Check the file extension and search it on the internet or [here](File format).

2) Launch the command `file` on the unknown file and check the output.

3) Retrieve the magic number (The first bytes) of your sample and compare it to these [data](https://www.garykessler.net/library/file_sigs.html)

4) Launch the command `strings` (UTF-8) and `strings -el` (UTF-16) on the unknown file and check the output. Don't hesite to specify other UTF such as UTF-32 to get other results.

5) Check the first bytes of the unknown file using a hexadecimal editor.

6) Check if the unknown file is encrypted.

7) Is the unknown file a capture of a binary data transfer ? Try to read the file as a dump of something and not as a file. 