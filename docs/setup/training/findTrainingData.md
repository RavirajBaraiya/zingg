---
parent: Creating training data
nav_order: 1
grand_parent: Step By Step Guide
---

# findTrainingData

## Finding pairs of records that could be similar to train Zingg

Zingg builds models to predict similarity. Training data is needed to build these models. The findTrainingData phase prompts Zingg to search for edge cases in the data. During findTrainingData, Zingg combs through the data samples and judiciously selects limited pairs for the user to mark. Zingg is very frugal about the training so that user effort is minimized and models can be built and deployed quickly.

This findTrainingData job writes the edge cases to the folder configured through zinggDir/modelId in the config.

`./zingg.sh --phase findTrainingData --conf config.json`

The findTrainingData phase is run first and then the label phase is run and this cycle is repeated so that the Zingg models get smarter from user feedback.
