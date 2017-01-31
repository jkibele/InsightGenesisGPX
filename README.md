# InsightGenesisGPX
This is a simple script to "fix" gpx depth logs downloaded from InsightGenesis.

## Overview

[InsightGenesis](https://gofreemarine.com/insight-genesis/) is a service that lets you upload sonar log files from Simrad, Lowrance, and B&G depth sounders and derive some data from those logs. One of the data formats that they offer for download is a gpx representation of your depth and position. This is great for what I'm working on ([MORE-MAPS](http://more-maps.org)), but there's a problem: gpx files downloaded from InsightGenesis use a bunch of non standard gpx tags. Of concern for me, they use `depth` instead of `ele`. That means that when I try to import the gpx file into QGIS, it loses the depth information. That's no good for me. This script copies the content of the `depth` tag to the`ele` tag so I can use these gpx files as depth and position logs in GIS.

By default, this script will also convert the depths from feet to meters. However, there's an option to keep it in feet if you're so inclined. The script also checks the `depthvalid` tag and only copies the depth if it's marked true.

## Installation

    pip install insightgenesisgpx

## Use

In the terminal, type:

    
