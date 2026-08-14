# cricket-objects-detection
Machine Learning Project to identify bats, balls, stumps in cricket images.

# Installing the project

1. Get `uv` from the website - https://docs.astral.sh/uv/getting-started/installation/.

2. Run `uv sync` to setup your virtual env at .venv.

3. Activate the virtual environment and run the desired notebooks. See below for the order in which to execute them.

# Data Pipeline

## 1. Initial Input 

The initial input is raw, unresized image files downloaded from the internet and saved in 'data/final_data/raw_images'. The format is mixed - most are .webp but some are .jpg as well.

## 2. Data Preprocessing

Code - `Preprocessing.ipynb`

Input - Raw Images downloaded from the internet
Output - Images resized to 800 x 600 and saved to 'data/final_data/resized_images' + individual grid cell images saved to disk in subfolders within 'data/final_data/grid_cells'.

## 3. Data Labelling

Input - Resized Images
Output - CSV with each grid cell of each image labelled as no_object/ball/bat/stump.

All labelling done by eyeballing each grid cell. We have used 2 utilities to assist in the labelling task - 1)LabelMe Open source tool and 2) Custom-built Python GUI utility

## 4. Data Prepping

Input - 