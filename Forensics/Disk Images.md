# Disk Images
*Downloads, downloads, downloads...*
Some help for the (unfortunately rather uncommon) disk image challenges you might come across!

## Tools for Analysing Disk Images:
- Autopsy (my personal reccomendation)
- FTK Imager


## Autopsy
A wonderful and powerful tool for the analysis of disk images. By default, it will run ingest modules when provided with a disk image such as `Exif Parser` and `Extension Mismatch Detector` among many others. However, these can take a little while to run if you leave them all on by default - especially with larger images. Modules such as `Hash Lookup` can very easily be left disabled for CTFs as this is something that is usually used in actual forensic investigations: comparing a list of hashes of "known bad" files to the files that are in the disk image.
Usually, the disk image will contain files that you will also have to analyse and mess with to get flags. A good example of this is RACTF2020's "Disk Forensics Fun" where a Linux Alpine image that contains files is given. I would recommend trying this challenge out for practice, as well as the other forensics challenges from RACTF2020.
