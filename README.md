# Predicting Weather Data Neural Network
By: Al, Seth, Rohan, Tyler

## The Problem
Imagine a travel company gives you the deal of a lifetime. An all inclusive trip to a resort island and you get to pick the date but it has to be at least a year in the future. Well, current ecological forecasts are unable to forecast that far but you just want to know what's the most probable thing to happen. You sure do not want to fly to an island and have it not be beach weather!

Our project aims to help with this problem. Based on the region and certain time of year, we want to know what is the most probable temperature for that month. We will analyze different trends in data to find out which time periods are the best to vacation in per region and build a model to predict the temperature in a certain region in the future.


## Data Collection
The Global Historical Climatology Network monthly (GHCNm) dataset is a comprehensive collection of climate data gathered from weather stations around the globe. The dataset was developed by the National Centers for Environmental Information (NCEI) in the early 1990s, with subsequent updates in 1997, 2011, and 2018. The data set consists of 139,758 samples from worldwide weather stations containing their names, longitude and latitude, identification number, and elevation in meters. Most crucially, it includes the year of the station record and monthly values of the recorded temperature to the 100th of a degree Celsius.
	Some potential limitations of the data are geographic coverage, temporal coverage, data quality, and urban effects. Some regions, particularly remote or underdeveloped areas, may have limited or no data representation, potentially skewing our climate analysis. Additionally, the period of recorded data varies by station with many records missing values which could also affect trend analysis. The actual data can be compromised as well by various factors such as station relocation, changes in measurement techniques, or unaccounted-for environmental changes around the station. Finally, stations in urban areas may record artificially high temperatures due to the urban heat island effect which could lead to biases in temperature trends.


## Data Preparation
Given to us was code that was able to pre-adjust the GHCNm dataset into something more workable for the project. First, the code gave us the ability to only use a specific interval of years for the dataset. For this project, we will focus on the years of 2011-2020. Also, the starter code reformatted the starting database by having each row represent a year for a specific weather station and having the average temperature for each month as a value in the columns. We further prepared the data by dropping any rows with null values to ensure there are no holes in our dataset. The dataset given had the rows be associated with a year for a station and a separate column. We reformatted the dataset by having each row correspond to a month in a year for a specific station.  


## Data Exploration

