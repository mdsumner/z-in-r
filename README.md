# z-in-r

Zarr is a very popular and growing format for multidimensional data arrays that is particularly designed for cloud 
storage and for efficient remote querying. The Zarr spec is very simple conceptually, it's a json metadata tree of encoded chunks of array data. Each chunk is either a binary file on disk or an object
in remote storage. These chunks can also be more abstract however, by referencing existing chunks *within* a file either local or remote. 

There is patchy and incomplete support for Zarr in R. In python it is natively supported by xarray, and has improving support for virtualization 
(VirtualiZarr will soon have an "open_mfvirtualdataset" function), and has a Rust-based replacement for the fsspec layer used by xarray. There is a zarrs Rust library
that is also compatible with VirtualiZarr and Icechunk (wip, really need to make sure this is accurate). 

GDAL 

- best used in classic mode via {gdalraster}
- gdalraster has an in-development support for GDALMultiDimRaster (our preferred support for the multimensional model)
- can be imported as a python module with reticulate, osgeo.gdal supports the full multidimensional GDAL model
- classic mode (raster as bands) can also be used with Zarr, but is less efficient than native multidim
- imported as rasterio (classic mode  only)
- available in part via {stars}::read_mdim()
- does not yet support virtualized chunks
- does not interface with Icechunk stores

zarrs

- fully-feature Rust-based library for Zarr

We propose a two-pronged approach to improving the existing support for Zarr in R.

1) Contribute to GDAL to ensure that the indirection needed by virtualized Zarr can be leveraged by the Zarr driver. Investigate the existing implemented
 (and specified) ways of encoding crs and transform. Investigate scope for Icechunk compatibility.
2) Create an R package to wrap zarrs.
3) 

### Alternatives to consider 

Import xarray with {reticulate}. This is a serious proposition and works well, potentially difficult to parallelize at scale given the
native use of parallellization tools in Python. 

The {pizzarr} package. This is the most well developed support in R, but isn't widely known, doesn't match existing ways of working (there's
no xarray in R so this is not a strong criticism).  Does not have virtualization or Icechunk compatibility. 

The Bioconductor Rarr package is closely related to pizzarr and shares some origins. 

NetCDF

- can be used to read Zarr via its internal NCZarr (this works then with RNetCDF, tidync, and {stars}::read_ncdf)
- no virtualization (this is possibly untrue) or Icechunk compatibility
