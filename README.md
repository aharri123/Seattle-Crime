# Seattle-Crime

## Business Goal ##
We'll be working with the great city of Seattle. Using two different classification algorithms (Random Forest and CatBoost), we'll determine which algorithm most accurately classifies not only types of new crimes, but also location of new crimes. Creating a successful model can lead to more efficient crime identification, which can have numerous positive effects on the city such as more efficient police action, allocation of various resources to more affected areas, and overall reduced crime.

## Data ##
The data itself came from the city of Seattle's website. The crime data, which ranges from 2008-present is publicly available at https://data.seattle.gov/Public-Safety/SPD-Crime-Data-2008-Present/tazs-3rd5. The data contained well over 1 million rows of data, and 17 variables.

## Preprocessing Our Data ##
We started off preprocessing our data in a separate notebook (Preprocessing Data.ipynb). We preprocessed our model differently for two separate models: A crime type model, and a crime location model. We started off with our crime type model. We first checked for null values, duplicates, etc. After that we started converting our variables into categorical variables, while setting up the type of crime to be our target variable in our main notebook. We did something similar for our location model, except we set up our Beat to be the target variable. Lastly, we wanted to see how type of crime and crime location played a part together, so we created a new dataframe containing only Beat and crime type, and grouped them together for later use in our main notebook. 
