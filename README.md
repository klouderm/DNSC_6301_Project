# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Kylie Loudermilk, `kylieloudermilk@gwmail.gwu.edu`
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Project.ipynb](https://colab.research.google.com/drive/1rfbg5pgdPFgBpZAr_m_vtZKSFy7kaaU3?usp=sharing)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: Scikit learn train_test_split() function used to partition data. Conducted two data partitions in order to create three datasets. 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
       
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier('ccp_alpha': 0.0, 'class_weight': None, 
                       'criterion': 'gini', 'max_depth': 6, 
                       'max_features': None, 'max_leaf_nodes': None, 
                       'min_impurity_decrease': 0.0, 
                       'min_samples_leaf': 1, 'min_samples_split': 2, 
                       'min_weight_fraction_leaf': 0.0, 
                       'random_state': 12345, 'splitter': 'best')
```
### Quantitative Analysis

* **Training AUC (Depth 6)**: 0.7837
* **Validation AUC (Depth 6)**: 0.7496
* **Test AUC(Depth 6)**: 0.7438

* **AIR Metrics (Calculated with validation data):**

    | **Protected Group** | **Control Group** | **AIR**|
    | ---- | ------------- | ---------------- |
    |Hispanic| White | 0.83 |
    | Black | White | 0.85 |
    | Asian | White | 1.00 |
    | Female | Male | 1.02 |
#### Correlation Heatmap
![Correlation Heatmap](heatmap.png)

This heatmap shows the largest positive correlation between the target 'DELINQ_NEXT' and another variable is with 'PAY_0' and
the largest negative correlation between the target 'DELINQ_NEXT' and another variable is with 'RACE'.

#### Final Iteration Plot
![Iteration Plot](final_iteration_plot.png)

The final iteration plot shows that the most ideal balance between the AIR and AUC values provided by this model exists at a tree depth of 6.

#### Ethical Considerations
* **Potential Negative Impacts**
    * Historic social biases teach us that financial data is encoded with racial bias. Even when we test for bias and utilize those results to select an         iteration of the model that performs at an industry standard level of fair, we are accepting that this model will predict that hispanic loan               applicants will be less likely to be qualified for approval than white applicants. By using predictions that are less likely to suggest hispanic           applicants as recipients of loans, we risk hurting this demographic and perpetuating the cycle of bias.

* **Potential Uncertainties**
    * This model has only been tested in the Google Colab environment, further testing would be needed to determine if the results would be affected
      by a change in development environment or hardware.
    * This model was developed and tested in a lab environment, and we can not guarantee to know how the model will perform in real world scenarios.             Therefore, the model's behavior and results must be closely monitored after production deployment.
