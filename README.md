<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/wallpaper.jpg">
</p>

<h1 align="center">Airline Passengers Satisfaction Classification</h1>

<h2>I) Introduction</h2>
<p><strong>Dataset</strong>: The dataset is a US airline’s passengers satisfaction survey. It consists of the feedbacks of passengers on various aspects of  their flight, as well as flight data. It can be found on Kaggle.</p>
<p><strong>23 variables:</strong><br>
-	<i>Flight distance</i>: flight distance of the flight, <br>
-	<i>Seat comfort</i>: satisfaction level of seat comfort, <br>
-	<i>Departure/Arrival time convenient</i>: satisfaction level of Departure/Arrival time convenient,<br>
-	<i>Food and drink</i>: satisfaction level of in-flight food and drink,<br>
-	<i>Gate location</i>: satisfaction level of the gate location in the airport, <br>
-	<i>Inflight wifi service</i>: satisfaction level of the in-flight wifi service (0:Not Applicable;1-5),<br>
-	<i>Inflight entertainment</i>: satisfaction level of in-flight entertainment,<br>
-	<i>Online support</i>: satisfaction level of airline online support, <br>
-	<i>Ease of Online booking</i>: satisfaction level of online booking,<br>
-	<i>On-board service</i>: satisfaction level of on-board service,<br>
-	<i>Leg room service</i>: satisfaction level of Leg room service,<br>
-	<i>Baggage handling</i>: satisfaction level of baggage handling,<br>
-	<i>Check-in service</i>: Satisfaction level of check-in service,<br>
-	<i>Cleanliness</i>: satisfaction level of cleanliness of the aircraft,<br>
-	<i>Online boarding</i>: Satisfaction level of online check-in,<br>
-	<i>Departure Delay in Minutes</i>: number of minutes of delay at departure, <br>
-	<i>Arrival Delay in Minutes</i>: number of minutes of delay at arrival, <br>
-	<i>Class</i>: travel class (Business, Eco, Eco Plus),<br>
-	<i>Age</i>: age of the passengers,<br>
-	<i>Customer Type</i>: loyal customer vs. Disloyal customer, <br>
-	<i>Gender</i>: gender of the passenger (female, male),<br>
-	<i>Satisfaction</i>: satisfied or not satisfied of the airline after the flight.</p>

<p><strong>Objectives:</strong><br>
- The first goal is to identify the variables that influence the most the overall satisfaction feeling of a passenger after a flight.<br>
- The second goal is to develop a model that predicts whether a customer will be satisfied or not (variable <i>satisfaction</i>) depending on the details and parameters of the flight.</p>

<p>I will explore the dataset, process it and design a Naive Bayes classifier as well as a Decision Tree.</p>

<h2>II) Exploratory Data Analysis</h2>
<h3>1) Summary statistics</h3>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/summary_statistics.png">
</p>

<p>We can observe that:<br>
  - There are more female than male survey respondents.<br>
  - The median age is 40. <br>
  - There are more than two times more business travellers than personal travellers. <br>
  - Most of the passengers fly in economy. <br>
  - More than half of the flights depart and arrive at time.</p>

<h3>2) Visualizations</h3>

<h2>III) Data processing</h2>
<p>After having got to know the data through summary statistics and visualizations, I will prepare it for modeling.</p> 

<h3>1) Rename variable names</h3>
<p>For ease of use, I rename the variable. The space and special characters were replaced with underscore. Then, the names were converted to lowercase. Here are the new variable names:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/variable_names.png">
</p>

<h3>2) Check missing values</h3>
<p>The only variable containing missing values is <i>arrival_delay_in_minutes</i> with 393 missing values. A missing value does not appear to imply that there is no arrival delay since the variable contains zeros. Furthermore, the missing values only represents 0.3% of the variable length so I chose to drop them.</p>

<h3>3) Treat outliers</h3>
<p>Our data contains 4 continuous variables: <i>light_distance</i>, <i>arrival_delay_in_minutes</i>, <i>departure_delay_in_minutes</i> and <i>age</i>.<br>
I plot their distribution using boxplots to detect outliers:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/outliers_boxplots_continuous_variables.jpg">
</p>

<p>As we can see, <i>flight_distance</i>, <i>departure_delay_in_minutes</i> and <i>arrival_delay_in_minutes</i> contain lots of outliers that have to be treated.<br>
Most of the outliers in <i>departure_delay_in_minutes</i> and <i>arrival_delay_in_minutes</i> are in the same observations (rows) since the arrival delay highly depends in the departure delay.</p>
<p>I identify and remove outliers using Tukey’s rule: any data point above or below 1.5x IQR is labeled as outlier and removed.</p>

