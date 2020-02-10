# physics-churn

In the world of physics, a major question is being able to anticipate whether or not a young researcher will leave the field and why. 
As I currently wrap up my PhD and look to transition into data science, this question is particularly close to my own experience as well as that of my peers currently on the academic job market.
It is also crucial for senior faculty looking to hire postdocs or junior faculty. 
Academic hiring committees want to select candidates with a strong chance for success: given the sparcity of academic jobs, due diligence is exercised for every hire. 
However, while there are a plethora of metrics which distinguish researchers (e.g. citations, h-index, e-index, etc.) there is also a high degree of subjectivity in the selection process. 

I decided to tackle this problem in a data-driven way using the large database of high-energy physics publiations and metadata available on INSPIRE (inspirehep.net).  
This is basically a problem of predicting "churn" based on existing data on authors who have either stayed in or left physics. 
More importantly, can we gain insights into the key features that correlate with academic longevity? 

In [inspire_data_cleaning][https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_data_cleaning.ipynb], we first clean the data (e.g. dealing with different authors with the same name or multiple names for the same author). 
We then engineer relevant features such as creating the network of collaborators for each author. 
I decided to use only appropriately "normalized" features such as "papers per year" and "citations per year averaged over all papers". 
This is because traditional metrics such as total citations or number of publications may lead to label leakage in any predictive model due to their containing implicit information about a given author's number of years in the field. 
By definition, such information is biased against a young researcher. 

Next in [inspire_eda][https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_eda.ipynb] we explore the data
We use various selection criteria to cut irrelevent data that might skew our results, e.g. authors in large experimental collaborations which can have O(100-1000) collaborators and a biased citation count. 
We then label the remaining author examples as either "Active", "Churn", or "Unlabeled" using a straightforward criteria. 

Finally in [inspire_model][https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_model.ipynb] we build a benchmark model and analyze its results. 
A simple random forest classifier achieves ~ 90% accurate on validation data, and we find the most important features are an author's max citations per year averaged over all papers and the max value of this same metric over all the author's collaborators.  
This agrees with the intuition that having breakthroughs (i.e. papers with lots of citations) and working with authors who have had previously breakthroughs tend to be correlated with academic success. 
We also use the model to provide a individual feature importances for sample "Unlabeled" authors using the SHAP framework, i.e. explain the model "black-box" for any single prediction. 

A major shortcoming with the model is it tends to give excessiely high churn probabilities for grad students. 
This is expected because a typical grad student's publications data (even if normalized by number of years in the field), usually cannot compare with those of "Active" authors who have been doing physics for >= 12 years.
One way to improve the model would be to somehow account for "potential".
This can be done by generating data points for each labeled author for every year they have been in the field. 
For instance, we can generate 20 additional author examples for a physicist of 20 years experience, with each example only accounting for the citation metrics up to that time period but still importantly labeled as active. 
However, based on a few test cases, it appears the benchmark does give sensible and insightful results for researchers currently on their first or second postdoc and seeking full-time academic jobs. 
It also appears to be quite informative for junior faculty who have yet to receive tenure in terms of outlining the important features that improve or reduce their chances of longevity. 




