# ai_utilities

A set of scripts useful in deep learning and AI purposes, originally for use with `fast.ai` lectures and libraries.

## image_download.py
Download images (typically limited to 1000) from a specified serach engine, currently Google or Bing.
image_download.py is useful in several respects:
- Because is utilizes selenium, it is not limited by the search engine api and generally allows for more downloaded images.
- It can operate in `headless` mode, which means it can be used on a server without access to a gui browser.
- The default browser is Firefox. The script can be modified to use other browsers such as Chrome.

```
usage: image_download.py [-h] [--gui] [--engine {google,bing}]
                         searchtext num_images

Select, search, download and save a specified number images using a choice of
search engines

positional arguments:
  searchtext            Search Image
  num_images            Number of Images

optional arguments:
  -h, --help            show this help message and exit
  --gui, -g             Use Browser in the GUI
  --engine {google,bing}, -e {google,bing}
                        Search Engine, default=google
```

Example: `image_download.py 'dog' 200 --engine 'bing' --gui` 

Notes:
1) Requires `Python >= 3`
2) Install selenium: `conda install selenium`  or  `pip install selenium`
3) Install other dependencies from conda
3) Install an appropriate browser and browser driver (appropriate for your browser and operating system) in PATH.
4) For example, if using Ubuntu and Firefox:
- `tar xfvz geckodriver-v0.19.1-linux64.tar.gz` 
- `mv geckodriver ~/bin/`, where `~/bin` is a dir in PATH

## make_train_valid.py
```
usage: make_train_valid.py [-h] [--train TRAIN] [--valid VALID] [--test TEST]
                           labels_dir

Make a train-valid directory and randomly copy files from labels_dir to sub-
directories

positional arguments:
  labels_dir     Contains at least two directories of labels, each containing
                 files of that label

optional arguments:
  -h, --help     show this help message and exit
  --train TRAIN  files for training, default=.8
  --valid VALID  files for validation, default=.2
  --test TEST    files for training, default=.0
```

For example, given a directory:
```
catsdogs/
         ..cat/[*.jpg]
         ..dog/[*.jpg]
``` 
```
make_train_valid.py catsdogs --train .75 --valid .25
```
Creates the following directory structure:
```
catsdogs/
         ..cat/[*.jpg]
         ..dog/[*.jpg]
         ..train/
                 ..cat/[*.jpg]
                 ..dog/[*.jpg]
         ..valid/
                 ..cat/[*.jpg]
                 ..dog/[*.jpg]
```

## filter_img.sh
Use `file` to determine the type of picture then filter (keep) only pictures of a specified type.

Images are filtered in place, i.e., non-JPEG files are deleted. (This can be modified within the script.)
```
Usage: filter_img.sh image_directory
```

Example:`filter_image.sh dogs/`

## Sample workflow
```
image_download.py 'bmw' 500 --engine 'bing' --gui
image_download.py 'cadillac' 500 --engine 'google'
mv dataset cars
filter_img.sh cars/bmw
filter_img.sh cars/cadillac
make_train_valid.py cars --train .75 --valid .25
```

## ai_utils.py

```
ai_utils.py
contains:
atttributes_of(obj, *exclude): -> prints obj attributes
methods_of(obj,lr=False):      -> prints obj methods

usage: import ai_utils

> data = ImageClassifierData.from_paths(PATH, tfms=tfms_from_model(arch, sz))
> attributes_of(data.trn_dl.dataset,'fnames')
c: 2
fnames: ...
is_multi: False
is_reg: False
n: 23000
path: data/dogscats/
sz: 224
y: [0 0 0 ... 1 1 1]

> methods_of(data.trn_dl.dataset)
denorm(arr):
get(tfm, x, y):
get_c():
get_n():
get_sz():
get_x(i):
get_y(i):
resize_imgs(targ, new_path):
transform(im, y=None):
```
