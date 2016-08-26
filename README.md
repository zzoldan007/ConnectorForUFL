# PI-Connector-for-UFL-Samples – JSON

This sample will show how to use the PI Connector for UFL to parse data in the form of the North American General Bike Feed Specification (GBFS). It will also show how the Connector can use information from the data file to create a PI AF element hierarchy. 

# Contents 
- A Python script which is designed to get the publicly available data and send it to the Connector’s REST endpoint. 
- A sample INI file which shows how to parse the JSON file
 
# Getting Started
You will need the PI Connector for UFL and a PI System (Including PI Data archive and PI AF). It is possible to continue without the use of PI AF, just disregard the parts of the INI which create PI AF elements. The INI file is used to parse public bike share data from San Francisco’s bike sharing system, “Bay Area Bike Share”. The JSON is located here: http://feeds.bayareabikeshare.com/stations/stations.json

#Requirements
- PI Connector for UFL – Version 1.0.0.41
- Python – Version 3.5.2
- Requests Python Module – 2.9.1

#Tutorial on how to use these scripts
- Open the PI Connector for UFL admin page (https://{servername}:{port}/admin/ui/)
-	Specify a PI Data Archive and create a new data source
-	Upload SF_BikeShare.ini as the configuration file, put the username/password as (pi,pi) 
-	Make the following choices:
-	Select REST as a data type
-	Leave new line as blank
- Select LOCAL for incoming timestamps 
-	Run the command below to send the data:

`Python putJSONdata_SF_Bikes.py https://{servername}:{port}/connectordata/SF_Live_Bikes`
- You can now see the PI AF elements which have been created by the Connector in your PI AF database (if applicable). You can also see two PI Points created for each bike station (UFL.{StationID}.AvailDocks and UFL.{StationID}.AvailBikes)
 
# Notes the Parsing of JSON data
The original JSON data file arrives unformatted and it is not straightforward for the Connector to parse it. We have several JSON elements, or bike stations, on one continuous line. 
`{"executionTime":"2016-08-26 06:07:17 AM","stationBeanList":[{"id":2,"stationName":"San Jose Diridon Caltrain Station","availableDocks":6,"totalDocks":27,"latitude":37.329732,"longitude":-121.901782,"statusValue":"In Service","statusKey":1,"availableBikes":20,"stAddress1":"San Jose Diridon Caltrain Station","stAddress2":"","city":"San Jose","postalCode":"`

Using the Pretty Print Method, we can split this up into a much more usable form:

```
{
            "altitude": "",
            "availableBikes": 20,
            "availableDocks": 11,
            "city": "San Francisco",
            "id": 90,
            "is_renting": true,
            "landMark": "San Francisco",
            "lastCommunicationTime": null,
            "latitude": 37.780148,
            "location": "",
            "longitude": -122.403158,
            "postalCode": "",
            "renting": true,
            "stAddress1": "5th St at Folsom St",
            "stAddress2": "",
            "stationName": "5th St at Folsom St",
            "statusKey": 1,
            "statusValue": "In Service",
            "testStation": false,
            "totalDocks": 31
},
```

# Automation
This data is updated every 10-20 minutes, so you may want to run the script that often. In order to automate password requests, the username/password were hardcoded in the script. We can call it repetitively using Windows Task Scheduler. In this case, we are launching the script in a BAT file to include all necessary arguments. 

# Expansion
As mentioned in the header, many bike sharing systems use the same or very similarly formatted files for their open data. Using the [GBFS Specification](https://github.com/NABSA/gbfs) we can add in data from other bike sharing systems such as Montreal's, New York City's, Toronto's, Philadelhia's and Boston's systems. We can then just add the calls to those Python scripts in the BAT file, to be deployed whenever we want. 

# PI Square
You can discuss this project on PI Square, associated with [this PI Square blog post.](https://pisquare.osisoft.com/community/all-things-pi/pi-interfaces/blog/2016/08/19/building-a-pi-af-hierarchy-using-the-pi-connector-for-ufl)

# Licensing 
Copyright 2016 OSIsoft, LLC.
Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at
   http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

Please see the file named LICENSE.md.
