# SkyWatch API
### Overview
Through the API you can access the following satellite imagery and climate/atmospheric datasets:
* [Landsat-8](http://www.skywatch.co/landsat8-1) (Level 1 - imagery only)
* [AIRS](http://www.skywatch.co/airs) (Level 2 and 3)
* [GOSAT/ACOS](http://www.skywatch.co/gosat) (Level 2 and 3)
* [MOPITT](http://www.skywatch.co/mopitt) (Level 1 and 3)
* [OCO-2](http://www.skywatch.co/oco2) (Level 2 only)
* [TES](http://www.skywatch.co/tes) (Level 2 and 3)

You can use the API to search satellite imagery and datasets by wavelength (band), cloud cover, resolution, data level, source, location, and date-time. 

There are two ways to use the API: [Graphical User Interface](https://app.skywatch.co) (GUI) or Command-Line Interface (CLI). The GUI can only be used to search for Landsat-8 imagery. API calls through the CLI will return a JSON (JavaScript Object Notation) response with a signed URL to download imagery and datasets that meet the search criteria.

### API Usage
```curl -H "x-api-key: <api-key>" https://api.skywatch.co/data/time/<time-period>/location/<coordinates>/source/<instrument-satellite>/level/<data-level>/resolution/<max-resolution>/cloudcover/<max-cloudcover>/band/<wavelengths>```

**NOTE:** ```time/<time-period>``` and ```location/<coordinates>``` are the only two mandatory fields - others are optional. However, the **order of the fields is crucial**, even if some field are omitted. For example, ```resolution/<resolution>``` can't come after ```band/<wavelengths>``` even if ```cloud-cover/<cloud-cover>``` is omitted.

**NOTE:** The API supports backward compatibility with versions 0.1 and 0.2. e.g. ```https://cqh77pglf1.execute-api.us-west-2.amazonaws.com/prod/data/level/<data-level>/location/<coordinates>/time/<time-period>```

### Search Results, Limits, and Downloading the Data

The search results from the JSON response are sorted by descending order of the date and time the image or the data were captured.

The current API limits are 1000 requests per second, and 2000 bursts per request. 

Each signed URL can be directly downloaded through a browser or programmatically, which expires **1 hour** after being generated.

### API Fields

**api-key**

A personal API key will be created for you once you register at [app.skywatch.co](https://app.skywatch.co).

**time-period** 

One or two UTC timestamps in ISO format (yyyy-mm-ddThh:mm:ss.sssss+|-zzzz). Partial or complete dates and timestamps can be specified (e.g. 2009, 2009-12, 2009-12-25, 2009-12-25T13:25:00.0000+0000). If no time is specified, midnight UTC on the day in question is assumed. 

If only one timestamp is passed in, a one day range is assumed. For example, if 2009-12-25 is specified, the search takes place as if 2009-12-25,2009-12-26 was specified.

**coordinates**

A list of longitude, latitude coordinate pairs as a flat, comma-separated list. A list of two numbers represents a point, four numbers is a square area where the coordinates are the corners, or if there are more than four numbers the coordinates represent a closed polygon, where the first point equals the last point in the list. Because this list represents a number of points, there always has to be an even number of numbers in the list.

*Examples:* 
* Point: -71.1043443253,-42.3150676016
* Square:  -71.1043443253471,-42.3150676015829,71.1043443253471,42.3150676015829
* Polygon: 43.81173831375078,-79.69345092773438,43.51668853502909,-79.70581054687499,43.463884091369046,-79.38995361328125,43.69865837138954,-79.34463500976562,43.875128129336716,-79.34188842773438,43.81173831375078,-79.69345092773438

**instrument-satellite**

Search criteria can be specified by the source of the data - either the instrument on-board the satellite or the satellite itself. Single or multiple sources can be specified. 
Choice of sources are: *ACOS, AIRS, CAI, FTS-SWIR,  Landsat-8, MOPITT, OCO2, and TES.* This field is not case-sensitive.

**data-level**

The data level is an optional path of the API URL that corresponds to the [data processing levels](http://science.nasa.gov/earth-science/earth-science-data/data-processing-levels-for-eosdis-data-products/) for Earth observation data. Level 1, 2, and 3 (L1, L2, L3) datasets are available. If no data level is specified, datasets of all levels will be returned. Only a single level can be specified.
Choices are: *1, 2, and 3.*

**max-resolution**

This maximum resolution field is only applicable to imagery that's available through the API (i.e. Landsat-8). Resolution is in metres (m). Resolutions less-than or equal-to this value will be returned. The resolution for Landsat-8 is 15 m. The maximum resolution is 30 m. If resolution is omitted all imagery or data matching other search criteria will be returned.

**max-cloudcover**

This maximum cloud cover field is only applicable to imagery that's available through the API (i.e. Landsat-8). Cloud cover is given as a percentage (%) of the image covered by cloud (0 to 100). Images less-than or equal-to this cloud cover value will be returned. If cloud cover is omitted all imagery or data matching other search criteria will be returned.

**wavelengths**

Search criteria can be specified by the wavelength bands for imagery (i.e. Landsat-8) and by file type for non-imagery data (e.g. *Hierarchical-Data-Format*). 
Choices of bands are: *Blue, Cirrus, Coastal-Aerosol, Green, Hierarchical-Data-Format, Near-Infrared, Panchromatic, Red, Short-Wave-Infrared-1, Short-Wave Infrared-2, Thermal-Infrared-1, and Thermal-Infrared-2.* This field is not case-sensitive.

### Example API Calls

Examples of API calls and outputs can be found [HERE](https://github.com/skywatchspaceapps/api/blob/master/EXAMPLES.md).

### Dataset Documentation

The following list are links to documentation on the individual datasets available through the API:

* AIRS: [Version 6](http://disc.sci.gsfc.nasa.gov/AIRS/documentation/v6_docs):
  * [L3 data](http://disc.sci.gsfc.nasa.gov/AIRS/documentation/v6_docs/v6releasedocs-1/V6_L3_User_Guide.pdf)
* [GOSAT/ACOS](http://disc.sci.gsfc.nasa.gov/acdisc/documentation/ACOS.html)
  * [CAI L3 global radiance data](http://data.gosat.nies.go.jp/GosatUserInterfaceGateway/guig/doc/documents/GOSAT_ProductDescription_33_CAIL3_V2.00_en.pdf)
  * [FTS SWIR L3 data](http://data.gosat.nies.go.jp/GosatUserInterfaceGateway/guig/doc/documents/GOSAT_ProductDescription_31_FTSSWIRL3_V2.02_en.pdf)
* [MOPITT](http://www.acom.ucar.edu/mopitt/file-spec.shtml)
  * [L3 data](http://www2.acom.ucar.edu/sites/default/files/mopitt/v6_users_guide_201309.pdf)
* [OCO-2](http://disc.sci.gsfc.nasa.gov/OCO-2/documentation/oco-2-v6)
* [TES](https://eosweb.larc.nasa.gov/project/tes/tes_table)
  * [L3 data](http://tes.jpl.nasa.gov/uploadedfiles/TES_DPS_V11.8.pdf)

### HDF Documentation and Resources

The following list are links to documentation and resources that relate to HDF (Hierarchical Data Format; .hdf) (HDF4 and HDF5) file formats:

* [HDF5](https://www.hdfgroup.org/HDF5/) - all non-image datasets except AIRS
* [HDF4](https://www.hdfgroup.org/products/hdf4/) - all AIRS datasets
* Python libraries for HDF:
  * HDF5 - [h5py](http://www.h5py.org/)
  * HDF4 - [pyhdf](http://pysclint.sourceforge.net/pyhdf/)

### Known Issues

* API calls that take longer than 30 seconds to complete will time out. We recommend refining your search criteria to be narrow enough to complete within 30 seconds.

### Troubleshooting

For any issues or questions, please contact dexter@skywatch.co.
