# hsd2ora
A Python script to convert HiPaint .HSD files to .ORA format. Now slightly faster than before.

## Requirements
Generated using pipreqs. The czipfile is the most important, as it is different from the one available on pip.

batch_processing==1.0.1 <br />
[czipfile==1.0.1](https://github.com/ziyuang/czipfile) <br />
Pillow==10.4.0 <br />
pyora==0.3.11 <br />
[fpng_py==0.0.3](https://github.com/K0lb3/fpng_py)

## Notes
This script was made without any official specs on the .HSD format--even extracting the files required cracking a password. Thus, bugs are to be expected. Please report them as issues, ideally accompanied by an .HSD, and I will attempt to fix them.

Reference layers are not exported, as the reference images are not stored in the HSD. The models from 3D layers are also not stored, and said layers stored as flat images. The exported .ORA may be rotated or flipped incorrectly, but this is also an issue I've faced exporting image files from within HiPaint itself.

~~While I've used batch_processing to allow for converting multiple .HSD files at once, I've found it tends to crash for memory reasons if too many files are done at the same time.~~ This issue has been greatly alleviated by modifying layer storage to use .PNG files instead of active memory.

My notes on the format of .HSD files are also on the repo.

## Current Issues
- Rotation may be incorrect (low priority)
