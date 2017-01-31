# InsightGenesisGPX
This is a simple script to "fix" gpx depth logs downloaded from InsightGenesis.

## Overview

[InsightGenesis](https://gofreemarine.com/insight-genesis/) is a service that lets you upload sonar log files from Simrad, Lowrance, and B&G depth sounders and derive some data from those logs. One of the data formats that they offer for download is a gpx representation of your depth and position. This is great for what I'm working on ([MORE-MAPS](http://more-maps.org)), but there's a problem: gpx files downloaded from InsightGenesis use a bunch of non standard gpx tags. Of concern for me, they use `depth` instead of `ele`. That means that when I try to import the gpx file into QGIS, it loses the depth information. That's no good for me. This script copies the content of the `depth` tag to the`ele` tag so I can use these gpx files as depth and position logs in GIS.

By default, this script will also convert the depths from feet to meters. However, there's an option to keep it in feet if you're so inclined. The script also checks the `depthvalid` tag and only copies the depth if it's marked true.

## Installation

    pip install insightgenesisgpx

## Use

In the terminal, type:

    insightgenesisgpx.py infile outfile
    
Where `infile` is the path to the gpx file you'd like to modify and `outfile` is the path to where you'd like to save the results. If to view the help, type this in the terminal:

    insightgenesisgpx.py -h
    
This will print the following:

    usage: insightgenesisgpx.py [-h] [--metric | --no-metric] infile outfile

    Copy the depth from the `depth` tag to the standard gpx `ele` tag. This will
    allow the gpx to be imported into GIS programs while retaining the depth
    information.

    positional arguments:
      infile       The file that you would like to fix.
      outfile      The corrected output file.

    optional arguments:
      -h, --help   show this help message and exit
      --metric     Output depths in meters rather than feet. Metric output is the
                   default because: science.
      --no-metric  Do not convert depths to meters.


    
