# Model to predict academic "churn", extract insights into key features correlating with researchers leaving physics

In the world of physics, a major question is being able to anticipate whether or not a young researcher will leave the field and why. 
As I currently wrap up my PhD and look to transition into data science, this question is particularly close to my own experience as well as that of my peers currently on the academic job market.
It is also crucial for senior faculty looking to hire postdocs or junior faculty. 
Given the sparcity of academic jobs, hiring committees exercise due diligence in selecting candidates with a strong chance for success. 
However, while there are a plethora of metrics which distinguish researchers (e.g. citations, h-index, e-index, etc.) there is also a high degree of subjectivity in the entire process. 

I decided to tackle this problem in a data-driven way using the large database of high-energy physics publiations and metadata available on [INSPIRE](https://www.inspirehep.net).  
This is basically a problem of predicting "churn" based on existing data on authors who have either stayed in or left physics. 
More importantly, can we gain insights into the key features that correlate with academic longevity? 

In [inspire_data_cleaning](https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_data_cleaning.ipynb), we first clean the data (e.g. dealing with different authors with the same name or multiple names for the same author). 
We then engineer relevant features such as creating the network of collaborators for each author. 
I decided to use only appropriately "normalized" features such as "papers per year" and "citations per year averaged over all papers". 
This is because traditional metrics such as total citations or number of publications may lead to label leakage in any predictive model due to their containing implicit information about a given author's number of years in the field. 
By definition, such information is biased against a young researcher. 

Next in [inspire_eda](https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_eda.ipynb) we explore the data. 
We use various selection criteria to cut irrelevent data that would skew our results, e.g. authors in large experimental collaborations which can have O(100-1000) collaborators and a biased citation count. 
We then label the remaining author examples as either "Active", "Churn", or "Unlabeled" using a straightforward criteria. 

Finally in [inspire_model](https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_model.ipynb) we build a benchmark model and analyze its results. 
A simple random forest classifier achieves ~ 90% accuracy on validation data. 
Generally, the most important features are an author's max citations per year averaged over all papers and the max value of this same metric over all the author's collaborators.  
This agrees with the intuition that having breakthroughs (i.e. papers with lots of citations) and working with people who have had breakthroughs tend to be correlated with academic success. 
We also use the SHAP framework to interpret how the model makes individual predictions and to explain the feature importances on "Unlabeled" test cases. 
We found the benchmark model givse sensible and informative results for researchers currently on their first or second postdoc seeking full-time academic jobs. 

A major shortcoming with the model is that it tends to give excessiely high churn probabilities for graduate students. 
This is expected because a typical grad student's publications data (even if normalized by number of years in the field), usually cannot compare with those of "Active" authors who have been doing physics for >= 12 years.
One way to improve the model would be to somehow account for "potential".
This can be done by generating data points for each labeled author for every year they have been in the field. 
For instance, we can generate 20 additional author examples (labeled as "Active") for a physicist of 20 years experience, with each example only accounting for the citation metrics up to a given year.

