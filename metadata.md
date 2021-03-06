# Metadata

There are eight tables that are described below. example data is stored in [data](data).

-   [Measurement](#Measurement) ([Measurements.csv](data/Measurements.csv))
-   [Sample](#Sample) ([Sample.csv](data/Sample.csv))
-   [Site](#Site) ([Site.csv](data/Site.csv))
-   [Reporter](#Reporter) ([Reporter.csv](data/Reporter.csv))
-   [Lab](#Lab) ([Lab.csv](data/Lab.csv))
-   [AssayMethod](#AssayMethod) (AssayMethod.csv - **TBA**)
-   [Polygon](#Polygon) (Polygon.csv - **TBA**)
-   [CovidPublicHealthData](#CovidPublicHealthData) (CovidPublicHealthData.csv - **TBA**)

Entity Relationship Diagram [here](#erd).

## Entity Relationship Diagram <span id="erd"><span>

![](img/ERD.svg)

Comment on the ERD in [Lucidcharts](https://lucid.app/lucidchart/023490f3-6cc5-41be-bc2d-d96425f3c68f/edit?page=0_0#?folder_id=home&browser=icon)

## Measurement (Measurement.csv) <span id="Measurement"><span>

Results for a measurement of a single property of SARS-CoV-2 wastewater test, for either a viral region or wastewater treatment plant.


  - `PPMV`: SARS-CoV-2 gene region N1
-   **MeasurementID**: Unique identification for measurement (Primary Key).

-   **sampleID**: Links with the identified sample (foreign key).

-   **labID**: Links with the identified Lab that performed the analysis (foreign key).

-   **assayID**: Links with the AssayMethod table used to perform the analysis (foreign key).

-   **analysisDate**: Date the data was analysed in the lab.

-   **reportDate**: Data the data was reported. One sampleID may have updated reports based on updates to assay method or reporting standard. In this situation, use the original `sampleID` but updated `measurementID`, `reportDate` and `assayID` (if needed).

-   **sampleFraction**: Faction of the sample that is analyzed.

    -   `liquid`: Liquid fraction
    -   `solid`: Solid fraction
    -   `mixed`: Mixed/homogenized sample

-   **measureCat**: Gene target region (`covid-`) or wastewater treatment plant parameter (`ww-param-`).

    - `covidUnspecified (default)`: 
    - `covidN1`: SARS-CoV-2 gene region N1
    - `covidN2`: SARS-CoV-2 gene region N2
    - `covidN3`: SARS-CoV-2 gene region N3
    - `covidE`: SARS-CoV-2 gene region E
    - `covidRdRp`: SARS-CoV-2 gene region RdRp
    - `covidN1N2avg`: SARS-CoV-2 gene region average of N1 and N2
    -   `covidN1N2avg`: 
    -   `wwParamFlow`: 
    -   `wwParamTss`: 
    -   `wwParamBod`: 
    -   `catOther`: 

-   **measureCatOther**: Description for other target region (use prefix `covid-` or wastewater treatment plant parameter (use prefix `ww-param-`. See `measureCat`.

-   **measureUnit**: Unit of SARS-CoV-2 measurement.

    -   `PMMV`: Viral copies/copies PMMoV
    -   `ml`: Viral copies/mL
    -   `gms`: Viral copies/gm solids
    -   `l`: Viral copies/L
    -   `crA`: Viral copies/copies crAssphage
    -   `Ct`:
    -   `m3s`: meters cubed per second
    -   `mgl`: milligrams per liter
    -   `mgOl`: milligrams of oxygen per liter
    -   `measureOther`: Other measurement of viral copies or wastewater treatment plant parameter. Also add `measureUnitOther`.

-   **measureUnitOther**: Description for other type of SARS-CoV-2 measurement unit. See `measureUnit`.

-   **measureType**: Statistical measures used to report the sample units of Ct/Cq, unless otherwise stated. Each measureType has a corresponding value (measureValue).

    -   `geoMean`: GeoMean of results
    -   `singleton`: This value is not an aggregate measurement in any way, and thus is not a `mean`, `median`, `geomean` or other
    -   `mean`: Sample mean
    -   `meanNormal`: Sample mean, normalized
    -   `median`: Median of results
    -   `rangeLowestValue`: Lowest value in a range of values
    -   `rangeHighestValue`: Highest value in a range of values
    -   `SD`: Sample standard deviation
    -   `SDNormal`: Sample standard deviation, normalized
    -   `typeOther`: Other measures

-   **measureTypeOther**: Description for other type of measurement type. See `measurementUnit`.

-   **measureValue**: Value of measureType.

-   **measureValueDetected**: Boolean Value if True then covid-19 was detected.

-   **notes**: Any additional notes.

## Sample (Sample.csv) <span id="Sample"><span>

The sample is an amount of water taken from a site which is then analysed by a lab.

-   **sampleID**: Unique identification for sample. Suggest siteID-date-sample, or siteID-. (Primary Key)

-   **siteID**: links with the site table. (foreign key)

-   **dateTime**: for grab samples this is the date and time and timezone the sample was taken.

-   **dateTimeStart**: for integrated time average samples this is the date and time and timezone the sample was started being taken.

-   **dateTimeEnd**: for integrated time average samples this is the date and time and timezone the sample was finished being taken.

-   **sampleType**: type of sample.

    -   `sludge`: Primary clarified sludge
    -   `rawCollector`: Raw wastewater (in collector system)
    -   `rawPostGrit`: Raw wastewater (post-grit)
    -   `other`: Other type of site. Add description to `sampleTypeOther`.

-   **sampleTypeOther**: Description for other type of sample type. See `sampleType`.

-   **methodCollection**: method used to collect the data.

    -   `grab sample`: Sample was a simple grab sample
    -   `contFlowProp`: Continuous flow proportional
    -   `contConstant`: Continuous constant
    -   `contOther`: Continuous other
    -   `discTimeProp`: Discrete time proportional
    -   `discTimeProp24hq1h`: Discrete time proportional 24-hour composite, every 1 hr
    -   `discTimeProp24hq4h`: Discrete time proportional 24-hour composite, every 4 hr
    -   `discTimeProp24hq6h`: Discrete time proportional 24-hour composite, every 6 hr
    -   `discFlowProp`: Discrete flow proportional
    -   `discVolumeProp`: Discrete volume proportional
    -   `discOther`: Discrete other
    -   `integratedOther`: Integrated other

-   **methodCollectionOther**: Description for other type of method when any option with `other` is selected `methodCollection`.

-   **sampleSizeL**: Total volume of water or sludge sampled.

-   **sampleStorageTempC**: Temperature that the sample is stored at in Celsius.

-   **notes**: Any additional notes.

## Site (Site.csv) <span id="Site"><span>

The site of wastewater sampling, including several *defaults* that can be used to populate new samples.

-   **siteID**: (Primary Key) Unique identifier for the location where wastewater sample was taken.

-   **siteName**: Name corresponding to `siteID`. Location name could be a treatment plant, campus, institution or sewer location, etc.

-   **siteDescription**: Description of wastewater site (city, building, street, etc., to identify location sampled).

-   **reporterID**: links with the reporter that is responsible for the data (foreign key).

-   **siteType**: Type site where the sampling is taken.

    -   `SAPFlowWater`: Sewer access point flowing water
    -   `SAPStandingWater`: Sewer access point standing water
    -   `treatPlantInfluent`: Treatment plant influent
    -   `treatPlantEffluent`: Treatment plant effluent
    -   `lagoon`: Lagoon
    -   `other`: Other

-   **siteTypeOther**: Description of site where the site is other. See `siteType`

-   **sampleTypeDefault**: Used as default when new samples records are created see `SampleType` in `Sample` table.

-   **sampleTypeOtherDefault**: Used as default when new `Sample` records are created see `SampleTypeOther` in `Sample` table.

-   **methodCollectionDefault**: Used as default when new `Sample` records are created see `methodCollection` in `Sample` table.

-   **methodCollectOtherDefault**: Used as default when new `Sample` records are created see `methodCollectionOther` in `Sample` table.

-   **sampleFractionDefault**: Used as the default when new `Measurement` records are created.

-   **sampleTempCdefault**: Used as the default when new `Sample` records are created

-   **geoLat**: Site geographical location, latitude in decimal coordinates, ie.: (45.424721)

-   **geoLong**: Site geographical location, longitude in decimal coordinates, ie.: (-75.695000)

-   **notes**: Any additional notes.

-   **sewerNetworkPolygonID**: Links with the Polygon table.(foreign key)

-   **sewerNetworkFileLink**: Link to a file that has any detailed information about the sewer network associated with the site (any format).

-   **sewerNetworkFileBLOB**: A file BLOB that has any detailed information about the sewer network associated with the site (any format).

## Reporter (Reporter.csv) <span id="Reporter"><span>

The individual or organization that is reporting and responsible for the quality of the data.

-   **reporterID**: (Primary Key) Unique identifier for the person or organization that is reporting the data.
-   **siteIDDefault**: Used as the default when new `Sample` records are created by this `reporter`.
-   **labIDDefault**: Used as the default when new `Sample` records are created by this `reporter`.
-   **contactName**: Full Name of the reporter, either an organization or individual.
-   **contactEmail**: Contact e-mail address.
-   **contactPhone**: Contact phone number.
-   **allowAcceesToSelf**: Default: True. If this is False the data will not be shown on the portal when the data provider logs in
-   **allowAcceesToFederalPublicHealthAuthorities**: Default: True. If this is False the data will not be available to employees of PHAC
-   **allowAccessToLocalPublicHealthAuthorities**: Default: True. If this is False data will not be available when local health Authorities log in.
-   **allowAccessToProvincialPublicHealthAuthorities**: Default: True. If this is False data will not available when provincial health Authorities log in.
-   **allowAccessToOtherDataProviders**: Default: True. If this is False data will not be available when other data providers login.
-   **allowAccessToAllOrganizations**: Default: True. If this is False data will not be available when any partner organization logs into the system
-   **allowAccessToPublic**: Default: True. If this is False data will not be available to the public.
-   **allowAccessToSpec**: Details or specifics on confidentiality requirements.
-   **notes**: Any additional notes.

## Lab (Lab.csv) <span id="Lab"><span>

Laboratory that performs SARS-CoV-2 wastewater testing at one or more sites.

-   **labID**: Unique identifier for the laboratory. (Primary Key)
-   **assayIDDefault**: Unique identifier for the assay normally performed by this lab, use to populate new `measurement` records. (foreign key)
-   **laboratoryName**: Name corresponding to lab.
-   **contactName**: Contact person or group, for the lab.
-   **contactEmail**: Contact e-mail address, for the lab.
-   **contactPhone**: Contact phone number, for the lab.
-   **labUpdateDate**: Date information was provided or updated.

## AssayMethod (AssayMethod.csv) <span id="AssayMethod"><span>

The assay method that was used to perform testing. This database will be developed in consultation with testing labs to identify key assay features that can affect SARS-CoV-2 results.

-   **assayID**: (Primary key) Unique identifier for the assay method.

-   **version**: Version of the assay. [Semantic versioning](https://semver.org) is recommended.

-   **sampleSizeL**: Size of the sample that is analysed in liters.

-   **loq**: Limit of Quantification for this method if one exists.

-   **lod**: Limit of detection for this method if one exists.

-   **methodUnits**: Units used by this method, that are applicable to the LOD or LOQ.

    -   `PMMoV`: Viral copies/copies PMMoV
    -   `ml`: Viral copies/mL
    -   `gms`: Viral copies/gm solids
    -   `l`: Viral copies/L
    -   `crA`: Viral copies/copies crAssphage
    -   `m3s`: meters cubed per second
    -   `mgl`: milligrams per liter
    -   `mgOl`: milligrams of oxygen per liter
    -   `measureOther`: Other measurement of viral copies or wastewater treatment plant parameter. Also add `measureUnitOther`.

-   **methodUnitsOther**: Units used by this method, that are applicable to the LOD or LOQ.

-   **assayDate**: Date the assayMethod was created or updated (for version update).

-   **inhibition**: Text decription of the inhibition.

-   **surrogateRecovery**: Text description of the Surrogate Recovery for this method.

-   **assayDesc**: Description of assay.

## Polygon (Polygon.csv) <span id="Polygon"><span>

A simple polygon that encloses an are on the surface of the earth, normally these polygons will either be of a sewer catchment area or of a health region or other reporting area.

-   **polygonID**: (Primary key) Unique identifier for the polygon.

-   **polygonName**: Name of the polygon (e.g. G.E. Booth catchment area, Ottawa Health Region).

-   **polygonPop**: Approximate population size of living inside a given polygon.

-   **polygonType**: Type of polygon

    -   `sewerNetwork` : Sewer network
    -   `healthRegion` : Health region served by the sewer network

-   **polygonWKT** [well known text](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) of the polygon

-   **polygonFile** File for storing the polygon BLOB format.

-   **polygonLink** Link to an external file that describes the geometry.

## CovidPublicHealthData (CovidPublicHealthData.csv) <span id="CovidPublicHealthData"><span>

Saves some information about covid-19 in a given polygon.

-   **publicHealthID**: (Primary key) Unique identifier for the table.

-   **ReporterID** ID of the reporter who gave this data

-   **PolygonID**: Links with the `Polygon` table (foreign key).

-   **date**: Date of covid-19 measure.

-   **dateType**: Type of date used.

    -   `episodeDate` : Episode date is usually just the earliest of a list of dates available as not every case has every date
    -   `onsetDate`: Earliest that symptoms were reported for this case
    -   `reportedDate`: Date that the numbers were reported publicly

-   **confirmed**: Number of confirmed cases.

-   **active**: Number of active cases.

-   **tests**: Number of tests.

-   **positiveTests**: Number of positive tests.

-   **percentPositivityRate**: Percent postivity rate.

-   **hospitalCensus**: Hospital census or the number of people admitted with covid-19.

-   **hospitalAdmit**: Hospital admissions or patients newly admitted to hospital.

## File naming convention

-   **variable and category names**: Both variables and variable categories use camelCase with long tables. Wide tables use `_` to concatenate variables from long tables.
-   **merging Tables** : when you merge tables concatenate column names with `.`. So `dateTime` from the `Sample` table becomes `Sample.dateTime`.

A long table would represent a test sample as the following:

    measureCat = covidN1
    measureUnit = PPMoV
    measureType = Mean
    measureValue = 42

A wide table would represent the same measurement as:

    covidN1_PPMV_mean = 42

-   **date**: MM/DD/YYYY HH:mm:ss (24 hour format, in UTC)
-   **location**: TBD
-   **versions**: [Semantic versioning](https://semver.org)
