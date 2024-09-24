# hsd2ora
A Python script to convert HiPaint .HSD files to .ORA format. Not especially fast, but I could find no other options for converting .HSD files outside of HiPaint itself.

## Requirements
Generated using pipreqs

batch_processing==1.0.1
[czipfile==1.0.1](https://github.com/ziyuang/czipfile)
Pillow==10.4.0
pyora==0.3.11

## Notes
This script was made without any official specs on the .HSD format--even extracting the files required cracking a password. Thus, bugs are to be expected. Please report them as issues, ideally accompanied by an .HSD, and I will attempt to fix them.

Reference layers are not exported, as the reference images are not stored in the HSD. The models from 3D layers are also not stored, and are stored as flat images, the same as any other layer. The exported .ORA may be rotated or flipped incorrectly, but this is also an issue I've faced exporting image files from within HiPaint itself. While I've used batch_processing to allow for converting multiple .HSD files at once, I've found it tends to crash for memory reasons if too many files are done at the same time.

My notes on the format of .HSD files are also on the repo.
