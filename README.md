# physics-churn

In the world of physics, a major question is being able to anticipate whether or not a young researcher will leave the field and why. 
As I currently wrap up my PhD and look to transition into data science, this question is particularly close to my own experience as well as that of my peers currently on the academic job market.
It is also crucial for senior faculty looking to hire postdocs or junior faculty. 
Academic hiring committees want to select candidates with a strong chance for success: given the sparcity of academic jobs, due diligence is exercised for every hire. 
However, while there are a plethora of metrics which distinguish researchers (e.g. citations, h-index, e-index, etc.) there is also a high degree of subjectivity in the selection process. 

I decided to tackle this problem in a data-driven way using the large database of high-energy physics publiations and metadata available on INSPIRE (inspirehep.net).  
This is basically a problem of predicting "churn" based on existing data on authors who have both stayed in or left physics. 
More importantly, can we gain insights into the most relevant features that correlate with academic longevity? 

In https://nbviewer.jupyter.org/github/vijayoct27/physics-churn/blob/master/inspire_data_cleaning.ipynb, 

we clean the data (e.g. dealing with different authors with the same name or multiple names for the same author). 
We then engineer relevant features, such as creating a network of collaborators for each author. 
It is especially important that we only use appropriately "normalized" features such as "papers per year" and "citations per year averaged over all papers". 
For instance, total citations or number of papers are biased features and would lead to label leakage in any predictive model as these implicitly contain time dependence. 

Next in http://localhost:8888/notebooks/Documents/GitHub/physics-churn/inspire_eda.ipynb 
we explore the data. 
We also employ various selection cuts to remove irrelevent data that might skew our results, e.g. authors in large experimental collaborations which can have >1000 collaborators and a biased citation count. 
We also label the remaining authors as either "Active", "Churn", or unlabeled using a straightforward criteria. 

Finally in http://localhost:8888/notebooks/Documents/GitHub/physics-churn/inspire_model.ipynb 
we build a predictive model and analyze its results/shortcomings.
We find that a simple random forest classifier is able to achieve 90% accuracy on the training data with cross-validation, and that the most important features for prediction are related to citations of an authors' collaborators and the author's maximum citations per year over all their papers. 
This agrees with the intuition that breakthroughs (i.e. papers with lots of citations) are important, and also that having good/famous collaborators can go a long way. 
It appears the model does not generalize well to authors with a minimum publication record, i.e. graduate students. 
This may due to the sparcity of such examples in the training data. 
However, it does seem to give accurate predictions ("Active" vs. "Churn") to authors that are currently finishing their first postdoc, based on a few test cases.

