# BedDot Heartrate Timeseries Graph Dashboard

## Overview
This document will discuss how to create a dashboard and a timeseries graph widget to display the heartrate data collected from BedDot.

## Step 1: Dashboard Creation
In Thingsboard, navigate to the Dashboard tab, and create a new dashboard. This dashboard can contain widgets to display data and information. The name of the dashboard in this demo is BedDot Display.

## Step 2: Timeseries Graph Creation
In the dashboard, add the Timeseries Line Chart widget. This will take in timeseries data and present it in a graph. To use the widget, a data source will need to be provided. In this demo, the type of the source is Entity. The entity alias is BedDotTest. The timeseries data key is the field declared and instantiated in the script block from the rule chain, which is curr_heartrate. The graph can be customized.
