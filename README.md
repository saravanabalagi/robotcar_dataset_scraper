# Introduction

Perform sequenced downloads of various sensors logs for the Oxford Robotcar Dataset <https://robotcar-dataset.robots.ox.ac.uk/>.

Forked from https://github.com/mttgdd/RobotCarDataset-Scraper. This fork uses poetry dependency manager and adds scripts for debayering and undistoring raw images automatically after the files are downloaded. Code is tested on Ubuntu 22.04.

## Setup

- Clone and install dependencies using poetry.

```sh
git clone https://github.com/saravanabalagi/robotcar_dataset_scraper.git
cd robotcar_dataset_scraper
poetry install
```

- Add credentials and downloads path to `.env` file.

```
cp .env.example .env

# edit .env file
# update OXRO_USERNAME and OXRO_PASSWORD
# update DOWNLOADS_DIR to the directory where you want to save the downloaded files
```

## Download

`get_datasets.py` script parses the html for the complete dataset listing, scrapes each dataset page. retrieves the available files (tar) for download and saves all the information to `datasets.csv` file. Then use `scrape_mrgdatashare.py` to download the full dataset as shown below:

![note](https://img.shields.io/badge/-note%20-blue) This script only downloads and extracts the images. For debayering and undistoring, please see section below.

```sh
# download datasets.csv
# if you want to move datasets.csv
# make sure to update OXRO_DATASETS_FILE in .env
python get_datasets.py

# modify and download data listed in datasets.csv
python scrape_mrgdatashare.py
```

### Selective Download
If you do not need the full dataset, you can use `--choice_sensors` and `--choice_runs_file` options to download parts of the dataset on your own needs. 

- `--choice_sensors` option can receive multiple sensor names in `tags, stereo_centre, stereo_left, stereo_right, vo, mono_left, mono_right, mono_rear, lms_front, lms_rear, ldmrs, gps, all. `
- `--choice_runs_file` option receive a `.txt` file that contains the names of runs you want to download, we provide an example filea sample file `example_list.txt` .

for example you can download "stereo_centre", "vo" and "lms_front" data of  "2014-05-19-13-20-57" and "2014-06-26-09-31-18" by the following command:

```sh
python scrape_mrgdatashare.py \
    --choice_sensors stereo_centre,vo,lms_front \
    --choice_runs_file example_list.txt
```

### Debayer and Undistort

Download [Robotcar Camera Models](http://mrgdatashare.robots.ox.ac.uk/download/?filename=camera-models.tar) and place all the `.bin` and `.txt` files inside [camera_models](camera_models) folder.

Then simply add `debayer_undistort` option, and all images will be debayered, undistorted and saved as JPG files automatically after downloading `.tar` files. Note that this uses multiprocessing to do image processing in parallel, so adjust the number of parallel processes to spawn using `--nb_processes` option.

```sh
python scrape_mrgdatashare.py \
    --debayer_undistort \
    --nb_processes 12
```

For interactive playground for debayering and undistoring, please refer to [debayer.ipynb](debayer.ipynb)
