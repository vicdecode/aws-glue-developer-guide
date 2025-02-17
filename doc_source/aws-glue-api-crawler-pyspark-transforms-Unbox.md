# Unbox Class<a name="aws-glue-api-crawler-pyspark-transforms-Unbox"></a>

Unboxes a string field in a `DynamicFrame`\.

## Methods<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-_methods"></a>
+ [\_\_call\_\_](#aws-glue-api-crawler-pyspark-transforms-Unbox-__call__)
+ [apply](#aws-glue-api-crawler-pyspark-transforms-Unbox-apply)
+ [name](#aws-glue-api-crawler-pyspark-transforms-Unbox-name)
+ [describeArgs](#aws-glue-api-crawler-pyspark-transforms-Unbox-describeArgs)
+ [describeReturn](#aws-glue-api-crawler-pyspark-transforms-Unbox-describeReturn)
+ [describeTransform](#aws-glue-api-crawler-pyspark-transforms-Unbox-describeTransform)
+ [describeErrors](#aws-glue-api-crawler-pyspark-transforms-Unbox-describeErrors)
+ [describe](#aws-glue-api-crawler-pyspark-transforms-Unbox-describe)

## \_\_call\_\_\(frame, path, format, transformation\_ctx = "", info="", stageThreshold=0, totalThreshold=0, \*\*options\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-__call__"></a>

Unboxes a string field in a `DynamicFrame`\.
+ `frame` – The `DynamicFrame` in which to unbox a field\. \(required\)\.
+ `path` – The full path to the `StringNode` to unbox \(required\)\.
+ `format` – A format specification \(optional\)\. This is used for an Amazon Simple Storage Service \(Amazon S3\) or an AWS Glue connection that supports multiple formats\. See [Format Options for ETL Inputs and Outputs in AWS Glue](aws-glue-programming-etl-format.md) for the formats that are supported\.
+ `transformation_ctx` – A unique string that is used to identify state information \(optional\)\.
+ `info` – A string associated with errors in the transformation \(optional\)\.
+ `stageThreshold` – The maximum number of errors that can occur in the transformation before it errors out \(optional; the default is zero\)\.
+ `totalThreshold` – The maximum number of errors that can occur overall before processing errors out \(optional; the default is zero\)\.
+ `separator` – A separator token \(optional\)\.
+ `escaper` – An escape token \(optional\)\.
+ `skipFirst` – `True` if the first line of data should be skipped, or `False` if it should not be skipped \(optional\)\.
+ withSchema`` – A string containing schema for the data to be unboxed \(optional\)\. This should always be created using `StructType.json`\.
+ `withHeader` – `True` if the data being unpacked includes a header, or `False` if not \(optional\)\.

Returns a new `DynamicFrame` with unboxed `DynamicRecords`\.

## apply\(cls, \*args, \*\*kwargs\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-apply"></a>

Inherited from `GlueTransform` [apply](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-apply)\.

## name\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-name"></a>

Inherited from `GlueTransform` [name](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-name)\.

## describeArgs\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-describeArgs"></a>

Inherited from `GlueTransform` [describeArgs](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeArgs)\.

## describeReturn\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-describeReturn"></a>

Inherited from `GlueTransform` [describeReturn](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeReturn)\.

## describeTransform\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-describeTransform"></a>

Inherited from `GlueTransform` [describeTransform](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeTransform)\.

## describeErrors\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-describeErrors"></a>

Inherited from `GlueTransform` [describeErrors](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describeErrors)\.

## describe\(cls\)<a name="aws-glue-api-crawler-pyspark-transforms-Unbox-describe"></a>

Inherited from `GlueTransform` [describe](aws-glue-api-crawler-pyspark-transforms-GlueTransform.md#aws-glue-api-crawler-pyspark-transforms-GlueTransform-describe)\.

## Example for Unbox<a name="pyspark-Unbox-example"></a>

The following commands create the `dyf_warehouse` DynamicFrame that is used in this example\.

```
warehouse_inventory_list = [ ['TX_WAREHOUSE', '{\ "strawberry":"220",\ "pineapple":"560",\ "mango":"350",\
 "pears":null}' ],\ ['CA_WAREHOUSE', '{\ "strawberry":"34",\ "pineapple":"123",\ "mango":"42",\ "pears":null}\ '], 
['CO_WAREHOUSE', '{\ "strawberry":"340",\ "pineapple":"180",\ "mango":"2",\ "pears":null}' ] ] 

warehouse_schema = StructType([StructField("warehouse_loc", StringType())\ ,StructField("data", StringType())]) 

df_warehouse = spark.createDataFrame(warehouse_inventory_list, schema = warehouse_schema)

dyf_warehouse = DynamicFrame.fromDF(df_warehouse, glueContext, "dyf_warehouse")
```

The Unbox function in the following example converts the lists within the `String` datatype into a structure\.

```
dyf_warehouse.printSchema()

root
|-- warehouse_location: string
|-- data: string

dyf_unbox = Unbox.apply(frame = dyf_warehouse, path = "data", format="json")
dyf_unbox.printSchema()

root
|-- warehouse_loc: string
|-- data: struct
| |-- strawberry: int
| |-- pineapple: int
| |-- mango: int
| |-- pears: null 

dyf_unbox.toDF().show() 

+-------------+----------------+
|warehouse_loc| data|
+-------------+----------------+
| TX_WAREHOUSE|[220, 560, 350,]|
| CA_WAREHOUSE| [34, 123, 42,]|
| CO_WAREHOUSE| [340, 180, 2,]|
+-------------+----------------+
```