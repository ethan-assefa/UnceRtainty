# UnceRtainty

*Created by Ethan Assefa, Thomas Burrell, Tatev Gomtsyan*

<p align="center">
  <img src="https://github.com/ethan-assefa/UnceRtainty/blob/main/PackageLogo.png?raw=true" alt="Package Logo" width="350"/>
</p>

UnceRtainty is a package for helping evaluate logistic models for uncertainty and classification performance. The sorting hat, inspired by Harry Potter, symbolizes classification models' decision-making, "sorting" nature. 

## Purpose
The `UnceRtainty` package provides evaluation of logistic models through a comprehensive suite of tools for assessing both uncertainty and classification performance. This package serves as a valuable resource for researchers, data scientists, and analysts working with logistic regression models. By offering a range of functions and visualizations, users can efficiently explore model uncertainties, assess the reliability of predictions, and gain insights into the overall performance of their logistic models. The package is designed to enhance the model evaluation process by offering a user-friendly interface, robust statistical metrics, and informative graphical outputs, empowering users to make informed decisions about model selection, calibration, and deployment. For users focused on binary classification tasks, this package aims to be a versatile and indispensable tool for evaluating logistic models in a rigorous and efficient manner.

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

### Running the logistic regression model on the training data
Now we are able to run our logistic regression model below:

```r
# Creates a logisitic regression model
logistic_model <- glm(am ~ qsec + wt + gear, data = train_data, family = "binomial")
```

With this we have all three elements needed to work in the `UnceRtainty` package.

### The `unceRtain_object`
All functions in the `UnceRtainty` package work with an `unceRtain_object` that consists of two parts. 

1. The `predictions` generated by the model. These are the probabilities (numeric value between 0 and 1) produced by the model for the observations it is being tested on in the test set.
2. The `observed_outcomes` from the test set, the list of binary outcomes (all 0s or 1s) that are the actual observed outcomes in the test set.

The can be created using the function `MakePredictions()` and the three elements from earlier. We can see how the code works below using the same elements we created earlier:

```r
# Creates unceRtain_object
uncertain_object <- MakePredictions(model = logistic_model, test_data = test_data, observed_outcome = test_ys)

# Contains the predicted probabilities
uncertain_object$predictions

# Contains the actual observations
uncertain_object$observed_outcomes
```

Having successfully created the `unceRtain_object`, you can now implement this object as the input for a suite of functions that provide additional information about the classification performance.

## Package Functions
The package functions are shown below:

### Calculate Expected Prediction Error (EPE)
The `calculate_EPE()` function takes in an unceRtain_object and finds the mean absolute value difference between the predictions and the true values. A smaller value means your predictions we're close to the true values, while large values mean you are further away. The values will be data/context dependent.

```r
# Calculates EPE
calculate_EPE(unceRtain_object)
```

### Cost of False Positive and False Negative
The `set_cost()` function takes in three arguments, the unceRtain_object previously created, the cost of a false positive, and the cost of a false negative. Then it will calculate and return the total cost of your classification model.

```r
# Calculates total cost
set_cost(unceRtain_object, cfp = 1, cfn = 2)
```

### Find Optimal Threshold for Classification
The `find_optimal_threshold()` function takes in an unceRtain_object, and uses ROC to find the optimal threshold for classification.

```r
# Determines optimal threshold level
find_optimal_threshold(unceRtain_object)
```

### Generate ROC Curve
This `generate_roc()` function generates the Receiver Operating Characteristic (ROC) curve, providing insights into the binary classification model's performance across the range of possible thresholds.

```r
# Plots ROC curve visial 
generate_roc(unceRtain_object)
```

### Confusion Matrix Metrics
The `confusion_matrix_metrics_plot()` function takes in an unceRtain_object that will create a confusion matrix table and ggplot version.

```r
# Creates visual of confusion matrix
confusion_matrix_metrics_plot(unceRtain_object)
```

### Calculate Mis-Classification Cost and Misclassification Error
The `calculate_misclassification_cost()` function takes in an unceRtain_object, the cost of false positive, and the cost of false negative. It will calculate and return the missclassification cost.

```r
# Calculates mis-classification cost
calculate_misclassification_cost(unceRtain_object, cfp = 1, cfn = 2)
```

### Calculate Classification Metrics
The `calculate_classification_metrics()` function takes an unceRtain_object and calculates the True Positive Rate (TPR), False Positive Rate (FPR), and Precision for a binary classification model.

```r
# Provides a range of classification metrics
calculate_classification_metrics(unceRtain_object)
```

