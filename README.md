# Cyclistic Case Study
Capstone project for Google Data Analytics Certificate.

## The Problem
![bike_sharing](/images/bike_sharing.jpeg)

## Backgroud Information

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

## The Problem

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, the executive team believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, there is a very good chance to convert casual riders into members with targeted marketing.

### The Objective
- To better understand how annual members and casual riders differ.

- To determine why casual riders would buy a membership.

The insights generated from this analysis would allow Cyclistic to design marketing strategies aimed at converting casual riders into annual members.

### Features and Target

#### Client data:
1 - age (numeric) <br>
2 - job : type of job (categorical: 'admin.', 'blue-collar', 'entrepreneur', 'housemaid', 'management', 'retired', 'self employed', 'services', 'student', 'technician', 'unemployed', 'unknown') <br>
3 - marital : marital status (categorical: 'divorced', 'married', 'single', 'unknown'; note: 'divorced' means divorced or widowed) <br>
4 - education (categorical: 'basic.4y', 'basic.6y', 'basic.9y', 'high.school', 'illiterate', 'professional.course', 'university.degree', 'unknown') <br>
5 - default: has credit in default? (categorical: 'no', 'yes', 'unknown') <br>
6 - balance: average yearly balance (numeric) <br>
7 - housing: has housing loan? (categorical: 'no', 'yes', 'unknown') <br>
8 - loan: has personal loan? (categorical: 'no', 'yes', 'unknown') <br>

#### Campaign data:
9 - contact: contact communication type (categorical: 'cellular','telephone') <br>
10 - day: last contact day of month (numeric) <br>
11 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec') <br>
12 - duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model. <br>
13 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact) <br>
14 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; -1 means client was not previously contacted) <br>
15 - previous: number of contacts performed before this campaign and for this client (numeric) <br>
16 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success') <br>


#### Target variable:
17 - deposit: has the client subscribed a term deposit? (binary: 'yes','no')

## EDA

### Correlation with target
![Correlation Heatmap](/Images/corr_heatmap.png)

Contact duration is highly correlated witht the subscription outcome.

### Kernel Density Estimate plot of clients' age
![Age KDE](/Images/EDA_age_KDE.png)

There is a higher proportion of clients below 30 years old and above 60 years old that subscribed for a term deposit. Most of the clients are beyween 30 to 40 years old.

### Distribution of contact duration
![Contact Duration](/Images/EDA_duration.png)

Clients with a longer duration of contact generally ended up subscribing for a term deposit.
If we assume 5 mins is the minimum amount of time for a telemarketer to convince a client, out of those that were contacted for a tleast 5 mins:
- 27.8% of them did not subscribe
- 72.2% of them subscribed

### Subscription outcome based on month of contact
![Month](/Images/EDA_month.png)

More than **60%** of the clients were contacted from May to August but the clients tend to not subscribe for a term deposit during this period.
Months above average: 'apr', 'dec', 'mar', 'oct', 'sep'
Months below average: 'aug', 'feb', 'jan', 'jul', 'jun', 'may', 'nov'

### Subscription outcome based on job type
![Job](/Images/EDA_job.png)

Most of the bank's clients are working in management and slightly more than half of them subscribed for a term deposit.
Clients that are students and retirees have a higher chance of subscribing for a term deposit.

## Data pre-processing

### Transformation pipeline
```python
def transform(df):
    """
    Prep the train and test set for machine learning.
        - encoding categorical variables with one-hot encoding
        - convert days and months (cyclical features) into a proper representaion
    
    Input: dataframe
    Output: transformed dataframe
    
    """
    
    # convert categorical variables
    df = pd.get_dummies(df, columns = cat_var)
    
    df = df.replace({'month_of_year' : month_dict})
    
    # convert cyclical features
    df['day_sin'] = np.sin(df.day_of_month*(2.*np.pi/24))
    df['day_cos'] = np.cos(df.day_of_month*(2.*np.pi/24))
    df['month_sin'] = np.sin((df.month_of_year-1)*(2.*np.pi/12))
    df['month_cos'] = np.cos((df.month_of_year-1)*(2.*np.pi/12))

    df.drop(columns = ['day_of_month', 'month_of_year'], inplace = True)
    
    # normalize variables
    scaled_norm_var = df[norm_var]
    scaler = StandardScaler().fit(scaled_norm_var.values)
    scaled_norm_var = scaler.transform(scaled_norm_var.values)
    df[norm_var] = scaled_norm_var
    
    return df
```

