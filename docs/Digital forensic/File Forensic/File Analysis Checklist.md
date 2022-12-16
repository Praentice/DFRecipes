The aim of this page is to provide tips and tricks regarding the analysis of unknown files. 

## Checklist for file identification

Here's a list of actions you can do to attempt the identification of an unknown file : 

1) Check the file extension and search it on the internet or [here](File format)

2) Launch the command `file` on the unknown file and check the output

3) Launch the command `strings` (UTF-8) and `strings -el` (UTF-16) on the unknown file and check the output

4) Check the first bytes of the unknown file using a hexadecimal editor

5) Check if the unknown file is ciphered or not

6) Is the unknown file a capture of a binary data transfer ? Try to read the file as a dump of something and not as a file. 