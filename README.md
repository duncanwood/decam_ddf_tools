# decam_ddf_tools
Tools for analyzing the DECam DDF program data.

Note:

 * "object" refers to a single detection in a difference image
 * "candidate" refers to associated detections at a given sky location


## Notebooks (Complete)

### image_processing_summary.ipynb

**Description**<br>
 * Create plots to analyze the DDF's image quality parameters.
   * e.g., limiting magnitude, sky background, seeing, and zeropoint
   * how parameters are correlated with moon separation/illumination

**Inputs/Outputs**<br>
 * inputs are Rob's databases, requires password
 * outputs are plots in `image_processing_summary_figures/`

### source_detection_summary.ipynb

**Description**<br>
 * Create plots to explore object detection and R/B scores vs. image quality.
 * Identify "probably-real" candidates (>10 detections, mean R/B>0.4) and create data files just for these candidates.
 * Write two naive filters to test methods for identifying transients and fast-risers with alerts. Find they're too simple to work well.
   * 'Transients' as candidates with >=5 objects in two consecutive nights in any filter (118 candidates)
   * 'Fast Risers' as candidates with >=5 objects in the first night in any filter, all brightening (0 candidates)

**Inputs/Outputs**<br>
 * inputs are Rob's databases, requires password
 * output plots are in `source_detection_summary_figures/`
 * output files of candidate data are in `source_detection_summary_files/`
   * candidates.dat (summary statistics for "probably-real" candidates, like number of detections)
   * exposures_field.dat (data for all exposures, e.g., limiting magnitudes, seeing)
   * lightcurves_field.dat (all objects for all "probably-real" candidates)

### candidate_nightly_epochs.ipynb

**Description**<br>
 * Use the data files of "probably-real" candidates and combine intra-night detections (objects) into nightly-epoch photometry.

**Inputs/Outputs**<br>
 * inputs are the candidate data files in `source_detection_summary_files/` (no password)


## Notebooks In Progress.

candidate_summary.ipynb : classify candidates identified as interesting by source_detection_summary

explore_galactic_tables.ipynb : ?

SNsearch.ipynb : The purpose of this notebook is to search through the extragalactic field to identify candidates that are likely supernovae. This is done by selecting every candidate that has at least X detections over at least Y days, with a change of at least Z mag, and that doesn't increase in brightness more than once

SNIasearch.ipynb : Essentially the same as SNsearch.ipynb, but it requires that (after peak r brightness) rmag < gmag in an effort to identify specifically type Ia SNe.

fastrisers.ipynb : This notebook searches the extragalactic fields for candidates that (on their first night of detection) rose monotonically by at least X mag over at least Y detections in the same night.

## Modules:

decam_utils.py : Useful functions for exploring our data. Currently contains:
 * plotlc(), a function that makes a lightcurve for a chosen candidate
 * rm_dupes(), a function that removes duplicate rows from arrays
 * pull_exptimes(), a function that updates fnm_exptime.txt

## Input data files:

archive_image_list.txt : generated by MLG using NOIRLab's DataLab to access DECam images from archive; contains a bunch of metadata for DDF images (RA, Dec, MJD, filter, airmass, target, exposure time, sequence ID, image type classification, moon separation, moon illumination, estimate of sky background in counts)

<br>
<br>

## Old Stuff

### old_files

archive_image_list_2021A.txt : deprecated; use archive_image_list.txt

moondata.txt : deprecated; use the moon data in archive_image_list.txt

medpixval.txt : deprecated; use the moon data in archive_image_list.txt

fnm_exptime : A table of filenames and exposure times, generated using the decam_utils.pull_exptimes() function (which pulls from Rob's database)

### old_nbs

basics_COSMOS.ipynb : answers a few basics questions about the extragalactic field data-- how many objects and candidates were detected, how objects are distributed among candidates, and how those distributions change when an R/B cutoff is applied.

sources_per_image_COSMOS.ipynb : looks at how many "good" (R/B > 0.6) objects are found per extragalactic field image, and at how a candidate's average R/B score is correlated to the number of times it is detected.

weather_sky_moon_COSMOS.ipynb : examines correlations between sky factors (moon distance, moon illumination, seeing, and zero-point magnitude) and the number of "good" (R/B > 0.6) object detections per exposure.

RB.ipynb : This notebook looks for correlations between the legitimacy of candidates and the RB scores of their candidates