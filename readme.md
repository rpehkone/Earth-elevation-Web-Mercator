
### AW3D30 to WGS84 Web Mercator zxy quadtree conversion
- [jaxa description](https://www.eorc.jaxa.jp/ALOS/en/dataset/aw3d30/aw3d30_e.htm)
- [opentopography description](https://portal.opentopography.org/raster?opentopoID=OTALOS.112016.4326.2)

#### Data download (220 GB)
- Install aws cli: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- aws s3 cp s3://raster/AW3D30/ . --recursive --endpoint-url https://opentopography.s3.sdsc.edu --no-sign-request
- Took 50min@586MB/s
- python3 -m pip install numpy pygeodesy rasterio opencv-python
- download online egm96-5.pgm, EGM96 geoid (18 MB)

#### Height format (custom)
- Mapbox used the [pseudo base-256 rgb24 encoding](https://github.com/mapbox/rio-rgbify) because works with all image parsers, but then vertex shader has to spam the conversion
- This generates pngs with signed uint16 data that can be converted to float gl texture
- image = (image + 11000) * (65535 / (11000 + 11000))

#### Conversion speed
- Precompute heightmap for egm96 (gravitational undulation calculations are slow)
- Operate with lists instead of single elevation samples
- Don't save tiles with no elevation data (image encoder slow)
- Early exit by testing output tile corners for elevation data existence
- multiprocessing
- After optimization png encoder remains as a bottleneck
- Level 10 output size 13GB


#### Image data under jaxa licence:
![](img/Death_Valley_California_USA.png)
![](img/Dead_Sea_Jordan-Israel.png)

![](img/Mount_Everest_China-Nepal.png)
![](img/New_York_USA.png)
<br/>

![](img/Aconcagua_Argentina.png)
![](img/Mount_Kilimanjaro_Tanzania.png)
<br/>

![](img/Qattara_Depression_Egypt.png)
![](img/Salton_Sea_California_USA.png)
<br/>

![](img/Turpan_Depression_Xinjiang_China.png)
![](img/Denali_Alaska_USA.png)
<br/>

![](img/Halti_Norway-Finland.png)
![](img/Caspian_Depression_Kazakhstan.png)
<br/>
