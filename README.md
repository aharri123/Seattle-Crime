# Classifying Seattle-Crime

![Seattle_Center_as_night_falls](https://user-images.githubusercontent.com/45251340/231828052-f6a2f8a6-ef82-4bfa-a613-3ae9bf302ec6.jpg)


## Business Goal ##
We'll be working with the great city of Seattle and its mayor. Seattle is a large, ever growing city, and a major problem is the crime situation. The mayor wants to find ways to reduce the crime in Seattle, but needs a way to predict crime to do so. Looking at previous crimes, and using machine learning classification models (Random Forest and CatBoost), we'll create two separate models to predict not only type of new crimes, but also location of new crimes. Predicting the type of a crime, will allow us to determine not only what kind of responsive action is needed at the moment, but also what kind of preventative measures can be taken, once we can identify trends in crime. Predicting the location of a crime will allow for much faster (and potentially even instantaneous) responsive action, and also can help us determine if we need to redirect or create new police forces throughout Seattle. Creating separate models is a great start, but our ultimate goal is to combine the two models into one singular model that can predict both crime type and location. Once proven successful, the model(s) can potentially be generalized to other parts of Washington, and even other states.

## Data ##
The data itself came from the city of Seattle's website. The crime dataset, which ranges from 2008-present is publicly available at https://data.seattle.gov/Public-Safety/SPD-Crime-Data-2008-Present/tazs-3rd5. The data contained well over 1 million rows of data, and 17 variables. Our initial variables (before filtering and preprocessing our data) can be seen below.


<img width="291" alt="initial df info" src="https://user-images.githubusercontent.com/45251340/235370148-2e0db823-3d04-42f8-91ff-a6d92381ced9.PNG">


## Preprocessing Our Data ##
We started off preprocessing our data in a separate notebook (Preprocessing Data.ipynb). We preprocessed our model differently for two separate models: A crime type model, and a crime location model. We started off with our crime type model. We first checked for null values, duplicates, etc. After that, we started converting our variables into categorical variables, while setting up the type of crime to be our target variable in our main notebook. At the end, we were left with 8 variables and 210,259 rows.


<img width="246" alt="crime type final df" src="https://user-images.githubusercontent.com/45251340/235370427-05d900b4-1f92-48ca-93e6-b86a393bef55.PNG">


We did something similar for our location model, except we set up our Beat to be the target variable. 

<img width="247" alt="crime location final df" src="https://user-images.githubusercontent.com/45251340/235370471-c5433de8-a989-4f12-adc3-ef5fff5a4920.PNG">


Lastly, we wanted to see how type of crime and crime location played a part together, so we created a new dataframe containing only Beat and crime type, and grouped them together for later use in our main notebook. 


<img width="167" alt="supp_analysis df" src="https://user-images.githubusercontent.com/45251340/235370545-5adf870a-0f29-44f8-a5e2-86303044fa08.PNG">

## Distribution Of Our Variables ##

Let's look at the distribution for our target variables. We also graphed the distribution of our other variables, which can be seen in our main notebook.

**Crime Type Target Variable (Offense) Distribution**

<img width="529" alt="offense target var distribution" src="https://user-images.githubusercontent.com/45251340/235370760-b1f50bd0-9a7d-4007-8b57-04d7c8e5a026.PNG">

**Crime Location Target Variable (Beat) Distribution**

<img width="487" alt="beat target var distribution" src="https://user-images.githubusercontent.com/45251340/235371919-7444dd39-86ae-4d70-a325-a249d0b63e99.PNG">

We can see that there's imbalances in both of our variables, which we address using SMOTE in our main notebook.


## Our Models ##
We had two different models: One for crime type, and one for crime location. We used two different types of algorithms for our crime type model, which were a Random Forest classifier and a CatBoost classifier, but only used a CatBoost classifier for our location model. 

### 1) Crime Type Model ###
We started off with our Random Forest classifier. We first used a default Random Forest model, but found it to be overfitting as well as having low precision, recall, and F1 scores. When we further analysed the model we found that our model seemed to best classify new crimes as Trespass of Real Property crimes. We then further tuned our intitial model with Grid-search, in an attempt to reduce overfitting and see if we could improve our classification report values. We found that we reduced overfitting and slightly increased recall values. This time, our model seemed to best classify new crimes as Identity Theft crimes. 

We then moved onto our CatBoost classifier. CatBoost classifier is a gradient boosting Decision Tree algorithm, that is mainly used when the data is comprised of categorical variables (like ours). It has great default model results, with little tuning needed, is great for reducing overfitting, and can even be trained on the GPU. We started off with a default CatBoost model, but implemented early stopping rounds to  to address overfitting. While still on the lower side, we already saw improvements in our precision, recall and F1 scores compared to our Random Forest best model. However there were still signs of overfitting, despite our use of early stopping rounds. Our model also best classified two types of crimes this time (instead of just one), which were Simple Assault and Identity Theft crimes. We tuned further using Grid-search again. Our tuned model did not perform better than our initial model. In fact our initial model had better classification report values and higher instances of confusion matrix values (like true positives). 

We therefore declared our initial CatBoost model to be the best model overall for crime type classification (compared to our Random Forest and tuned CatBoost models). Due to there being a large volume of data, we'll only display the results from our best model, but our other model's results can be seen in our main notebook. Our classification report and confusion matrix results for our best model can be seen below. 

**Classification Report**

<img width="505" alt="best_crime_type_cr" src="https://user-images.githubusercontent.com/45251340/230743373-16f0e96c-27ba-42db-9487-ad3dbd91ab9e.PNG">


**Confusion Matrix**

![best_crime_type_cm](https://user-images.githubusercontent.com/45251340/230743456-cd4b3d0f-d62e-4874-9474-6dbdd7699bc7.PNG)



**From our best crime type model we can see:**
* Our highest precision score belonged to Simple Assault crimes (.47), then both Aggravated Assault crimes and Identity Theft crimes with a score of .36. This tells us that out of all the crimes that the model predicted would be a certain type of crime, the largest percentage of crimes that were actually that type belonged to Simple Assault crimes, then Aggravated Assault crimes, then Identity Theft crimes. Our precision values could be much higher, but keep in mind these are initial models, and there's room for tuning/improvements.
* Our highest recall score belonged to Identity Theft crimes (.73), then Shoplifting crimes (.62), then Simple Assault crimes (.56). This tells us that when looking at the total actual instances of a type of crime, the largest percentage of our predictions were correctly classified as that crime when it came to Identity Theft crimes, then shoplifting crimes, then Simple Assault crimes. Our recall values could be higher, but keep in mind these are initial models, and there's room for tuning/improvements.
* Our highest F1 score belonged to Simple Assault crimes (.51), then Identity Theft crimes (.48), then Shoplifting crimes (.42). This tells us that our model's accuracy is best when it comes to Simple Assault crimes, then Identity Theft crimes, then Shoplifting crimes. Our F1 scores could be much higher, but again these are initial models, and there's room for tuning/improvements.
* For our instances of true positives, the most instances belonged to Burglary/Breaking & Entering crimes (2,474), then Simple Assault crimes (2,139), then Identity Theft crimes (2,118). We can see that we have high instances of True Positives, which is good since it means our models are classifying a large amount of crimes correctly. 
* The least instances of false negatives belonged to Trespass of Real Property crimes (0), Identity Theft crimes (803), and Shoplifting crimes (1,116). Low instances of false negatives are good since high instances of false negatives could mean that the wrong police action is taken, or even that someone could get charged for the wrong crime. The least misclassified crimes were Trespass of Real Property, Identity Theft, and Shoplifting crimes.
* The least instances of false postives belonged to Trespass of Real Property crimes (0), Robbery crimes (826), and Theft From Building crimes (1,309). Low instances of false positives are good for the same reason as with false negatives. This means that crimes were least likely to be misclassified as Trespass of Real Property, Robbery, and Theft From Building crimes.
* For our instances of true negatives, the most instances belonged to Theft From Building crimes (51,929), then Theft of Motor Vehicle Parts or Accessories crimes (51,863), then Robbery crimes (51,679). Low instances of true negatives are good, but this metric is not as useful as the other 3.

**We'll say in terms of precision, recall, F1 score, and instances of true positives/ false negatives that our top crimes are Simple Assault and Shoplifting.**


### 2) Crime Location Model ###

We also looked at location of crimes in a separate model. Seattle has 5 precincts, or police station areas. They are: North, East, South, West and Southwest. Then, there are smaller geographical areas within the precincts called sectors. Finally, each sector is divided into 3 smaller sections called beats, which individual patrol officers are assigned responsibility for. We'll be looking at which beats crimes occur in, since beats are the smallest area we can look at. Shown below is a  beat map for reference when looking at our later results, and also just to gain an idea of how the beats are spread out. 

![precinctmap](https://user-images.githubusercontent.com/45251340/230751410-ce588931-d4c1-4dbf-8208-d232c6271c94.png)


We first used a default CatBoost model (again with early stopping rounds), and found that our precision, recall and F1 values were relatively high. That being said, are model was overfitting. Our initial model seemed to best classify new crime locations as occuring in the E1, E3, and L2 beats. We wanted to see if we could reduce overfitting and get more realistic classification report values, so we tuned our model again using Grid-search. Interestingly, our classification report results were the same as our initial model. We also found that our model was still overfitting. In addition, our confusion matrix results were slightly lower when it came to metrics like false positives and false negatives, while higher for metrics like true negatives, when compared to our tuned model. 

Therefore, we declared our initial CatBoost model as the best model between the two. Let's take a look at our classification report and confusion matrix.

**Classification Report**

<img width="327" alt="best_location_model_cr" src="https://user-images.githubusercontent.com/45251340/230751792-d21b8001-86a8-47b6-805d-b31921255f37.PNG">

**Confusion Matrix**

![best_crime_type_cm](https://user-images.githubusercontent.com/45251340/230751798-952b8533-cb04-49fa-afc1-4a8f50e633ff.PNG)


**From our best crime type model we can see:**
* Our highest precision scores belonged to the E3 beat (.99), the B3 beat (.96), and the U3 beat (.89)
* Our highest recall values belonged to the E1 beat (.99), the B1 beat (.95), and the U2 beat (.90)
* Our highest F1 score belonged to the E1 beat (.87), the B3 beat (.84), and the E3 beat (.83)
* For our instances of true positives, the most instances belonged to the K3 beat (1,748) , then the L2 beat (1,604), then the R2 beat (1,601)
* The least instances of false negatives was a tie between the K3 beat, L2 beat, R2 beat, and N2 beat all with 0 instances.
* The least instances of false positives was a tie between the W2 beat, L2 beat, and the K3 beat, all with 0 instances.
* The most instances of true negatives belonged to the L2 beat (26,154), then the Q3 beat (25,283), then the E3 beat (25,160)

**We'll say in terms of precision, recall, F1 score, and instances of true positives/Negatives and false positives/negatives that our top crime locations are the B3 beat, E1 Beat, E3 Beat, and L2 Beat.**


### Additional Analysis ###

Since we ran two different models, we had to analyze our models separately. However, we wanted to be able to look at both type of crime and crime location together, and supplement that data with our models. Luckily, we were able to do this by looking at our initial dataframe, and using past data. We looked at what type of crimes occured the most in the past 3 years in the locations that our best model predicted. No machine learning models were used for this, just pure data.

We created a df only containing the Offense and Beat variables, then grouped them together while filtering to make sure that we only saw crimes occuring in the Beats that our best model classified. We then graphed our results, as seen below.


![beats_graph](https://user-images.githubusercontent.com/45251340/230795172-187d6ebd-6aa8-47c2-ad5d-7e71051cc622.png)


From our graph we can see that:

* In the B3 Beat, we can see that the top 3 crimes committed over the past 3 years were: Burglarly/Breaking & Entering, Theft From Motor Vehicle, and Theft of Motor Vehicle Parts or Accessories


* In the E1 Beat, we can see that the top 3 crimes committed over the past 3 years were: Burglarly/Breaking & Entering, Destruction/Damage/Vandalism Of Property, and Simple Assault


* In the L2 Beat, we can see that the top 3 crimes committed over the past 3 years were: Shoplifting, Burglarly/Breaking & Entering, and Theft From Motor Vehicle


**When we combined the top classified Beats with our graph results, we identified more crimes in addition to our model's results. Our favored model best classified Simple Assault crimes and Shoplifting crimes, which was also supported by our graph results, but now we're also aware that Burglarly/Breaking & Entering crimes and Theft From Motor Vehicle crimes are major crimes that occured over the years. Knowing what kind of crimes are most prevalent in what areas will allow us to come up with plans for preventing further crimes, allocate more resources to certain areas, etc. We'll go over these possibilities more in depth in our final conclusion.**


## Final Results ##

As a recap, our best model for our crime type classification was our initial CatBoost model. Our best model for our crime location classification model was also our initial CatBoost model. We found that our crime type model best classified Simple Assault crimes and Shoplifting crimes, while our crime location model best classified B3, E1, and L2 Beat locations. The results for our best crime type classification model can be found summarized above, as can the results for our crime location classification model. Lastly, when we put Beat and crime type together, we saw that the top crimes that occured over the past 3 years in the B3, E1 and L2 Beat were Burglarly/Breaking & Entering and Theft From Motor Vehicle crimes.

## Conclusion ##
The goal was to work with the city of Seattle to build two classification models that accurately classify not only types of new crimes, but also location of new crimes. Ideally, when a new crime occurs the models should be able to instantly and accurately predict what type of crime it is and where the crime is occuring. 

For our crime type models, we used both a Random Forest model and a CatBoost model. We also tuned these models further to see if we could improve performance. After comparing the various models, we found that our initial CatBoost model was the best crime type model overall. For crime location, we only used CatBoost models. We compared an initial default CatBoost model and a Grid-search tuned CatBoost model. We found that our initial CatBoost model performed the best. Lastly, we looked at crime that had occured in the past 3 years in the Beats that our crime location model best classified, and found that Burglarly/Breaking & Entering crimes and Theft From Motor Vehicle crimes were highly prevalent. 

So when it came to type of new crimes, the crimes that our model best classified were Simple Assault and Shoplifting. When it came to location of a new crime, our model best classified new crime as occuring in the B3, E1 and L2 Beat. Lastly, when we put Beat and crime type together, we saw that the top two crimes that occured over the past 3 years in the B3, E1 and L2 Beat were Burglarly/Breaking & Entering and Theft From Motor Vehicle crimes. When we put all of this together we concluded that the areas we should be focused on are the B3, E1, and L2 Beats, while the top crimes that we should be concerned about are Burglarly/Breaking & Entering, Shoplifting, Simple Assault, and Theft From Motor Vehicle crimes. But what does this all mean for the city of Seattle?  We'll discuss more about this in our reccomendation section.


## Recommendations ##

Our goal is to work with the city of Seattle to reduce crime. To do that, we need to know not only what types of crime are occuring, but also where they're occuring. We'll start off with looking at location of crime. We saw that the top Beats that crime was classified to occur in were the B3, E1 and L2 beats. Let's take a look at a partial Beat map of Seattle, and locations of police stations.

<img width="298" alt="partial beat map" src="https://user-images.githubusercontent.com/45251340/230994447-b4b17cac-8e6e-419c-b809-aab8487dba1f.PNG">


We can see that while South Seattle (where the E1 Beat is located) has several police stations (blue P markers on the map), North Seattle (where Beat B3 and L2 are located) only has one police station. Already this could lead to reduced Police presence, which could lead to reduced responsiveness to crimes. A great suggestion would be to build more (at least one) police stations closer to the B3 Beat. Later on (depending on results) more could be built, potentially around the outskirts of the Q1 and U3 Beat. Another solution would be to allocate some of South Seattle's police force to the northern area, to increase patrol ability and police responsiveness.

We saw that the top crimes in those Beats were Burglarly/Breaking & Entering, Shoplifting, Simple Assault, and Theft From Motor Vehicle. As mentioned earlier, we could increase police patrols and build additional stations around those areas to create faster response times, and potentially deter future crimes simply by presence of police. Another suggestion would be to implement a sort of neighboorhood watch that the police would aid in patrolling the area. Not only would that increase the manpower of people monitoring the area, but may also help improve relations between the police and the public. As our model improves and we gain a clearer picture of crime types and crime locations, we can create more watch programs. When looking at our top crimes, it may also be beneficial to implement the use of security cameras around the city (if not already implemented). That way if these crimes do manage to occur, it will be easier to track down the culprit(s), and may deter future crimes similar to increasing manpower. Lastly, in regards to Theft From Motor Vehicle crimes, it would be greatly beneficial to build more secure and affordable parking garages. This would not only reduce the amount of cars being broken into, but would also create positive feedback from the public, since finding affordable, secure parking in Seattle can be difficult.

When it comes to implementing the model, it would be beneficial to use it either in police dispatch center, or in the mobile laptops that the police carry with them. This way the notification and response can be almost instantaneous. Another great idea would be to try to implement our model with social media. Any crime or suspicious activity that gets posted online would instantly notify someone of the type and location of the crime. Lastly, it would be great if we could blend the two models together into one Machine Learning model. This may be a complex process, but not impossible.

If this model is successfully improved and implemented, it could have a huge positive impact on the city of Seattle. It would decrease crime, improve police relations with the public, and even have positive side effects like increased tourism and more businesses opening up. Then once proven repeatedly successful, the model can potentially be generalized to other parts of Washington, and even other states.

## Repository Structure ##

├── .ipynb_checkpoints

├── catboost_info

├──.gitignore

├── Preprocessing Data.ipynb

├── README.md

├── Seattle-Crime.ipynb

├── seattle_crime_beat.csv

├── seattle_crime_beat.csv

└── supplemental.csv
