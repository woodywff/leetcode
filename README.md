# code_daily
To implement the demo for single modality requirement from Prof.Biswal, I deleted t2.nii.gz t1ce.nii.gz and flair.nii.gz and copy t1.nii.gz to be t2 t1ce and flair.
```
import os
import glob
import shutil
from tqdm import *

mods = ['t1ce','t2','flair']
print('deleting...')
for mod in mods:
    for file in tqdm(glob.glob(os.path.abspath('../data/preprocessed_val_data/*/*/'+mod+'*'))):
        if os.path.exists(file):
            os.remove(file)
        else:
            print(file)
print('copying...')
for t1_file in tqdm(glob.glob(os.path.abspath('../data/preprocessed_val_data/*/*/t1*'))):
    for mod in mods:
        shutil.copy(t1_file,'/'.join(t1_file.split('/')[:-1])+'/'+mod+'.nii.gz')
print('Done')
```
