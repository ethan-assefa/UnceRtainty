# UnceRtainty
*by Ethan Assefa, Thomas Burrell, Tatev Gomtsyan*

A package for helping evaluate logistic models for uncertainty and classification performance.

## Purpose
The `UnceRtainty` package provides evaluation of logistic models through a comprehensive suite of tools for assessing both uncertainty and classification performance. This package serves as a valuable resource for researchers, data scientists, and analysts working with logistic regression models. By offering a range of functions and visualizations, users can efficiently explore model uncertainties, assess the reliability of predictions, and gain insights into the overall performance of their logistic models. The package is designed to enhance the model evaluation process by offering a user-friendly interface, robust statistical metrics, and informative graphical outputs, empowering users to make informed decisions about model selection, calibration, and deployment. Whether users are focused on binary or multiclass classification tasks, your package aims to be a versatile and indispensable tool for evaluating logistic models in a rigorous and efficient manner. An example of 

## Getting Started
Starting out in the `UnceRtainty` package requires three elements for input: 

1. A logistic model object of the class `glm`
2. A dataset containing columns with the predictor observations for the test data
3. A dataset or vector containing the observed outcome/response observations for the test data

### Creating the train/test split for a dataset 
Splitting a dataset into a training set and a testing set is a fundamental step in the machine learning workflow to evaluate the performance of a model, this includes when working with logistic regression models. In the process outlined below, we use the `mtcars` dataset as an example for creating the split. Some more information on this dataset is provided below:

**Description:** The data was extracted from the 1974 Motor Trend US magazine, and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973–74 models). The dataframe contains 32 observations on 11 (numeric) variables listed below:

- `[, 1]	mpg`	Miles/(US) gallon
- `[, 2]	cyl`	Number of cylinders
- `[, 3]	disp`	Displacement (cu.in.)
- `[, 4]	hp`	Gross horsepower
- `[, 5]	drat`	Rear axle ratio
- `[, 6]	wt`	Weight (1000 lbs)
- `[, 7]	qsec`	1/4 mile time
- `[, 8]	vs`	Engine (0 = V-shaped, 1 = straight)
- `[, 9]	am`	Transmission (0 = automatic, 1 = manual)
- `[,10]	gear`	Number of forward gears
- `[,11]	carb`	Number of carburetors

Lets say we are interested in attempting to predict the binary outcome/response of the transmission `am` based on the quarter mile time `qsec`, weight `wt`, and number of forward gears `gear` of the car.

We begin by using the `sample()` function to randomly select for the index numbers of the rows - in the case of the code below, we are setting aside 80% (i.e. 0.8) of the observations for the training set and the remaining 20% for the test dataset. These randomly selected indexes are applied to the dataset to create a new objects the train (`train_data`) and test sets (`test_data`). For our test set, we remove the column holding our response variable, `am` (column 9 in the dataset). We create a seperate object to hold the `am` outcome values for the test set (`test_ys`).

```r
# Sets the seed so results are reproducible
set.seed(123)
# Creates a split of the indexes for our dataset
sample_indices <- sample(1:nrow(mtcars), 0.8 * nrow(mtcars))
# Splits the dataset into training set
train_data <- mtcars[sample_indices, ]
# Splits the dataset into testing set without outcome variable
test_data <- mtcars[-sample_indices, -9]
# Splits the dataset into just the outcome variable observations for the testing set without outcome variable
test_ys <- mtcars[-sample_indices, 9]
```

### The `unceRtain_object`


## Package Functions

### 
The 'Calculate Expected Predictor Error' takes in an unceRtain_object and finds the mean absolute value difference between the predictions and the true values. A smaller value means your predictions we're close to the true values, while large values mean you are further away. The values will be data/context dependent.
 
The 'set_cost' function takes in three arguments, the unceRtain_object previously created, the cost of a false positive and the cost of a false negative. Then it will calculate and return the total_cost of your classification model.
 
The 'find_optimal_threshold' function takes in an unceRtain_object, and uses ROC to find the optimal threshold for classification.
 
The 'generate_visualizations' function takes in two arguments, an unceRtain_object and an optimal_threshold value. It then will produce a ROC curve, confusion matrix, and a ggplot version of the confusion matrix with labels. This can be used to asses how well the model is classifying and if the threshold needs to be adjusted.
 
The 'confusion_matrix_metrics_plot' takes in an unceRtain_object that will create a confusion matrix table and ggplot version.
 
The 'calculate_misclassification_cost' function takes in an unceRtain_object, the cost of false positive and the cost of false negative. It will calculate and return the missclassification cost.
 
The 'calculate_classification_metrics' function takes an unceRtain_object and calculates the TP, TN, FP, FN, and precision.
 

### Example Uses



## 

