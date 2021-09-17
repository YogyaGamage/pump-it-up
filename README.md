# Data Visualization

First we will explore each of the attributes in the dataset and figure out  the value ranges and unique value types.
-   `amount_tsh`  - Total amount water available to waterpoint.
	This is a numerical attribute and possibly contributes to the status of the pump.
-   `date_recorded`  - The date the row was entered. This field is not expected to have a relationship with the pump status.
-   `funder`  - Who funded the well. This is categorical attribute with 1897 unique values and there is possibility that this is correlated with the label which we will discover later.
-   `gps_height`  - Altitude of the well. This is a numerical attribute and this also could influence the output label.
-   `installer`  - Organization that installed the well. A bit similar to funder this also a categorical variable with 2145 unique values.
-   `longitude`  - GPS coordinate. 
-   `latitude`  - GPS coordinate. Longitude and latitude also could influence the pump status and we will visualize it in the next section. 
-   `wpt_name`  - Name of the waterpoint if there is one. This is a categorical variable but there are many unique values.
-   `num_private`  - This is a numerical variable.
-   `basin`  - Geographic water basin. A categorical variable with 9 unique values.
-   `subvillage`  - Geographic location. This is a categorical variable with many unique values.
-   `region`  - Geographic location. This is also a categorical variable and we can see that region is kind of like a higher level or similar to sub village, lga and ward.
-   `region_code`  - Geographic location (coded) 
-   `district_code`  - Geographic location (coded)
-   `lga`  - Geographic location
-   `ward`  - Geographic location
-   `population`  - Population around the well. This is a numerical attribute.
-   `public_meeting`  - True/False. This is boolean value and we will later see if this is correlated with the the target.
-   `recorded_by`  - Group entering this row of data. This is again not expected to have a relationship with the pump status.
-   `scheme_management`  - Who operates the waterpoint. A categorical variable with 14 unique values.
-   `scheme_name`  - Who operates the waterpoint. Similar to scheme management.
-   `permit`  - If the waterpoint is permitted. This is a boolean value and we will later see if this is correlated with the the target.
-   `construction_year`  - Year the waterpoint was constructed. This ranges from around 1950 to 2015.
-   `extraction_type`  - The kind of extraction the waterpoint uses
-   `extraction_type_group`  - The kind of extraction the waterpoint uses. A categorical variable with 13 unique values.
-   `extraction_type_class`  - The kind of extraction the waterpoint uses. This is higher level of extraction type and group with 7 unique values.
-   `management`  - How the waterpoint is managed. 12 unique values.
-   `management_group`  - How the waterpoint is managed. 5 unique values and an a higher level of management.
-   `payment`  - What the water costs. A categorical variable with 7 unique values.
-   `payment_type`  - What the water costs. Redundant attribute as this is same as payment.
-   `water_quality`  - The quality of the water. A categorical variable with 8 unique values.
-   `quality_group`  - The quality of the water. A categorical variable with 6 unique values.
-   `quantity`  - The quantity of water. A categorical variable with 5 unique values.
-   `quantity_group`  - The quantity of water. Redundant attribute as this is same as quantity.
-   `source`  - The source of the water. A categorical variable with 10 unique values.
-   `source_type`  - The source of the water. A categorical variable with 6 unique values. Almost as same as source but with a few values in source are joined here as one category.
-   `source_class`  - The source of the water. A categorical variable with 3 unique values. A higher level of source and source type.
-   `waterpoint_type`  - The kind of waterpoint. A categorical variable with 7 unique values. 
-   `waterpoint_type_group`  - The kind of waterpoint. The only difference with waterpoint_type is that, communal standpipe multiple and communal standpipe is joined here as communal standpipe.

## Identifying Relationships

In this section we will visualize correlations with the target variable and other attributes and decide what are the best features. 

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image0.png)

We can see that there is a difference in communal standpipe and CSPipe multiple. So we will use waterpoint type.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image1.png)

Source seems to show a similar pattern in all levels.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image2.png)

We will use quantity group as quantity and quantity group are same.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image3.png)

We will use "quality group" instead of "water quality" as abandoned type data are rare.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image4.png)

We will use payment type.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image5.png)

We will use management group as the distribution does not show much difference when grouped or not.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image6.png)

We will use extraction type class, because except for afridev other subclasses did not show significant difference with the parent class distribution.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image7.png)

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image8.png)

We can see that permit and public meeting has some relationship with the pump status.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image9.png)

Region seems to be the best attribute to use among the other location based attributes.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image10.png)

More non functional and need repair pipes appear to be among the older pipes.

 ![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image11.png)

The GPS coordinates also show a good correlation to the target variable.

# Handling Missing Data

The following are the columns with missing values.

|                |Total                          |%                         |
|----------------|---------------------|----------|
|scheme_name     |     28166     |    47.4       |
|scheme_management |      3877     |   6.5    |
|installer         |3655|6.2|  
|funder         |3635|6.1| 
|public_meeting         |3334|5.6| 
|permit         |3056|5.1| 
|subvillage         |371|0.6| 

We will impute public_meeting, permit and scheme_management with the most popular value. At the initial step we will imput funder and installer as "none".  We are not going to use the scheme_name and subvillage columns for reasons mentioned earlier, so we will not impute them.

# Transforming Data

We will encode categorical columns using the label encoder and we will scale the numerical columns using min max scalar.

For constructed year we will categorize it into four categories based on the results we got from the correlation graphs. 
>year<1990
>1990<year<2000
>2000<year<2006
>2006<year

For the installer and funder we will replace the values that have less than 100 rows as "other".

We will then remove unused columns. 

# The Model

We will experiment with a few models such as XGBoost and different Ensembled models.
For the XGBoost classifier we will use the default parameters. 
For the ensembled model we will try with combinations of different layers such as 
>XGBoost classifier
>AdaBoost classifier
>CatBoost classifier
>RandomForest classifier
>MLP classifier

As the final estimator logistic regression is used in the stacking classifier-that is the ensemble model.

# Results

The XGBoost model gave a score of 0.7270.

The ensemble models performed better giving more than 0.76.
As adding more layers did not improve the results significantly only XGBoost and CatBoost models are selected. 

For the feature set a few updates also gave better results. The construction year was used as it is rather than classifying it as it improved the results. The final achieved result was 0.7877

# Final Version

In the final version, the model, the parameters and the selected features were,

In the ensemble model,
>**XGBoost** n_estimators=251, max_depth=4, learning_rate=0.1, colsample_bytree=0.2,reg_lambda=1, silent=False
>**CatBoost** learning_rate=0.1
>**LogisticRegression** penalty=l2

The selected feature set was,
>amount_tsh, gps_height, longitude, latitude, num_private, basin, region, population, public_meeting, scheme_management, permit, construction_year, extraction_type_class, management_group, payment_type, quality_group, quantity_group, source_class, waterpoint_type

The final maximum score was 0.7877.

![alt text](https://github.com/YogyaGamage/pump-it-up/blob/main/images/image12.png)
