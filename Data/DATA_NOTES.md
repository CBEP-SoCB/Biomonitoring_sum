<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

# Details of Data Files
This repository continues analysis begun in the GIT Archive
'Access_biomonitoring'.  That archive describes how DEP data was accessed 
from on-line resources, using a series of Python scripts. In this archive
we use R to conduct statistical analysis. Most graphics for this data source are
maps, and were developed in ArcGIS.

# Contents
*   `Biomonitoring_Stations_CB.csv` -- list of all biomonitoring Stations in the
    Casco Bay watershed. These data provide geospatial and other information
    on sample locations.

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
FID         | Arbitrary ID number from GIS                  |
FID_1       | Arbitrary ID number from GIS                  |
Station_Nu  | Station code number used in DEP records       | "S-###", for stream, and "W-###" for wetland stations
Station     | Name and sometimes description of station     | Text
Town        | Township where station is located             | Text
County      | County                                        | Text
Major_Drai  | Major drainage, apparently at HUC 8 level     | Text
Site_Type   | "WETLAND" or "STREAM/RIVER BIOMONITORING"     |  
Sample_Typ  | "MACROINVERTEBRATE", "ALGAE" or "WETLAND"     |  
Latitude    | Latitude,  WGS 1984, as required for KLM data | Decimal number
Longitude   | Longitude. WGS 1984, as required for KLM data | Decimal number
PctImperv   | Imperviousness in the local catchment         | Percent
------------|-----------------------------------------------|--------------

*   `Biomonitoring_Samples_CB.csv` -- list of SAMPLES associated with those 
    biomonitoring Stations, including invertebrate, algae, and wetland
    monitoring. The primary data we work with here are the letter designations
    of water quality class assigned based on invertebrate samples.  Those
    classes are derived from a big biological integrity-index style
    classification model developed by Maine DEP.

    The key data from this data set is `Final Determination`, which contains
    results of the water quality classification model.  "A" stands for waters
    with a biological community consistent with "Class A" and Class "AA" waters,
    the categories for Maine's highest quality waters.  Similarly, "B" and "C"
    indicate samples consistent with Maine's "Class B" and "Class C" waters,
    which can have waters somewhat more impacted by pollution and other
    stressors.  "NA"" is used to signal "non attainment".  In this setting, it
    is NOT a signal for missing data. Sine NA is used by R as the default way of
    showing missing data, this may require special handling during loading of
    data in R. "I" stands for "Indeterminate", which signals a sample that
    could not be reliably classified into one of the other four categories.

Column Name         | Contents                                 | Units / Values
--------------------|------------------------------------------|--------------
Station Number      | Station code number used in DEP records  | "S-###", for stream, and "W-###" for wetland stations
Sample Type         | "MACROINVERTEBRATE", "ALGAE" or "WETLAND"| 
Sample ID           | Cod, usually numeric for sample          | 
Sample Date         | Date                                     | "%m/%d/%Y"
Statutory Class     | Statutory stream classification          | "AA", A", "B", "C"
Attained Class      | Did the sample meet biomonitoring criteria? | "Yes", "No", "Indeterminate" 
Report              | Originally a link to the report, here, just the word "Report" | 
Final Determination | Result of Maine DEP classification model | "A", "B", "C", "NA", or "I"
--------------------|------------------------------------------|--------------

    

*   `Recent_Stream_Biomonitoring.csv` Selection of the most recent invertebrate
    biomonitoring results for sites sampled within the last ten years at least 
    once.

    This file is a subset of the previous data file, where we have selected ONLY
    invertebrate biomonitoring samples, and ONLY the most recent samples
    collected from each site within the ten year period 2009 through 2018,
    inclusive.

Column Name    | Contents                                 | Units / Values
---------------|------------------------------------------|--------------
Station        | Station code number used in DEP records  | "S-###", usually with two or three digits 
Type           | "MACROINVERTEBRATE", "ALGAE" or "WETLAND"| 
Date           | Date                                     | "%m/%d/%Y" 
Statutory      | Statutory stream classification          | "AA", A", "B", "C" 
Final          | Result of Maine DEP classification model | "A", "B", "C", "NA", or "I" 
Attained       | Did the sample meet biomonitoring criteria?| "Yes", "No", "Indeterminate" 
Year           | Year of Sample (derived from Date)       | 
Final_f        | Repeat of "Final"                        | 
local_imperv   | Proportion impervious cover in the local NHD+ V2 catchment.| 
---------------|------------------------------------------|--------------

*   `Recent_Stream_Biomonitoring_clean.csv`  A version of the former file with
    column names and column order changed.
Column Name | Contents                                | Units / Values
------------|-----------------------------------------|--------------
Station     | Station code number used in DEP records | "S-###", usually with two or three digits 
Samp_Date   |  Date                                   | "%m/%d/%Y" 
Samp_Year   | Year of Sample (derived from Date)      | 
Statutory   | Statutory stream classification         | "AA", A", "B", "C" 
Final       |   Result of Maine DEP classification model | "A", "B", "C", "NA", or "I" 
Attained    | Did the sample meet biomonitoring criteria?| "Yes", "No", "Indeterminate" 
------------|-----------------------------------------|--------------

*   `Recent_Stream_Biomonitoring_final` (Shapefile). The shapefile was
    generated by joining the `Recent_Stream_Biomonitoring_clean.csv` file with 
    geospatial data and removing unnecessary data columns. Attributes include 
    the following:

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
Station_Nu  | Station code number used in DEP records       | "S-###", for stream, and "W-###" for wetland stations
Station     | Name and sometimes description of station     | Text 
Town        | Township where station is located             | Text 
County      | County                                        | Text 
Latitude    | Latitude,  WGS 1984, as required for KLM data | Decimal number
Longitude   | Longitude. WGS 1984, as required for KLM data | Decimal number
PctImperv   | Imperviousness in the local catchment         | Percent
Samp_Date   |  Date                                         | "%m/%d/%Y" 
Samp_Year   | Year of Sample (derived from Date)            | 
Statutory   | Statutory stream classification               | "AA", A", "B", "C" 
Final       |   Result of Maine DEP classification model    | "A", "B", "C", "NA", or "I" 
Attained    | Did the sample meet biomonitoring criteria?   | "Yes", "No", "Indeterminate" 
------------|-----------------------------------------------|--------------

