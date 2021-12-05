# Analysis of Maine DEP Stream Biomonitoring Data

Analysis of Maine stream invertebrate biomonitoting data for 2020 State of 
Casco Bay report.

<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

# Statement of Purpose
CBEP is committed to the ideal of open science.  Our State of the Bay data
archives ensure the science underlying the 2020/2021 State of Casco Bay report
is documented and reproducible by others. The purpose of these archives is to
release  data and data analysis code whenever possible to allow others to
review, critique, learn from, and build upon CBEP science.

# Archive Structure
CBEP 2020/2021 State of the Bay data analysis summaries contain a selection of 
data,  data analysis code, and visualization code as used to produce 
results shared via our most recent State of Casco Bay report. Usually, these
archives are organized into two or three folders, including the following:

- `Data`  folder.  Contains data in simplified or derived form as used in our
data  analysis.  Associated metadata is contained in related Markdown documents,
usually `DATA_SOURCES.md` and `DATA_NOTES.md`.

- Analysis.  Contains one or more R Notebooks proceeding through the principal
data analysis steps that underpin SoCB reporting. To simplify the archives,
much preliminary analysis, and many analysis "dead ends" have been omitted. 

- Graphics.  Contains R Notebooks stepping through development of graphics, and
also copies of resulting graphics, usually in \*.png and \*.pdf formats.  These
graphics may differ from graphics as they appear in final State of the Bay
graphical layouts. Again, most draft versions of graphics have been omitted for 
clarity.

# Summary of Data Sources

Maine's Department of Environmental Protection makes certain data available
on-line at https://www.maine.gov/dep/gis/datamaps/.   Among those data, the
site exposes GIS data on stream biomonitoring via a `KML` file,
`lawb_biomonitoring.kml`.

"KML"" files are used by (*inter alia*) Google Earth to represent geographic
data. At their heart, `KML` files are a specific flavor of `XML`, and they can
be parsed successfully by tools able to parse KML files.  Unfortunately, KML
files are also very flexible, and the data we are interested in finding can be
buried in complex ways.

1. `KML` files are hierarchical, so that one `KML` file can refer to data
embedded in other files on the internet.  In fact, the raw `KML` file DEP
exposes (above) refers to several other files that contain the actual
geographic data.

2. `KML` files often contain embedded data that is itself HTML encoded. The
default way that ArcGIS creates `KML` files apparently embeds the attribute
table for each feature in an embedded `HTML` table. When we access the
attributes, we need to parse an HTML table embedded in a `XML` folder.

3.  Closely related `KMZ` files are zip-encoded `KML` files, generally zipped
along with related information, such as symbols used to display the geographic
data.

4.  Some related metadata and additional detailed data is contained in files not
directly accessed by the KML files, but whose names can be inferred from the
attribute data in the KML file.

All data reported here was accessed from the DEP website, based on information
accessible through that "kml" file.  CBEP used small python scripts
to access and reorganize available data.  Data was initially accessed at a 
state-wide level and secondarily filtered down to data only from the Casco Bay 
watershed, using a combination of GIS and python code. Details and code are 
available in a companion GitHub repository, `Biomonitoring_Access`, but as the
URL for that archive is in flux, we recommend contacting CBEP directly for
additional information.