​​Through our model, we aim to predict future temperatures in certain locations to help prepare for future vacations. Sifting through this data we found much valuable information such as the temperature levels at various locations in each month. This will help us predict temperatures based on location and time of year, for example, one of the locations in the dataset was at the Sharjah International Airport in Sharjah, United Arab Emirates we found the average temperature in January to be 19.32 degrees Celsius, but the average temperature in August is around 36.197 degrees Celsius.
![Image](https://github.com/user-attachments/assets/ae9fb106-96d2-4b27-bb68-04e1f8c2f516)
Shown above are global temperature distributions. Global temperature seems to start off low then steadily increase but then decrease back throughout the year. This plot suggests some cyclical association between month and temperature and could be useful for our prediction model. 
These preliminary findings will help us to see if our model is somewhat accurate at the start, and going deeper into the data we will create a model to analyze the temperatures of all past months and years and accurately predict the future temperature values at certain locations and times of year. 

## The Model
We aim to be able to predict the temperature given the location and month based on past data. For the project, we decided to implement a multi-variable linear regression model to predict the temperature for a given place based on the month. Before developing the model, we decided to investigate which variables have the highest correlation with temperature. Having a higher correlation would help us choose which features the model will use to predict. We decided to first investigate the correlation between temperature and the variables of Latitude, Longitude, Standard Elevation, and Year. Shown below is part of the correlation matrix based on the initial filtering of the data:

![Image](https://github.com/user-attachments/assets/03a397c1-d9fb-4906-b373-e2280392cc52)

As seen above, the variable that has the highest correlation for temperature per month seems to be Latitude. Another interesting observation is that there seems to be a higher correlation between the chosen variables and temperature in the winter months for the northern hemisphere than than the summer months for the northern hemisphere. Overall, there does seem to be lower correlation between all these variables and temperature. 
Still, we decided to use all the features we chose in our initial linear regression model. We developed a specific model with the inputs being Latitude, Longitude, Standard Elevation, Year; the outputs for the model being that specific temperature prediction for the month. The average R^2 value for the model with this data frame is 0.0641. Shown below is a plot describing the actual vs the predicted.

![Image](https://github.com/user-attachments/assets/1936fbbc-d022-454b-88bb-8a00441f1e4d)

For a model to be better, the prediction value would want to be closer to the actual, hence being closer to the “perfect line” shown on the plot. With lower correlation, the model did not perform well as shown with the lower R^2 values and plot. We further investigate by having different variations of the input variables through filtering and adjusting the initial dataset. One very essential thing we forgot to take into account was the month. Yes, each row corresponded to a month in a year but we had forgotten to take into account which month it was during training. Based on our exploratory analysis, we saw that temperature seemed to take a parabolic shape depending on the month. Another thing we wanted to see is if taking into account which hemisphere the station is in would affect the model. Since summer in the northern hemisphere would mean winter in the southern hemisphere, having the model take into account hemispheres may prove useful. So we adjusted the data frame by adding a column associated with a parabolic form of the month and adding another column to be associated with the northern or southern hemisphere. Shown below is the correlation matrix including the two new features we added in the dataset.

![Image](https://github.com/user-attachments/assets/cbab1ca9-213e-465d-8ef2-245cfb287400)

With the new features introduced, we trained a Linear Regression model on this new and improved dataset. We were able to improve with an R^2 for the model on the testing set being 0.3247. Shown below is a plot describing the actual vs the predicted. 

![Image](https://github.com/user-attachments/assets/72d54247-c91d-477e-8c8e-cd717847eeb1)

There was a dramatic improvement in R^2 value and the predictions were centered more around the perfect prediction line. Still, there was a thought in the back of our heads that mutating the dataset could result in better scoring models. We then found out that instead of having a column describing the hemisphere, splitting the data frame into two, where one is associated with data in the northern hemisphere and the other southern, provided much better scoring results. The model trained on only the northern hemisphere gave an R^2 of 0.5651. The model trained on only the southern hemisphere gave an R^2 of 0.7661. Shown below is a plot describing the actual vs the predicted for each. 

![Image](https://github.com/user-attachments/assets/8ff522d7-f959-4e58-ba52-03357396c2c6)
![Image](https://github.com/user-attachments/assets/50de2a5a-4980-46cc-b544-2304233f5921)

There was even more substantial improvement in R^2 values for both models. Also, the predictions for both models were a lot more centered around the perfect prediction line. This structure of developing and creating the datasets outputted the best accurate models. 

## Model Deployment

The temperature prediction models we have developed could be used for web or mobile applications. Users could input their desired vacation month and region and the app would suggest the best vacation spots and period based on predictions from historical temperature data. In our code we developed a python notebook that is able to take location and find the closest weather station to the location. The data from that weather station could be used on the prediction model to predict the temperature.
It could also be integrated with travel platforms to incorporate temperature predictions directly into vacation planning tools. For example, users could see climate forecasts alongside flight and hotel options way off in the future. Some challenges with these deployments, however, involve inaccuracies in the predictions. Because the predictions depend on the historical data matching current weather patterns, incomplete data or changing climates could lead to unreliable forecasts. Additionally, the model would not account for extreme weather events which would affect the prediction reliability.

## Conclusion
The month of the year and hemisphere proved to have substantial effects on the temperature of the location. Still, the models based on the features we trained on were not able to predict extremely well. Weather forecasting is a very complicated procedure and more research can be done to determine what other features may influence having better predictions. In the end, we can advise on the temperature for your vacation (with a grain of salt)! 

