# Remote Sensing Image Processing
基于python的遥感数字影像处理

## Conda


## python GDAL库
### 安装  
GDAL(Geospatial Data Abstraction Library)是一个用于栅格数据操作的库，是开源地理空间基金会（Open Source Geospatial Foundation，OSGeo）的一个项目  
GDAL是一个操作各种栅格地理数据格式的库。包括读取、写入、转换、处理各种栅格数据格式。  
GDAL是基于C/C++开发的，直接使用pip安装有时会有问题，可从https://gdal.org/download.html 网站上下载GDAL的wheel文件进行安装。  
或使用Conda进行安装。  
```
conda install -c conda-forge gdal
```
### 读取文件
```
    def read(self):
        """
        根据不同传感器类型读取对应的数据
        :return: 图像数组
        """

        dataset = gdal.Open(self.path)
        data_type = dataset.GetRasterBand(1).DataType

        # 图像栅格矩阵的行列数
        self.x_size = dataset.RasterXSize
        self.y_size = dataset.RasterYSize

        self.geotransform = dataset.GetGeoTransform()   # 仿射矩阵，左上角像素的大地坐标和像素分辨率
        self.projection = dataset.GetProjection()   # 地图投影

        image = np.zeros((self.y_size, self.x_size, len(sensor_type)), dtype=np.float32)
        
        for band in range(bands):
            band_img = dataset.GetRasterBand(band + 1)
            image[:, :, band] = band_img.ReadAsArray()

        self.nan_position = np.where(image == 0)  # 将图片中的0替换为nan
        image[self.nan_position] = np.nan
        del dataset

        return image
```
    
