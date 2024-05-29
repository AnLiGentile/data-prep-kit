# Document ID Generation
Please see the set of
[transform project conventions](../../../README.md)
for details on general project conventions, transform configuration,
testing and IDE set up.

## Summary 

This transform assigns a unique integer ID to each row in a Spark DataFrame. It relies on the [monotonically_increasing_id](https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.functions.monotonically_increasing_id.html) pyspark function to generate the unique integer IDs. As described in the documentation of this function:
> The generated ID is guaranteed to be monotonically increasing and unique, but not consecutive. 

## Configuration and command line Options

The set of dictionary keys holding [DocIdTransform](src/doc_id_transform.py) 
configuration for values are as follows:

* _doc_id_column_name_ - specifies the name of the DataFrame column that holds the generated document IDs.

## Running
You can run the [doc_id_local.py](src/doc_id_local.py) (spark-based implementation) to transform the `test1.parquet` file in [test input data](test-data/input) to an `output` directory.  The directory will contain both the new annotated `test1.parquet` file and the `metadata.json` file.

### Launched Command Line Options 
When running the transform with the Spark launcher (i.e. SparkTransformLauncher),
the following command line arguments are available in addition to 
the options provided by the [python launcher](../../../../data-processing-lib/doc/python-launcher-options.md).

```
  --doc_id_column_name DOC_ID_COLUMN_NAME
                        name of the column that holds the generated document ids
```

### Running as spark-based application
```
(venv) cma:src$ python doc_id_local.py
18:32:13 INFO - data factory data_ is using local data access: input_folder - /home/cma/de/data-prep-kit/transforms/universal/doc_id/spark/test-data/input output_folder - /home/cma/de/data-prep-kit/transforms/universal/doc_id/spark/output at "/home/cma/de/data-prep-kit/data-processing-lib/ray/src/data_processing/data_access/data_access_factory.py:185"
18:32:13 INFO - data factory data_ max_files -1, n_sample -1 at "/home/cma/de/data-prep-kit/data-processing-lib/ray/src/data_processing/data_access/data_access_factory.py:201"
18:32:13 INFO - data factory data_ Not using data sets, checkpointing False, max files -1, random samples -1, files to use ['.parquet'] at "/home/cma/de/data-prep-kit/data-processing-lib/ray/src/data_processing/data_access/data_access_factory.py:214"
18:32:13 INFO - pipeline id pipeline_id at "/home/cma/de/data-prep-kit/data-processing-lib/ray/src/data_processing/runtime/execution_configuration.py:80"
18:32:13 INFO - code location {'github': 'github', 'commit_hash': '12345', 'path': 'path'} at "/home/cma/de/data-prep-kit/data-processing-lib/ray/src/data_processing/runtime/execution_configuration.py:83"
18:32:13 INFO - spark execution config : {'spark_local_config_filepath': '/home/cma/de/data-prep-kit/transforms/universal/doc_id/spark/config/spark_profile_local.yml', 'spark_kube_config_filepath': 'config/spark_profile_kube.yml'} at "/home/cma/de/data-prep-kit/data-processing-lib/spark/src/data_processing_spark/runtime/spark/spark_execution_config.py:42"
24/05/26 18:32:14 WARN Utils: Your hostname, li-7aed0a4c-2d51-11b2-a85c-dfad31db696b.ibm.com resolves to a loopback address: 127.0.0.1; using 192.168.1.223 instead (on interface wlp0s20f3)
24/05/26 18:32:14 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
24/05/26 18:32:15 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
18:32:17 INFO - files = ['/home/cma/de/data-prep-kit/transforms/universal/doc_id/spark/test-data/input/test_doc_id_1.parquet', '/home/cma/de/data-prep-kit/transforms/universal/doc_id/spark/test-data/input/test_doc_id_2.parquet'] at "/home/cma/de/data-prep-kit/data-processing-lib/spark/src/data_processing_spark/runtime/spark/spark_launcher.py:184"
24/05/26 18:32:23 WARN SparkStringUtils: Truncated the string representation of a plan since it was too large. This behavior can be adjusted by setting 'spark.sql.debug.maxToStringFields'.
```

### Doc ID Statistics
The metadata generated by the Spark `doc_id` transform contains the following statistics:
  * `total_docs_count`, `total_columns_count`: total number of documents (rows), and columns in the input table, before the `doc_id` transform ran    
  * `docs_after_doc_id`, `columns_after_doc_id`: total number of documents (rows), and columns in the output table, after the `doc_id` transform ran  