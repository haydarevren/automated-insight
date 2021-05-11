How much of the data processing can be automated? Is it possible to manage all aspects of data analysis with a single click? Our best friends for the decision-making process are the insights from the results. Organizations that have been effective in adopting and evangelizing analytics have set themselves apart from their competitors. However, it is always a huge hassle for small-size companies to use data for a better-informed decision. Hiring a data analyst or scientist might not always be cost effective. The first step toward alleviating the burden is automated data processing. It offers a systematic approach to reviewing, cleaning, transforming, and modeling data to uncover valuable knowledge, make recommendations and help decision-making for further investigation.

This project aims to guide enterprise stakeholders in their future business decisions. The users can upload several datasets and choose the target subjects, target variables, and requested prediction interval.

It will bring the following for its users:

•	the predictions for the target subjects and variables,

•	data insights related to target subjects and target variables,

•	a combined dataset that all missing entries are interpolated.

For example, if a shoe store manager knows how many units each brand would sell for the next two months, s(he) can adjust his/her stock accordingly.

For the best user experience, it is deployed as a webapp on heroku. One can simply go to https://automated-insight-capstone.herokuapp.com/ to give it a try.

I will use the GDP and inflation forecast example to demonstrate how automated data analysis can be done and what values it will bring to the users.

Data: 
I have used 2019 dataset from across 13 publicly available sources. (15312 observations, 3451 variables, 403.2MB memory usage, 196 countries observed for 58 years)
1.	World Bank Development Indicators (https://datacatalog.worldbank.org/dataset/world-development-indicators)
2.	Penn World tables (https://www.rug.nl/ggdc/productivity/pwt/?lang=en)
3.	Global competitiveness indices (https://govdata360.worldbank.org/indicators/h93b3b7a4)
4.	Global competitiveness report (https://www.weforum.org/reports/how-to-end-a-decade-of-lost-productivity-growth)
5.	World governance indicators (https://info.worldbank.org/governance/wgi/)
6.	IMF World economic outlook (https://www.imf.org/en/Publications/SPROLLs/world-economic-outlook-databases)
7.	Atlas of Economic complexity (https://atlas.cid.harvard.edu/)
8.	Reinhart and Rogoff FX (https://www.ilzetzki.com/irr-data)
9.	Barro-Lee education data (http://www.barrolee.com/)
10.	Enterprise survey (https://www.enterprisesurveys.org/en/enterprisesurveys)
11.	Doing business indicators (DBI) (https://www.doingbusiness.org/en/data)
12.	Economist intelligence unit (https://country.eiu.com/united-states)
13.	Quality of government (https://www.gu.se/en/quality-government/qog-data)

Model:

The users will upload the datasets and select their target subjects and variables to predict. They will also choose the prediction interval in the future. The example is a panel (longitudinal) data set. It is multi-dimensional data involving measurements over time. Time-series and cross-sectional data can be considered special cases of panel data in one dimension only. I pick this type of dataset since it is the most common type of data we see in business data analysis. The aim is to both automate all steps and find fast and reliable predictions.

I used the combined dataset and built a generic preprocessing step. The aim is to accommodate all types of uncleaned datasets. The dataset in my example has too many missing values. The first step is imputation of missing data.

I first encode the categorical columns (leave out the missing values and save encoders as a dictionary). Then I created lag and lead terms for each observation (ensure that lags and leads are coming from the same subject). Then I use Bayesian Ridge iterative imputer (ensure that the estimates for categorical variables do not go outside their bounds). Then, I transform the scaled data to obtain the imputed dataset. This is the first output of the project. The users can use utilize this dataset for past information if they choose to. The next step is creating automated prediction and insight.

I used a feature reduction technique to transform high-dimensional non-target columns into a space of fewer dimensions. I utilized the UMAP dimension reduction technique. It is very similar to t-SNE but is very general non-linear dimension reduction and faster than t-SNE. It can accommodate both local and global properties. The next step is detecting the possible periodicity in target variables. It would be very beneficial to know this periodicity information for the LSTM model. It would optimize how long data window model needs to feed into LSTM. It finds the highest power periods in the FFT transformation of the target columns.

Long Short-Term Memory (LSTM) recurrent neural networks are an excellent algorithm for time series data that also can easily adapt to multivariate or multiple input forecasting problems. LSTM models require reshaping your dataset to a 3-dimensional object. I fed all these seven variables with found optimal periodicity lag (found from previous FFT spectrum analysis). There are two hidden layers in the model. It takes around 1 minute to train the model and gives a similar result to the baseline model that does not use any feature reduction.

The deliverable of this project will be a web app. What it yields and the inputs are listed below.

•	Several datasets: uploaded by users

•	Target subject columns, target subject names, target variables, date columns, prediction interval: selected by user

•	Imputed data set: created by the webapp

•	Scatter graphs: created using plotly

•	Prediction data set: created by the webapp

•	Performance of the model in backtesting: created by the model
