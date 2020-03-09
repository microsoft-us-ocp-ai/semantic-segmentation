# Semantic segmentation

This repo contains notebooks for pixel-wise segmentation on images to localize a region of interest (ROI) using U-NET. The initial input data format is Photoshop files, where there is a layer for the original image and a layer for the ROI mask.

Note: You will need a .env file at the root of your project with appropriate environment variables for the filepaths used throughout this repo. 

## Load data

We first load in the data from .psd files to extract each layer, compose our masks, and save the resulting image and mask pairs into an output location as numPy arrays. Here, we have labeled ROI pixels as 1s and all other pixels as 0s in our masks.  The output directory structure is 2 parallel directories, one that contains the images, and one that contains the masks, where the image and mask both have the same filename in their respective directories.  If your data does not come from .psd files, you can sustitute your own logic for creating these parallel folders for your images and masks. This structure will allow you to proceed to slicing images.

Note: You will need to modify the [constants](https://github.com/laurentran/segmentation/blob/master/src/utils/constant.py) with the name of your layer that represents the image mask. 

* Load images from photoshop files and save images/masks as numPy arrays: [load-data](https://github.com/laurentran/segmentation/blob/master/notebooks/load-data.ipynb)

## Slice and prepare images

Next, with large images (in our case 6000x4000 pixels), we need to slice the images into smaller patches to train the network.  We similarly save the sliced images and masks into two folders, where the image and its associated mask have the same filename under two parallel folders.  We found that 128x128 patches worked well in our case.

* Slice images: [slice-images](https://github.com/laurentran/segmentation/blob/master/notebooks/slice-images.ipynb)

[OPTIONAL] Where there is high class imbalance (in cases where there are many more 0s than 1s, or in other words, where the ROI you wish to localize has a small proportion of pixels to background), it is effective to filter out a subset of image patches where there is no signal (no 1s). 

* Filter samples: [filter-samples](https://github.com/laurentran/segmentation/blob/master/notebooks/filter-samples.ipynb)

## Train segmentation network

With our sliced images and masks, we are ready to train our U-NET.  This notebook contains a vanilla implementation of U-NET as well as a custom Keras generator to read in the images into batches.

* Train network: [unet-segmentation](https://github.com/laurentran/segmentation/blob/master/notebooks/unet-segmentation.ipynb)

## Score and visualize results

With a trained and saved model, next we score and visualize the results.  To best visualize results, we stitch the scored smaller image patches together to reconstruct the full image, which we visualize next to the original image.

* Score, stitch, and visualize results: [score-and-visualize](https://github.com/laurentran/segmentation/blob/master/notebooks/score_and_visualize.ipynb)
