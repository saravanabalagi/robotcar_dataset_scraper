# Introduction

Perform sequenced downloads of various sensors logs for the Oxford Robotcar Dataset <https://robotcar-dataset.robots.ox.ac.uk/>.

The code is tested for Python3 on Ubuntu 16.04.

## Setup

```sh
git clone https://github.com/saravanabalagi/robotcar_dataset_scraper.git
cd robotcar_dataset_scraper
poetry install
```

## Download

`get_datasets.py` script parses the html for the complete dataset listing, scrapes each dataset page. retrieves the available files (tar) for download and saves all the information to `datasets.csv` file. Then use `scrape_mrgdatashare.py` to download the full dataset as shown below:

```sh
# download datasets.csv
python get_datasets.py

# modify and download data listed in datasets.csv
python scrape_mrgdatashare.py \
    --downloads_dir downloads \
    --datasets_file datasets.csv \
    --username USERNAME \
    --password PASSWORD
```

If you do not need the full dataset, you can use `--choice_sensors` and `--choice_runs_file` options to download parts of the dataset on your own needs. 

- `--choice_sensors` option can receive multiple sensor names in `tags, stereo_centre, stereo_left, stereo_right, vo, mono_left, mono_right, mono_rear, lms_front, lms_rear, ldmrs, gps, all. `
- `--choice_runs_file` option receive a `.txt` file that contains the names of runs you want to download, we provide an example filea sample file `example_list.txt` .

for example you can download "stereo_centre", "vo" and "lms_front" data of  "2014-05-19-13-20-57" and "2014-06-26-09-31-18" by the following command:

```sh
python scrape_mrgdatashare.py \
    --choice_sensors stereo_centre,vo,lms_front \
    --choice_runs_file example_list.txt \
    --downloads_dir downloads \
    --datasets_file datasets.csv \
    --username USERNAME \
    --password PASSWORD
```

