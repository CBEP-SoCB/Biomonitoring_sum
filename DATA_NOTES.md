<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

#  KML Source Files
The `KML` file here `lawb_biomonitoring_station_2019.kml`, was extracted from
a KMZ file by converting the 'kmz' extension to `zip`, and unzipping in Windows. 
We discarded the associated graphic symbols, which we do not need. See below for 
the format of the data contained in this `KML` file.

# List of Output Files
The key output files are the following:
*  `Biomonitoring_Stations_CB.csv`
*  `Biomonitoring_Samples_CB.csv`
*  `Most_Recent_Samples_CB.csv`

# Summary of Data preparation Steps
1.  Identify Source KML file (`lawb_biomonitoring_station_2019.kml`).

2.  Run  `DataParserStation.py`, to generate statewide CSV files,
    `Biomonitoring_Samples_Parsed.csv` and `Biomonitoring_Stations_Parsed.csv`
    (not shown here).
    
3.  In GIS, select stations found in the Casco Bay watershed, saved as 
    Biomonitoring_Stations_CB.csv` for all biomonitoring stations.
    
4.  Run `SelectedStationDataSelector_all.py` to produce 
    `Biomonitoring_Samples_CB.csv`.
    
5.  (Optional) Run `MostRecentDataSelector_all.py` to produce 
    `Most_Recent_Samples_Casco_Bay.csv`


# Detailed Explanation of Data Derivation
## Parsed CSV Files
The files `Biomonitoring_Samples_Parsed.csv` and `Biomonitoring_Stations_Parsed.csv`
Were generated from the KML files using a python script,`DataParserStation.py`,
and an associated "helper Class" defined in `htmlParse2Data.py`. 

The `DataParserStation.py` script is a simple script that instantiates 
a data parser, scans through the KML file and generates two CSV files, for
ease of further analysis.  Both input KML file and output CSV file names
are hard coded into the script.  To run `DataParserStation.py`, both the
source `KML` file and the helper python file, `htmlParse2Data.py` must be
in the same directory.

Users should be aware that the identity of some data columns is also hard
coded into the script, so results should be checked carefully for correct
column headers and missing data columns.

We have run the script through Spyder and Idle.  When we double click on
the python file in Windows, it briefly opens a command window, but
does not generate any output.  Running Python scripts directly from the
desktop in Windows is often problematic.

## Casco Bay Watershed Station Lists
We worked in ArcGIS to generate lists of biomonitoring
stations  and invertebrate biomonitoring stations in the Casco Bay Watershed. 
These files are called `CB_Biomonitoting_Stations.csv` and
`CB_Invertebrate_Stations.csv`. 

The process followed the obvious steps:
*  importing latitude and longitude data from
   `Biomonitoring_Stations_Parsed.csv` into ArcGIS as an event layer,
*  exporting the data as a shapefile,
*  selecting points that overlap the Casco Bay Watershed, 
*  (Filtering to Stream Invertebrate sampling stations if necessary). 

The only complex step in preparing the GIS files involved adding local 
imperviousness to the data.  That involved running a spatial merge with the
Catchments Data Layer (see below) to extract estimates of impervious cover in
local catchments, and exporting the data from ArcGIS. We removed unnecessary
data columns, and changed the file extension to ".csv".

## Casco Bay Watershed Sample Data
We generated `Biomonitoring_Samples_CB.csv` and
`Invertebrate_Samples_CB.csv` using python scripts, 
`SelectedStationDataSelector_all.py` and `SelectedStationDataSelector_inverts.py`.
The files contain sample results ONLY from stations within the Casco Bay
Watershed.  The two Python scripts differ only by including different hard
coded input and output file names.

# Most Recent Samples
We use data from the "most recent" samples to show "current" conditions for
each site.  Some of these historic observations, however, are fairly old, and
arguably not relevant for current status.  Further, filtering observations
would be simpler in R, but we had functioning code from 2015, which we updated
for 2020.

We use two Python scripts, including `MostRecentDataSelector_all.py` and
`MostRecentDataSelector_inverts.py` that differ only with regards to hard
coded file names.  The final produced files are
`Most_Recent_Samples_Casco_Bay.csv` and `Most_Recent_Invert_Samples_Casco_Bay.csv`

# GIS Files
## Biomonitoring Locations
We generated our geospatial data showing stream biomonitoring
locations as follows (intermediate geospatial layers are not exposed in the Git 
archive):

1.  We imported  latitude and longitude data from `Biomonitoring_Stations_Parsed.csv`
    into ArcGIS as an event layer, exporting the data as a shapefile, 
    `DEP_Biomonitoring`

2.  We selected points that overlap the Casco Bay Watershed, using 
    'Select by Location', producing `CB_Biomonitoring`

3.  We then further filtered to select only stream invertebrate sampling
    stations, using "Select by Attributes" to select biomonitoring stations
    with "Sample_Typ" == "Macroinvertebrate", producing a shapefile,
    `CB_Inverebrate_Biomonitoring`.

4.  Finally, we exported that files attribute table as a text (comma delimited)
    file, as described above to create the Casco Bay Watershed Station List
    described above.

## Catchments 
Since Maine has no recent high resolution state-wide impervious cover data, we
reused impervious cover data by catchment area we developed for the 2015
State of the Bay Report.  These data are based on impervious cover estimates from
Maine IF&W, based on a one meter pixel size.  Those estimates are based principally 
on aerial photography from 2007, so they represent on-the-ground conditions
from over a decade ago, but they are the most recent values available.  

Our derived data layer aggregated impervious cover data by (somewhat simplified)
NAD+ V2 catchment areas.  We simplified the NAD+ V2 catchements principally to
remove catchments under 50 Ha, and absorb some small coastal catchments
into adjacent areas that drain to the same coastal bodies of water.

Our notes from 2015 describe preparation of this data sets as follows:

1.	Starting from CascoBayCatchments_Over_50 Shapefile (derived brom NHD2+).  

2.	Copy data to a new file (CascoBayCatchments_Over_50_Imperviousness)  

3.	Add Data column for Area of imperviousness and Percent imperviousness.  

4.	Use ZonalStatisticsAsTable to produce a table that ties polygons to
        imperviousness.  The resulting table counts the impervious area, in square
        meters (because the original imperviousness grid had a resolution of
        one meter).  Sum would give the same answer, since each pixel is 1 m on
        a side.  

5.	Join the Catchment data layer to the results of ZonalStatisticAsTable. 
 
6.	Copy the value from the table for the Impervious Area for each polygon
        into the data column defined in step 2.  Remove the join.  Note that
        areas that contain no impervious cover trigger a ?warning? in this
        step, but the value added to the attribute table is correctly set to zero.  

7.	In the Percent Imperviousness column defined in step 2,
        Calculate percent impervious for each polygon from the catchment area in
        square meters and the impervious area in each.








