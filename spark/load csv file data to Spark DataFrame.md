# Load CSV file data to Spark DataFrame

### Spark 1.6 (at Zeppelin 0.6)

```python
%pyspark

from pyspark.sql import SQLContext
from pyspark.sql.types import *
from pyspark.sql.functions import udf
from datetime import datetime
sqlContext = SQLContext(sc)

def loadBusData(file):
    # Load a text file and convert each line to a tuple.
    bus = sc.textFile(file).map(lambda l: l.split(",")).map(lambda b: (b[0], b[1], b[2], b[3], b[4]))
    
    # The schema is encoded in a string.
    schemaString = "carId routeId bus_lng bus_lat bus_time"
    
    fields = [StructField(field_name, StringType(), True) for field_name in schemaString.split()]
    schema = StructType(fields)
    
    # Apply the schema to the RDD.
    df = sqlContext.createDataFrame(bus, schema)
    
    # convert column to right DataType
    df = df.withColumn("bus_lat", df.bus_lat.cast(FloatType())).withColumn("bus_lng", df.bus_lng.cast(FloatType()))
    str2datetime_udf = udf(lambda x: datetime.strptime(x, '%Y-%m-%d %H:%M:%S'), TimestampType())
    df = df.withColumn("bus_time", str2datetime_udf('bus_time'))
    
    return df
    
bus_df = loadBusData("/tmp/data/spark_data/bus/20160814.csv")
bus_df.show()
```

### Spark 2.0 (at Zeppelin 0.7)

```python
%spark.pyspark 

from pyspark.sql.types import *
busSchema = StructType([
    StructField("carId", StringType()),
    StructField("routeId", StringType()),
    StructField("bus_lng", FloatType()),
    StructField("bus_lat", FloatType()),
    StructField("bus_time", TimestampType())
])

bus_df = spark.read.csv("/tmp/data/spark_data/bus/20160814.csv", header=False, mode="DROPMALFORMED", schema=busSchema)
bus_df.show()
```

**註**：Spark 2.0 + Zeppelin 0.7 操作 DataFrame 時，會有`AttributeError: 'JavaMember' object has no attribute 'parseDataType'`的錯誤，是一個已知的 bug ([Zeppelin PySpark: 'JavaMember' object has no attribute 'parseDataType'](http://stackoverflow.com/questions/39227534/zeppelin-pyspark-javamember-object-has-no-attribute-parsedatatype))。

### 參考資料

- [Load CSV file with Spark](http://stackoverflow.com/questions/28782940/load-csv-file-with-spark)


