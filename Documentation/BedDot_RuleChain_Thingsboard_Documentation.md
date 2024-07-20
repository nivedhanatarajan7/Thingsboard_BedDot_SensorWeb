# Thingsboard Heartrate BedDot API Rule Chain

## Overview
This document will discuss how to set up a rule chain in Thingsboard and retrieve real-time heartrate data from the BedDot API. This data can be represented in a time series graph in a Thingsboard dashboard. This process is based on the Thingsboard: Community Edition platform.

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
