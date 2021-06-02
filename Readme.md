#  The Development of COVID-19 Incidences in Germany by County
The main aim of this project is to produce a series of maps to illustrate the spread of COVID-19 in Germany from March 1, 2020 to the current date (for as long as the API provides the data).
<br/><br/>

## Result
A series of color-coded maps with the seven days incidence is saved as video under the name ["germany_incidence_V4_300ms_1080p_music.mp4"](media/germany_incidence_V4_300ms_1080p_music.mp4) in the folder ["media/"](media/).
The color of a county represents its seven days incidence as defined by the legend.
<br/><br/>
A series of histograms on the right side shows the distribution of the seven days incidences of German counties.
One given histogram contains 15 bars. Each bar represents an incidence range of 25, except for the last bar which represents the remaining incidences from 350 upwards.
The length of the bars represents the number of counties with an incidence within the given range.
<br/><br/>
This and the other videos feature the song ["Algorithms" from Chad Crouch](https://freemusicarchive.org/music/Chad_Crouch/Arps/Algorithms) and are created by using the [Windows Video Editor](https://support.microsoft.com/en-us/windows/create-films-with-video-editor-94e651f8-a5be-ae03-3c50-e49f013d47f6).
<br/><br/>

## Files
This project contains three files:
- ["plot_data.ipynb"](plot_data.ipynb)
- ["get_data.ipynb"](get_data.ipynb)
- ["get_geographical_data_of_german_counties.ipynb"](get_geographical_data_of_german_counties.ipynb)

The files have a hierarchical structure: The file "plot_data.ipynb" executes the file "get_data.ipynb" which in turn executes the file "get_geographical_data_of_german_counties.ipynb".
<br/><br/>
The file ["plot_data.ipynb"](plot_data.ipynb) plots the data and saves the plots that are part of a time series in the [media-folder](media/).
<br/><br/>
The file ["get_data.ipynb"](get_data.ipynb) converts "unpolished" data to "polished" data.
"Unpolished" data is raw data pulled directly from an API or from its backup.
Data that has been checked and where superfluous dictionary shells and unnecessary data (e.g. object IDs) have been discarded is called "polished data".<br/>
The unpolished data is saved as "backup of the API" in the folder ["unpolished_data"](unpolished_data). The polished data is saved in the folder ["polished_data"](polished_data).
<br/><br/>
The file ["get_geographical_data_of_german_counties.ipynb"](get_geographical_data_of_german_counties.ipynb) is an outsourced part of ["get_data.ipynb"](get_data.ipynb) that contains the data collection of the counties' shapes.
This data collection is disproportionately large because the shape of approximately 100 counties needs to be checked individually.
<br/><br/>

## Data
### Format and Content of the Data
There are three different dictionaries holding all the data:
- covid19
- counties_geography
- non_county_specific_data
<br/>
<br/>

**covid19**
<br/>
The dictionary "covid19" stores the accumulated number of cases per day as a list (reachable with the key "cases") and the seven days incidence per day as another list (reachable with the key "incidences"). Both lists hold exactly as many values as there are days from March 1, 2020 to the current date (for as long as the API provides the data), with each value representing the given data in the county on that day.
<br/>
<br/>
The list of number of cases is taken from the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/).
By contrast, the seven days incidences of a county are calculated by multiplying every county's number of COVID-19 cases by 100,000 and dividing that number by the county's number of inhabitants.
<br/>
<br/>

**counties_geography**
<br/>
The dictionary "counties_geography" contains the following keys and provides the following kind of data (all packaged in a dictionary reachable through the AdmUnitID of a county):
   - name: Name of the county
   - population: Number of inhabitants from the last official estimate (varies, therefore check out the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/) for more information). These numbers are also used in the official incidence calculations.
   - area_in_m2: Area of the county in square meters, cannot be calculated from the polygons stored in geometry.
   - geometry: The shape of the county stored as one or more polygons. They also determine the position of the county on the map.
   - raw_geometry: The original version of the geometry used to draw the outlines of the counties.
   - population_density: The area_in_m2 divided by the population multiplied by 100,000.
<br/>

All the data is taken from the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/) except for the data on population density, which is calculated as described.
<br/>
<br/>

**non_county_specific_data**
<br/>
The dictionary "non_county_specific_data" has the following keys and provides the following kind of data:
<br/>
- unixtime: The time as provided by the API: The number of milliseconds that have elapsed since the Unix epoch. This indicates the days represented in the data.
- states: names and numbers of the German federal states. The first one or two numbers of the "AdmUnitID" (Gemeindeschlüssel) represent the number of the federal state.
- highest_case_number: highest number of COVID-19 cases in any county
- lowest_case_number: lowest number of COVID-19 cases in any county
- highest_incidence: highest seven days incidence in any county
- lowest_incidence: lowest seven days incidence in any county
- UTC: The Unix time converted to the Coordinated Universal Time and the format DD.MM.YYYY.
- UTC+7days: The list of UTC dates plus the seven days before the first date provided by the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/). This item was added in order to be able to display the first seven time frames of the seven days incidence in the plot.
<br/>

The Unix times and the names and numbers of the German federal states are taken from the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/). All the other data is collected or calculated as described.
<br/>
<br/>
<br/>
The dictionaries "covid19" and "counties_geography" both hold information about every individual county: First the "AdmUnitID" (Gemeindeschlüssel) of a county must be used to access the COVID-19 data of that county.
<br/>
In the code example below, the AdmUnitID of Kiel ("1002") was chosen for demonstration purposes.
<br/><br/>
**NB: The AdmUnitID is a string, not a number, because storing and restoring it with the json-format converts all keys of dictionaries to strings!**
<br/><br/>

Code example:
```
Input:    list(covid19["1002"].keys())
Output:   ['cases', 'incidences']

Input:    list(counties_geography["1002"].keys())
Output:   ['name',
           'population',
           'area_in_m2',
           'geometry',
           'raw_geometry',
           'population_density']

Input:    list(non_county_specific_data.keys())
Output:   ['unixtime',
           'states',
           'highest_case_number',
           'lowest_case_number',
           'highest_incidence',
           'lowest_incidence',
           'UTC',
           'UTC+7days']
```
<br/>

### Data Sources - Retrieval and Storage
The project uses COVID-19 and geographical data of 412 German counties. Both data are retrieved from the ["COVID-19 Datenhub"](https://npgeo-corona-npgeo-de.hub.arcgis.com/). The datahub was chosen as a source because it is referenced by the [Robert-Koch-Institut (RKI)](www.rki.de) and the [German government](https://www.bundesregierung.de/breg-de).
<br/><br/>
There are three different ways the COVID-19 data and the geographical data are stored:
- Stored on the server of the RKI, reachable through the API.
- Stored unpolished and unmodified on this machine as backup if the API does not work anymore.
- Stored polished and ready to go on this machine.
<br/><br/>

The user has to choose one of the three sources of stored data both for the COVID-19 number of cases and for the geographical information of the counties. This is done in the "Controls" chapter of the two files handling the data collection: ["get_data.ipynb"](get_data.ipynb) and ["get_geographical_data_of_german_counties.ipynb"](get_geographical_data_of_german_counties.ipynb).
<br/><br/>