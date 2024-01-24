# Practical Application III -- Comparing Classifers for Bank Marketing

## Business Understanding

### Target User
THis nmodeling exercise is desinged for teams runnning targetted marketing campainges.  The models are built using data from a retail bank in Portugual so the results are specific to them, but the approach coudl be applied to a number of B2B and B2C businesses that are trying to improve the effectiveness and efficiency of theri direct marketing efforts.

### Current scenario
There are two approaches to marketing services: mass marketing and targeted marketing.  While inexpensive, mass marketing has a low engagement rate and may not fit with a company's brand/image.  Targetted marketing is more effective, but it is also more expensive becasue of the need for call center personel and the costs of contact acquistion. Current regulatory and business pressures put new emphasis on selling new banking services and increasing deposits.

### Problem Statement
Thbe problem statement is to use data mining techniques to analyse the results of prio campaings to uncover patterns that will make the targetted campaigns more efficient.  

### Success Metrics
The goal is to reduce the number of contacts while keeping the success rate (new deposits or new services sold) the same.  The result shold be a measurable increase in efficiency over the baseline established from prio campaigns, wiht the ultimate measure being a net increase in deposits.

## Data Understanding

### Data Source
The dataset can be found at the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/222/bank+marketing). It contains 41,188 records aggregated from 17 prior marekting campaigns. 

### Overview of the dataset

#### Bank client data:
+ 1 - age (numeric)
+ 2 - job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-employed','services','student','technician','unemployed','unknown')
+ 3 - marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)
+ 4 - education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')
+ 5 - default: has credit in default? (categorical: 'no','yes','unknown')
+ 6 - housing: has housing loan? (categorical: 'no','yes','unknown')
+ 7 - loan: has personal loan? (categorical: 'no','yes','unknown')

#### Related with the last contact of the current campaign:
+ 8 - contact: contact communication type (categorical: 'cellular','telephone')
+ 9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')
+ 10 - day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')
+ 11 - duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.

#### Other attributes:
+ 12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
+ 13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)
+ 14 - previous: number of contacts performed before this campaign and for this client (numeric)
+ 15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')

#### Social and economic context attributes
+ 16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)
+ 17 - cons.price.idx: consumer price index - monthly indicator (numeric)
+ 18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)
+ 19 - euribor3m: euribor 3 month rate - daily indicator (numeric)
+ 20 - nr.employed: number of employees - quarterly indicator (numeric)

#### Output variable (desired target):
+ 21 - y - has the client subscribed a term deposit? (binary: 'yes','no')

### EDA

There are no issues with missing or incomplete data. Howver, the dataset has a class imbalance with regards to the the target variable with an 89%/11% split between 'no' and 'yes'. Per the constraints of the project, the focus in on the first 7 features.

<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/bee6dccd-8051-4b34-b2aa-483390f6bea6">
<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/a7816b59-c70a-424f-8b5f-dbf4f18c191f">
<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/d964b09e-3d4f-4b01-9c14-6b1eb82bf632">
<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/9804cc5d-e5be-4e1c-bc32-e851da3d9222">
<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/8ae0eaa6-38c3-4d01-a30d-ef12ccd27ff9">
<img width="851" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/c37e2dfe-ce20-4356-a11b-e2c3dfd16b4c">

Looking for intersting features, we can see the patterns in *job* (admin, technician, and blue-collar), *education* (university degree), not being in *default* and not having a *loan*.  *Housing* and *marital* status don't appear to have much differentiation, so not useful for prediction.

### Preperation
Preperation was pretty straightforward.  The dataset was cut down to the first seven features, the catagorical features were encoded and the target column was renames and converted into a numeric format. The data was then split into training and testing sets.

Baseed on the EDA analysis a second samller dataset and split was created which focused on *job*, *education* and *loan*.

## Modeling
Modeling was initially done on the second smaller data set and started with using a Dummy Classifier to establish a basline, the the followoing classifiers were fit using default parameters and scored:
+ Logical Regression
+ K-Nearest Neighbor
+ Support Vector Machines
+ Decision Tree

### Results and recommendations
The following table represents the sumamry results of the four classifiers:

<img width="512" alt="image" src="https://github.com/omarsultan/Practical_App_3/assets/6629296/aff32a8e-5148-45c1-85ac-09c7a08393c0">

While SVM and Logistic Regression have the highest test accuracy, they are also the most compute intensive. A better alternative might be the Decision Tree, which has almost as high test accuracy at significantly lower compute cost. That would be my recommendation as you give up very little in accuracy but gain significant, on-going operational savings.

### Improving the model

A number of attempts were made to improve the Decision Tree Model:
+ Running the model agaisnt the dataset wiht all 7 features yielded higher traing accuracy (91.697) but lower test accuracy (86.433) which likely means it is overfit to the data
+ The default parameters fielded a tree with a depth of 34.  Reducing the depth did not have a marked impact but it did further reduce compute cost.  With `max_depth=3`, test accuracy was 88.764 but fit time was further reduced to 0.02632.

### Next steps
+ Address the class imbalance to see if that improves model performance
+ Use sequential feature selection to see if the is some sweet spot between the 4-feature and 7-feature dataset that improves model performance
+ Add features beyond the 7 in the initial constraints.