The above function will allow us to easily transform the test set for prediction.

## Modelling

### Base model - Logistic Regression
F1 score - 0.790

### Random Forest Classifier
Mean F1 score from 5-fold cross validation - 0.849

### Gradient Boosting Classifier
Mean F1 score from 5-fold cross validation - 0.841

## Feature Importance

### Top 15 features
![Feature Importance](/Images/feature_importance.png)

Cyclical features are better represented in terms of sines and cosines.

## Predictions for test set with Random Forest Classifier

F1 score - 0.845
The model is able to generalise well to new data, getting a similar score as for the training set.

### Confusion Matrix

![Confusion Matrix](/Images/confusion_matrix.png)

The model performs better at predicting for clients that subscribed for a term deposit.

For this case, a false negative is more costly than a false positive. 
- A false negative would mean a client is more likely to subscribe for a term deposit but no contact was made from the bank (missed out on potential revenue). 
- A false positive would mean that the bank made contacts but the client is more likely to not subscribe for a term deposit (waste of marketing effort and resources).

## Possible Solutions

![Telemarketing](/Images/telemarketing.jpg)

```python
rf.predict_proba(test_set_prep)
```

array([[0.43, 0.57], <br>
       [0.89, 0.11], <br>
       [0.07, 0.93], <br>
       ..., <br>
       [0.78, 0.22], <br>
       [0.95, 0.05], <br>
       [0.97, 0.03]])

We can use the probabilities to classify the clients based on their likelihood of subscribing for a term deposit.

    - >= 0.6, clients to target
    - >= 0.4 < 0.6, to monitor and get additional data (no. of dependents, annual income, term deposit at other banks etc)
    - < 0.4, to market different services (credit card, loans etc)
    
For clients to target, as the contact duration is the most important feature, it is recommended to have a longer contact duration to convince the client.

It was also determined that more than 80% of clients that subscribed for a term deposit before opted to subscribe for another term. The bank could introduce some form of **loyalty marketing** (e.g. higher interest rates for second and subsequent term deposits).

As for students and retirees, the bank could come up with a special plan to attract more clients from these group. <br>
For example, the bank could tie up with schools or reduce the minimum deposit amount to allow more students to be able to commit. 

For retirees, the bank could come up with a 'Deposit-Retirement-Account' (a term deposit specific for their retirement needs. They may also tie up with insurance services companies to further enhance their Deposit-Retirmenet schemes.

More than **60%** of the clients were contacted during May to August but clients tend to not subscribe for a term deposit during this period. The bank could look into this in detail and find out if there is any underlying reason (one possible reason is that the notice of assesment for income tax is usually out during this period and clients would prefer to set aside some money to pay off their taxes).

## Example Scenario
Consider the following example. Each call made by the bank costs \\$5 and the bank has a marketing budget of \\$500,000. <br> Assume also that from each subscription, the bank generates \\$100 in revenue and based on the dataset about 47\% of calls result in a subscription.

So the bank would get 4700 subscriptions from 10000 calls, getting a profit of **$420,000**.

The precision of the model on the test set is **0.81** (949 / (949 + 217)). <br>
If the bank made the same amount of calls (10000), it could gain more than 8000 subscriptions. This would result in profits of **$750,000**, almost 79\% increase!

Another scenario could be when the bank have a target number of subscriptions (say 6000) to achieve. Without predictive modelling, the bank would need to make about 12765 calls compared to 7407 calls with it. This results in direct savings on marketing costs of about \$26,790.
