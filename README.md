# Seattle-Crime

## Business Goal ##
We'll be working with the great city of Seattle. Using two different classification algorithms (Random Forest and CatBoost), we'll determine which algorithm most accurately classifies not only types of new crimes, but also location of new crimes. Creating a successful model can lead to more efficient crime identification, which can have numerous positive effects on the city such as more efficient police action, allocation of various resources to more affected areas, and overall reduced crime.

## Data ##
The data itself came from the city of Seattle's website. The crime data, which ranges from 2008-present is publicly available at https://data.seattle.gov/Public-Safety/SPD-Crime-Data-2008-Present/tazs-3rd5. The data contained well over 1 million rows of data, and 17 variables.

## Preprocessing Our Data ##
We started off preprocessing our data in a separate notebook (Preprocessing Data.ipynb). We preprocessed our model differently for two separate models: A crime type model, and a crime location model. We started off with our crime type model. We first checked for null values, duplicates, etc. After that we started converting our variables into categorical variables, while setting up the type of crime to be our target variable in our main notebook. We did something similar for our location model, except we set up our Beat to be the target variable. Lastly, we wanted to see how type of crime and crime location played a part together, so we created a new dataframe containing only Beat and crime type, and grouped them together for later use in our main notebook. 

## Our Models ##
We had two different models: One for crime type, and one for crime location. We used two different types of algorithms for our crime type model, which were a Random Forest classifier and a CatBoost classifier, but only used a CatBoost classifier for our location model. 

### Crime Type Model ###
We started off with our Random Forest classifier. We first used a default Random Forest model, but found it to be overfitting as well as having low precision, recall, and F1 scores. When we further analysed the model we found that our model seemed to best classify new crimes as Trespass of Real Property crimes. We then further tuned our intitial model with Grid-search, in an attempt to reduce overfitting and see if we could improve our classification report values. We found that we reduced overfitting and slightly increased recall values. This time, our model seemed to best classify new crimes as Identity Theft crimes. We then moved onto our CatBoost classifier. CatBoost classifier is a gradient boosting Decision Tree algorithm, that is mainly used when the data is comprised of categorical variables (like ours). It has great default model results, with little tuning needed, is great for reducing overfitting, and can even be trained on the GPU. We started off with a default CatBoost model, but implemented early stopping rounds to  to address overfitting. While still on the lower side, we already saw improvements in our precision, recall and F1 scores compared to our Random Forest best model. However there were still signs of overfitting, despite our use of early stopping rounds. Our model also best classified two types of crimes this time (instead of just one), which were Simple Assault and Identity Theft crimes. We tuned further using Grid-search again. Our tuned model did not perform better than our initial model. In fact our initial model had better classification report values and higher instances of confusion matrix values (like true positives). We therefore declared our initial CatBoost model to be the best model overall for crime type classification. Due to there being a large volume of data, we'll only display the results from our best model, but our other model's results can be seen in our main notebook. Our classification report and confusion matrix results for our best model can be seen below. 

**Classification Report**

<img width="505" alt="best_crime_type_cr" src="https://user-images.githubusercontent.com/45251340/230743373-16f0e96c-27ba-42db-9487-ad3dbd91ab9e.PNG">


**Confusion Matrix**

![best_crime_type_cm](https://user-images.githubusercontent.com/45251340/230743456-cd4b3d0f-d62e-4874-9474-6dbdd7699bc7.PNG)



