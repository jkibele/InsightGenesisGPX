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

## More Detail Than You Probably Really Want

Here's the first part of the gpx file as downloaded from InsightGenesis:

    <gpx xmlns="http://www.topografix.com/GPX/1/1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="1.1" creator="Insight Genesis - Navico, Inc" schemaLocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd">
        <metadata>
            <name>Sonar0000.slg.gpx</name>
            <author>
                <name>Navico, Inc</name>
            </author>
            <copyright author="2016 - Navico, Inc"/>
            <link href="http://www.navico.com">
                <text>Navico, Inc</text>
            </link>
            <time>2016-10-13T03:34:08Z</time>
            <keywords>Insight Genesis, maps</keywords>
            <depthunits>feet</depthunits>
            <tempunits>C</tempunits>
            <directionunits>radians</directionunits>
            <timeunits>ms</timeunits>
            <sogunits>kn</sogunits>
            </metadata>
        <trk>
            <name>
            <![CDATA[ SK2 TRAN1-2.sl2 track ]]>
            </name>
            <desc>
            <![CDATA[ ]]>
            </desc>
            <number>1</number>
            <trkseg>
                <trkpt lat="-36.2660838100883" lon="174.803030013204">
                    <sym>Waypoint</sym>
                    <time>2016-10-12T14:07:38Z</time>
                    <channel>Primary</channel>
                    <frequency>200kHz</frequency>
                    <upperlimit>0</upperlimit>
                    <lowerlimit>164</lowerlimit>
                    <depthvalid>T</depthvalid>
                    <depth>59.339</depth>
                    <watertempvalid>T</watertempvalid>
                    <watertemp>16.25</watertemp>
                    <waterspeedvalid>F</waterspeedvalid>
                    <waterspeed>0</waterspeed>
                    <positionvalid>T</positionvalid>
                    <timeoffset>19</timeoffset>
                    <speedvalid>F</speedvalid>
                    <speed>0</speed>
                    <trackvalid>T</trackvalid>
                    <track>2.617994</track>
                    <altitudevalid>F</altitudevalid>
                    <altitude>0</altitude>
                    <heading>0</heading>
                </trkpt>
                <trkpt lat="-36.2660838100883" lon="174.803030013204">
                    <sym>Waypoint</sym>
                    <time>2016-10-12T14:07:38Z</time>
                    <channel>Primary</channel>
                    <frequency>200kHz</frequency>
                    <upperlimit>0</upperlimit>
                    <lowerlimit>164</lowerlimit>
                    <depthvalid>T</depthvalid>
                    <depth>59.339</depth>
                    <watertempvalid>T</watertempvalid>
                    <watertemp>16.23001</watertemp>
                    <waterspeedvalid>F</waterspeedvalid>
                    <waterspeed>0</waterspeed>
                    <positionvalid>T</positionvalid>
                    <timeoffset>109</timeoffset>
                    <speedvalid>F</speedvalid>
                    <speed>0</speed>
                    <trackvalid>T</trackvalid>
                    <track>2.617994</track>
                    <altitudevalid>F</altitudevalid>
                    <altitude>0</altitude>
                    <heading>0</heading>
                </trkpt>
                ...etc, etc, etc...

Notice that the `ele` tags are missing from the track points (`trkpt`). Here's the same gpx as this script writes it out:

    <gpx xmlns="http://www.topografix.com/GPX/1/1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" version="1.1" creator="Insight Genesis - Navico, Inc" schemaLocation="http://www.topografix.com/GPX/1/1 http://www.topografix.com/GPX/1/1/gpx.xsd">
      <metadata>
        <name>Sonar0000.slg.gpx</name>
        <author>
          <name>Navico, Inc</name>
        </author>
        <copyright author="2016 - Navico, Inc"/>
        <link href="http://www.navico.com">
          <text>Navico, Inc</text>
        </link>
        <time>2016-10-13T03:34:08Z</time>
        <keywords>Insight Genesis, maps</keywords>
        <depthunits>feet</depthunits>
        <tempunits>C</tempunits>
        <directionunits>radians</directionunits>
        <timeunits>ms</timeunits>
        <sogunits>kn</sogunits>
      </metadata>
      <trk>
        <name>SK2 TRAN1-2.sl2 track</name>
        <desc></desc>
        <number>1</number>
        <trkseg>
          <trkpt lat="-36.2660838100883" lon="174.803030013204">
            <sym>Waypoint</sym>
            <time>2016-10-12T14:07:38Z</time>
            <channel>Primary</channel>
            <frequency>200kHz</frequency>
            <upperlimit>0</upperlimit>
            <lowerlimit>164</lowerlimit>
            <depthvalid>T</depthvalid>
            <depth>59.339</depth>
            <watertempvalid>T</watertempvalid>
            <watertemp>16.25</watertemp>
            <waterspeedvalid>F</waterspeedvalid>
            <waterspeed>0</waterspeed>
            <positionvalid>T</positionvalid>
            <timeoffset>19</timeoffset>
            <speedvalid>F</speedvalid>
            <speed>0</speed>
            <trackvalid>T</trackvalid>
            <track>2.617994</track>
            <altitudevalid>F</altitudevalid>
            <altitude>0</altitude>
            <heading>0</heading>
            <ele>-18.0867471348</ele>
          </trkpt>
          <trkpt lat="-36.2660838100883" lon="174.803030013204">
            <sym>Waypoint</sym>
            <time>2016-10-12T14:07:38Z</time>
            <channel>Primary</channel>
            <frequency>200kHz</frequency>
            <upperlimit>0</upperlimit>
            <lowerlimit>164</lowerlimit>
            <depthvalid>
            ...and so on...
            
Now the depth has been converted to meters and copied to the `ele` tag. Now the gpx file can be used with standard GIS software that expects depth (or elevation) to be in the `ele` tag.
