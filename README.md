# Displaying Heartrate Data Using Thingsboard

This repository contains the JSON files and documentation for the Thingsboard BedDot heartrate display dashboard demo.

# Thingsboard Heartrate BedDot API Rule Chain

## Overview
This section will discuss how to set up a rule chain in Thingsboard and retrieve real-time heartrate data from the BedDot API. This data can be represented in a time series graph in a Thingsboard dashboard. This process is based on the Thingsboard: Community Edition platform.

## Step 1: Customer Creation
In Thingsboard, create a customer by navigating to the Customers tab and adding a customer. In this demo, the name of the customer is TestCustomer.

## Step 2: Asset Creation
In Thingsboard, create an asset by navigating to the Assets tab and adding an asset profile. In this demo, the asset name is BedDotTest and its asset profile is BedDotTest.

After creating the asset, assign to a customer.

## Step 3: Rule Chain Creation


### Generator
In the generator block, add the body of the BedDot API request. Since this demo will show how to retrieve real-time heartrate data from the BedDot API, the msg will contain the MAC address of the device and the vital sign name of interest.

```
var msg = {
    "mac": "30:30:f9:72:3b:b0",
    "vital_name": "heartrate",
};

return {
    msg: msg,
    metadata: {},
    msgType: "POST_TELEMETRY_REQUEST"
};
```

### Originator Attributes
Within the Originator Attributes block, add mac and vital_name as Server Attributes. These are the two fields within the body for the BedDot API call.

### Rest API Call
To call the BedDot API, use the Rest API Call block. The Endpoint URL pattern is https://homedots.us:3425/readVital. The request method is POST. In the Headers section, add 

```
{
    Content-Type: “application/json”,
    Authorization: 12345678
}
```

### Script
To retrieve the data through Thingsboard to use, the Script block will help map the API call result to attribute. The result from the BedDot API call will contain a result field. Store the returning value as curr_heartrate.

```
var newMsg = {
    "curr_heartrate": msg.result[0][0]
};

return {msg: newMsg, metadata: metadata, msgType: msgType};
```

### Timeseries
Add a Save Timeseries block to save the resulting data as a timeseries.

# BedDot Heartrate Timeseries Graph Dashboard

## Overview
This section will discuss how to create a dashboard and a timeseries graph widget to display the heartrate data collected from BedDot.

## Step 1: Dashboard Creation
In Thingsboard, navigate to the Dashboard tab, and create a new dashboard. This dashboard can contain widgets to display data and information. The name of the dashboard in this demo is BedDot Display.

## Step 2: Timeseries Graph Creation
In the dashboard, add the Timeseries Line Chart widget. This will take in timeseries data and present it in a graph. To use the widget, a data source will need to be provided. In this demo, the type of the source is Entity. The entity alias is BedDotTest. The timeseries data key is the field declared and instantiated in the script block from the rule chain, which is curr_heartrate. The graph can be customized.

# Viewing the Heartrate Dashboard

## Overview
This section will dicuss how to view the demo Thingsboard BedDot heartrate display dashboard.

## Login and View Dashboard
To view the Thingsboards dashboard, navigate to https://demo.thingsboard.io/. Using the username and password for the account, sign into Thingsboard, and the dashboard will be visible in the Home section of the Thingsboard platform.