<p><i><strong>- flight_distance</strong></i>:<br>
Outliers identified: 2575 <br>
Propotion of outliers: 2%<br>
Mean of the outliers: 4905.6 <br>
Mean with outliers: 1981.01 <br>
Mean without outliers: 1921.67 </p>

<p>
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/outliers_flight_distance.png">
</p>
  
<p><i><strong>- departure_delay_in_minutes</strong></i>:<br>
Outliers identified: 17970 <br>
Propotion of outliers: 16.1 <br>
Mean of the outliers: 82.46 <br>
Mean with outliers: 14.64 <br>
Mean without outliers: 3.71 </p>

<p>
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/outliers_departure_delay.png">
</p>

<p><i><strong>- arrival_delay_in_minutes</strong></i>:<br>
Outliers identified: 17492 <br>
Propotion of outliers: 15.6%<br>
Mean of the outliers: 85.49 <br>
Mean with outliers: 15.09 <br>
Mean without outliers: 4.1 </p>

<p>
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/outliers_arrival_delay.png">
</p>

<h3>4) Encode features</h3>
<p>The data contains 5 categorical variables. It is necessary to encode them into relevant numerical ones before developing our models.</p>
<p>- <i>satisfaction</i>, <i>gender</i>, <i>customer_type</i> and <i>type_of_travel</i> are ordinally coded as follow:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/features_encoding.png">
</p>

<p>- <i>class</i> is one-hot encoded as follow:
  
<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/class_feature_encoding.png">
</p>

<h3>5) Check correlation</h3>
<p>I have to check if there is autocorrelation between the variables I will use to develop the model.</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/correlation_heatmap.png">
</p>

<p>As we can see, <i>class_business</i> and <i>class_eco</i> seems to be highly correlated. <br>
  <i>class_business</i> correlation:<br>
  - <i>class_eco</i>: -0.864757202<br>
  - <i>class_eco_plus</i>: -0.265647751</p>
<p>This is a dummy variable trap: the newly created dummy variables of the class variable are multicollinear. A passenger flying in economy class is obviously not flying in business class, and vice versa. Thus, I remove the <i>class_business</i> variable from the data.</p>

<h2>Features selection</h2>
<p>I used the Boruta algorithm (R package) to find the most meaningful features for the model.<br>
The Boruta algorithm is a wrapper that performs a Random Forest Classifier on the dataset and checks the Z-scores of each feature with their shadow (randomized) feature. After 12 iterations, Boruta found out that <strong>all the 22 features are important</strong>:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/boruta_features_selection.png">
</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/selected_features.png">
</p>

<h2>V) Naive Bayes Classifier</h2>
<p>The Naive Bayes classifier is a probabilistic classifier that predicts on the basis of the probability of an object.</p>
<p>I will use all the variables to predict whether or not a passenger is satisfied or not with the airline. 80% of the data is used for training and 20% is used for test.</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/fit_naive_bayes.png">
</p>

<p>After fitting the model to the data and making predictions on the test set, I obtain the following results:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/evaluation_naive_bayes.png">
</p>

<p>As we can see, the model has a high accuracy of 81.93% and is excellent at both predicting true negative (TN) and true positive (TP).</p>

<h2>VI) Decision Tree</h2>
<p>I also tried to fit a decision tree classifier on the data.<br>
A classification tree labels, records, and assigns variables to discrete classes. It is built through the process of binary recursive partitioning.</p> 

<p>There again, I use all the variables to predict the target variable <i>satisfied</i>:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/fit_decision_tree.png">
</p>

<p>The following output is obtained:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/cp_decision_tree.png">
</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/cp_plot_decision_tree.png">
</p>

<p>As we can see, the best CP is 0.01. I prune the tree with CP=0.01 and obtain the following decision tree:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/decision_tree_graph.png">
</p>

<p>As we can see, the decision tree estimates that <i>inflight_entertainment</i> is the most decisive variable to estimate whether or not a passenger will be satisfied of the airline after his journey. This tree generates rules that are easily understandable for the airline.</p>

<p>Now, I must evaluate the model:</p>

<p align="center">
  <img src="https://github.com/marc-bolle/airlines-passengers-satisfaction-classification/blob/main/images/decision_tree_evaluation.png">
</p>

<p>The decision tree even performs better than the Naive Bayes classifier, with an accuracy score of 0.86%. It is also excellent at predicting true negative (TN) and true positive (TP).</p>
