# Obtain information {#concept_nbj_1fv_vdb .concept}

## Obtain basic information and EXIF data {#section_bdf_bfv_vdb .section}

You can use `info` to obtain basic information about a file, including its width, length, file size, format.  If the file contains exif information, exif information is returned. Otherwise, only basic information is returned. Results that are returned are in JSON format. For more information, see[EXIF2.31](http://oss-attachment.cn-hangzhou.oss.aliyun-inc.com/DC-008-Translation-2016-E.pdf).

## Request format {#section_ebm_cfv_vdb .section}

Operation name: `info`

## Response format {#section_igk_ffv_vdb .section}

JSON format

## Examples {#section_fpf_gfv_vdb .section}

-   Example of a request for EXIF data under the condition that the source image does not have EXIF data

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/info](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/info)

    ```
    
      "FileSize": {"value": "21839"},
      "Format": {"value": "jpg"},
      "ImageHeight": {"value": "267"},
      "ImageWidth": {"value": "400"}
    
    ```

-   Example of a request for EXIF data under the condition that the source image has EXIF data

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/info](http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/info)

    ```
    
      "Compression": {"value": "6"},
      "DateTime": {"value": "2015:02:11 15:38:27"},
      "ExifTag": {"value": "2212"},
      "FileSize": {"value": "23471"},
      "Format": {"value": "jpg"},
      "GPSLatitude": {"value": "0deg "},
      "GPSLatitudeRef": {"value": "North"},
      "GPSLongitude": {"value": "0deg "},
      "GPSLongitudeRef": {"value": "East"},
      "GPSMapDatum": {"value": "WGS-84"},
      "GPSTag": {"value": "4292"},
      "GPSVersionID": {"value": "2 2 0 0"},
      "ImageHeight": {"value": "333"},
      "ImageWidth": {"value": "424"},
      "JPEGInterchangeFormat": {"value": "4518"},
      "JPEGInterchangeFormatLength": {"value": "3232"},
      "Orientation": {"value": "7"},
      "ResolutionUnit": {"value": "2"},
      "Software": {"value": "Microsoft Windows Photo Viewer 6.1.7600.16385"},
      "XResolution": {"value": "96/1"},
      "YResolution": {"value": "96/1"}}
    ```


