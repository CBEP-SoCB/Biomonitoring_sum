<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

# Original KML File Data
Maine's Department of Environmental Protection makes certain data available 
on-line at <https://www.maine.gov/dep/gis/datamaps/>.   Among those data, the 
site exposes GIS data on stream and wetland biomonitoring via a `KML` file, 
`lawb_biomonitoring.kml`.

This KML file consists principally of <NetworkLink> tags pointing to other 
online resources.  These are mostly KMZ files containing
data.  They appear to contain the same -- or nearly the same -- data,  organized 
in different ways for display purposes.

A `KMZ` file is zip archive containing a `KML` file, usually with other 
associated files. Our interest rests in analyzing the data contained in those
embedded KML files.

## Downloaded KMZ files
We work principally with the file found through the following URL:
<https://www.maine.gov/dep/gis/datamaps/lawb_biomonitoring/lawb_biomonitoring_station.kmz>.
which is one of the online resources referenced within `lawb_biomonitoring.kml`.

We accessed that file by identifying the link in `lawb_biomonitoring.kml`, and 
downloading the file manually.

That file is modified and uploaded periodically, so the date of access matters.

This analysis is based principally on a version of the file downloaded by 
Curtis C. Bohlen on April 17th of 2019.  When we tried to access the same file 
on November 14, 2020, the file accessed through that URL contained only the 
"Statutory Class" field for individual sample results, and not the "Final 
Determination".  We contacted Maine DEP to alert them to the problem, and were
told that the situation was temporary, but we have not revisited the DEP web
page to confirm that the complete data is now once again available.

The `KML` file, `lawb_biomonitoring_station_2019.kml`, was extracted from
a KMZ file by converting the 'kmz' extension to `zip`, and unzipping in Windows. 
We discarded the associated graphic symbols, which we do not need.

## KML File Data Format
These `KML` files contain principally `<Placemark>` tags. Each `<Placemark>`
contains the following tags:  

*  `<name>`         - The name of the placemark.  This appears to relate to a sample or a station.
                    It usually corresponds to a code or ID in the HTML
				
*  `<visibility>`  

*  `<styleUrl>`

*  `<Snippet maxLines='0'>` 

*  `<description>` - Contains HTML encoded data (see below)

*  `<Point>`       - Contains a `<coordinates>` tag containing a pair of
                    floating point values, representing Longitude and
					          Latitude (in that order).

##  HTML Data Format
The <description> tag contains a CDATA block that contains HTML. That
HTML contains all the data (both attributes and locations) that we need.
The HTML consists of two tables.

The FIRST table provides data on the sampling location.  The rows of 
the table contain the data.  Each row contains two entries, a data name,
and the data contents, each contained in a table cell.  For example, the
first row of the <description> tag for one entry contained the following:
<tr><td>Station:
*  LITTLE ANDROSCOGGIN RIVER - STATION 43</td></tr>

Successive rows of this first table contain the following: 

*  Station

*  Station Number

*  Town

*  County

*  Major Drainage

*  Site Type

*  Sample Type

*  Latitude

*  Longitude

The latitude and longitude here matches the latitude and longitude in the
`<coordinates>` of the matching `<Point>` tag.  Similarly, the Station Number in
the HTML matches the `<name>` tag in the `<Placemark>`.

The Second Table follows a line of bolded text containing "Sample(s):".  
It contains one or more rows providing summary data on **samples** taken at this
sampling location ("Station" in our parlance.)  The first row of this
second table provided table column headings. Remaining rows contain  data about 
individual samples from each sampling location.

The HTML data has the following contents:

*  Sample ID        (A sample code, usually (always?) a number for invertebrate 
                     samples)  
*  Sample Date      (In MM/DD/YYYY format)  
*  Statutory Class  (A code designating designated class:  'AA', 'A', 'B', 'C')
*  Attained Class   (Did the sample meet or exceed the designates stream class?)
*  Report           (Some samples have associated with them a PDF file 
                     containing results of sampling.  If available, this item 
                     contains an <a> tag containing a href pointing to the 
                     report.  
*  Final Determination  (A code designating achieved class:  'A', 'B', 'C', 
                         'NC', 'I')  
The contents of this table have varied from time to time, so you need to check
that the contents are as expected. In 2020, the "Attained Class" and "Final 
Determination" columns were missing, meaning the actual data of interest was 
temporarily unavailable.

After that table, the HTML often contains lines containing links to photographs
and a separate HTML version of the data.  
*  DOWNSTREAM PHOTO (<IMG alt='BIOMONITORING DOWNSTREAM WEB PHOTO'....>)  
*  UPSTREAM PHOTO   (<IMG alt='BIOMONITORING UPSTREAM WEB PHOTO'....>)  
*  View or Print in web browser (the `<a href =....>` tag to a separate HTML
   version of the data immediately precedes that text).

# Data Parsing.
Several Python scripts and simple GIS analysis were used to parse the data from 
this KML file, select data only from locations within the Casco Bay watershed, 
and convert the data to CSV files.  The process generates two CSV files, one 
listing stations, the other listing samples. The Sample data is linked to the 
Stations data by a common "Station Number" key. 

Related files in this archive are
`Biomonitoring_Stations_CB.csv` `Biomonitoring_Samples_CB.csv`.

# Diving Deeper.
The DEP KML files frequently contain links to underlying data files giving
results of biomonitoring in great detail.  These secondary links take two forms:
PDF reports and CSV tabular summaries of individual parameters.  Both CSV and
PDF files can be downloaded from the DEP server using a simple URL request, if
you know the appropriate URL.  The URL format is fairly simple, consisting of a
long header, followed by a sample ID, and last, the ".csv. extension, as
follows:

* the URL header: 
  "http://www.maine.gov/dep/gis/datamaps/lawb_biomonitoring/results/"

* A Sample ID Number:   e.g., Sample "314" from Station S-143. (We have a list 
  of Sample ID Numbers now.)

* The Suffix ".csv'

The resulting URL:
<http://www.maine.gov/dep/gis/datamaps/lawb_biomonitoring/results/314.csv>
accesses the CSV file, which can be saved directly, or parsed.

We have developed draft Python scripts to access and parse these CSV files, but
we do not use the raw CSV files in State of Casco Bay, so they are not included
here.