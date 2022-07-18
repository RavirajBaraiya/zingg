---
parent: Creating training data
grand_parent: Step By Step Guide
nav_order: 2
---

# label

## Providing user feedback on the training pairs

This phase opens an interactive learner where the user can mark the pairs found by findTrainingData phase as matches or non-matches. The findTrainingData phase generates edge cases for labeling and the label phase helps the user mark them.

`./zingg.sh --phase label --conf config.json`

![Shows records and asks user to mark yes, no, can't say on the cli.](../../../assets/label.gif)

Proceed running findTrainingData followed by label phases till you have at least 30-40 positives, or when you see the predictions by Zingg converging with the output you want. At each stage, the user will get different variations of attributes across the records. Zingg performs pretty well with even a small number of training, as the samples to be labeled are chosen by the algorithm itself.
