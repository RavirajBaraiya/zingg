---
parent: Creating training data
title: Exporting labeled data as csv
grand_parent: Step By Step Guide
nav_order: 4
---

# Exporting labeled data as CSV

## Export labeled data from previously labeled Zingg learner pairs and import it to a new Zingg instance/model

The labeled data of one model can be exported and used as training data for another model.

Here are the instructions to do this. We are going to use PySpark, which comes with the Apache Spark installation.

To run PySpark,

```
$ $SPARK_HOME/bin/pyspark
```

The PySpark command will open a PySpark shell. We will now read the labeled data from the existing model/labelled rounds. Please run the following commands in the PySpark shell.

## Getting Training data

Let us first read the labeled data into Spark as follows. You will need to pass the appropriate location of the model folder from where you want to copy the training data. Typically, this is the zinggDir/modelId/trainingData/marked folder. Zingg examples write the model under ZINGG\_HOME/models//trainingData/marked.

```
labeledData = spark.read.parquet("<fully qualified location of models/modelId>/trainingData/marked")
```

The labeled data above has extra attributes added by Zingg beyond the field definitions in config JSON. Let us define these in PySpark.

```
baseCols = ['z_cluster', 'z_zid', 'z_prediction', 'z_score', 'z_source', 'z_isMatch']
```

The following command will filter out all the Zingg added columns. After running, sourceDataColumns will contain the original columns which define the user data.

```
sourceDataColumns =  [c for c in labeledData.columns if c not in  baseCols]
```

But we also need the labels and the clusters assigned to them to utilize the work we did earlier during labeling. The following columns contain that information.

```
additionalTrainingColumns = ['z_cluster','z_isMatch']
```

Let us now build our data frame with the list of columns needed for training samples in the desired sequence.

```
trainingSampleColumns = [*additionalTrainingColumns, *sourceDataColumns]
```

With the trainingSampleColumns in place, let us now select all the required columns from the labeled data. This data will be exported to CSV and become our training data.

```
trainingSamples = labeledData.select(trainingSampleColumns)
```

Let us now save the samples in a single CSV file.

```
trainingSamples.coalesce(1).write.csv("<location>")
```

At the above location folder, a file named part\*\*\*.csv will get created. This file contains the labeled data so far.

## Getting Schema

We can generate the schema of this file to feed as part of the trainingSamples schema by the following command. This schema can be used for the trainingSamples schema in the Zingg config file.

```
trainingSamples.schema.jsonValue()
```

While running PySpark, you can view the list of columns or actual labeled data, print columns, etc. by using the following commands:

```
trainingSamples.show()
trainingSamples.columns
print(trainingSampleColumns)
```
