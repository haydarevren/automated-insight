How much of the data processing can be automated? Is it possible to manage all aspects of data analysis with a single click? Our guiding angels for the decision-making process are the insights from the results. Organizations that have been effective in adopting and evangelizing analytics have set themselves apart from their competitors. However, it is always a huge hassle for small-size companies to use data for a better-informed decision. Hiring a data analyst or scientist might be very luxurious. The first step toward alleviating the burden is automated data processing. It offers a systematic approach to reviewing, cleaning, transforming, and modeling data to uncover valuable knowledge, make recommendations and help decision-making for further investigation.

This project aims to guide enterprise stakeholders in their future business decisions. The users can upload several datasets and choose the target subjects, target variables, and requested prediction interval.

It will bring the following for its users:

•	the predictions for the target subjects and variables,

•	data insights related to target subjects and target variables,

•	a combined dataset that all missing entries are imputed.

For example, a shoe store manager can know how much each brand would sell next two months. He can adjust his stocks accordingly.

For the best user experience, it is deployed as a webapp on heroku using Flask. One can simply go to https://automated-insight-capstone.herokuapp.com/ to give it a try.

I will use the GDP and inflation forecast example to demonstrate how automated data analysis can be done and what values it will bring to the users.

Data: 

I have used 2019 dataset from across 13 publicly available sources. (15312 observations, 3451 variables, 403.2MB memory usage)
1.	World Bank Development Indicators
2.	Penn World tables
3.	Global competitiveness indices
4.	Global competitiveness report
5.	World governance indicators
6.	IMF World economic outlook
7.	Atlas of Economic complexity
8.	Reinhart and Rogoff FX
9.	Barro-Lee education data
10.	Enterprise survey
11.	Doing business indicators (DBI)
12.	Economist intelligence unit
13.	Quality of government

Model:

The users will upload the datasets and select their target subjects and variables to predict. They will also choose the prediction interval in the future. The example is a panel (longitudinal) data set. It is multi-dimensional data involving measurements over time. Time-series and cross-sectional data can be considered special cases of panel data in one dimension only. I pick this type of dataset since it is the most common type of data we see in business data analysis. The aim is to both automate all steps and find fast and reliable predictions.

I used the combined dataset and built a generic preprocessing step. The aim is to accommodate all types of uncleaned datasets. The dataset in my example has too many missing values. The first step is imputation the missing values.

I first encode the categorical columns (leave out the missing values and save encoders as a dictionary). Then I created lag and lead terms for each observation (ensure that lags and leads are coming from the same subject). Then I use Bayesian Ridge iterative imputer (ensure that the estimates for categorical variables do not go outside their bounds). Then, I transform the scaled data to obtain the imputed dataset. This is the first output of the project. The users can use utilize this dataset for past information. The next step is creating automated prediction and insight.

I used a feature reduction technique to transform high-dimensional non-target columns into a space of fewer dimensions. I utilized the UMAP dimension reduction technique. It is very similar to t-SNE but is very general non-linear dimension reduction and faster than t-SNE. It can accommodate both local and global properties. The next step is detecting the periodicity in target variables. It would be very beneficial to know this information for the LSTM model. It would optimize how long data window I need to feed into LSTM. I find the highest power periods in the FFT transformation of the target columns.

LSTM models require reshaping your dataset to a 3-dimensional object. I fed all these seven variables with five years lags (found from previous FFT spectrum analysis). It takes around 1 minute to train the model and gives better results than the base model.

The deliverable of this project will be a web app. What it yields and the inputs are listed below.

•	Several datasets: uploaded by users

•	Target subject columns, target subject names, target variables, date columns, prediction interval: selected by user

•	Imputed data set: created by the webapp

•	Scatter graphs: created using plotly

•	Prediction data set: created by the webapp

•	Performance of the model in backtesting: created by the model
