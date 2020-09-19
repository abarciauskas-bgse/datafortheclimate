---
title: "Marine Heat Waves"
date: 2020-09-19T08:57:16-07:00
draft: false
---

During [OceanHackWeek 2020](https://oceanhackweek.github.io/), I was introduced the concept of [marine heat waves](https://research.noaa.gov/article/ArtMID/587/ArticleID/2559/So-what-are-marine-heat-waves).

Marine heat waves are massive warming events with extents over 100 kilometers. Climate change has made these warming events more frequent and scientists suspect they are longer and span larger areas. These events impact fisheries, weather and biodiversity.

Yet, research so far has been limited to a temporal scale of 5 days. Chelle Gentemann, oceanographer at Falleron Institute, believes a lot of ecosystems can handle short 5-day heat waves, but the impact of longer heat waves has yet to be studied. A more complete picture of heat waves and heat wave variability is, according to Chelle, “a computational problem." 

An OceanHackWeek team formed to develop a flexible heat wave modeling framework (see their github project: [ohw20-proj-marine-heat-waves](https://github.com/oceanhackweek/ohw20-proj-marine-heat-waves)). The hackweek team used the [Multi-Scale Ultra High Resolution (MUR) Sea Surface Temperature (SST) in Zarr](https://registry.opendata.aws/mur/) dataset and will use [dask](https://docs.dask.org/en/latest/) to scale spatial and temporal search for marine heat wave events. Using this framework, scientists will explore correlations with weather and changes in biodiversity.

Below, is some example code demonstrating how to create a timeseries of sea surface temperature data from 2001 to 2020 at one point.

One can see peaks in the data between 2014 and 2016, likely corresponsing to ["the Blob" marine heat wave](https://www.nytimes.com/2016/04/10/world/asia/climate-related-death-of-coral-around-world-alarms-scientists.html).

>the Blob is up to four degrees Fahrenheit warmer than surrounding waters, and has been blamed for a host of odd phenomena, including the beaching of hungry sea lions in California and the sighting of tropical skipjack tuna off Alaska.



```python
file_location = 's3://mur-sst/zarr'
ikey = fsspec.get_mapper(file_location, anon=True)
ds_sst = xr.open_zarr(ikey,consolidated=True)
sst_timeseries = ds_sst['analysed_sst'].sel(time = slice('2002-06-01','2020-01-01'),
                                            lat  = 47,
                                            lon  = -125,
                                           ).load()

sst_timeseries.plot()
```

![/images/mursst-timeseries.png](/images/mursst-timeseries.png)
