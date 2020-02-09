# physics-churn

My first (real) data project! 

In the world of physics, a major question is whether or not a promising young researcher will leave the field. 
Of course, as I currently wrap up my PhD and look to transition into data science, this question is particularly close to my own experience. 
It is also extremely important for senior faculty members looking to hire postdocs or junior faculty. 
Academic hiring committees want to be certain that their selected candidates have a strong chance for success. 
Given the sparcity of academic jobs, due diligence is exercised for every hire. 
However, while there are a plethora of metrics which distinguish researchers (e.g. citations, h-index, e-index, ...) there is also a high degree of subjectivity in the selection process (e.g. familiarity, letters of recommendation, etc.)

I decided to tackle this problem in a data-driven way using the large database of high-energy physics papers and metadata available on INSPIRE HEP. 
This is basically a problem of predicting "churn" --- is a researcher more likely to leave physics based on their history? 
Further, what are the key features that have served as an indicator for churn in the past?

First, in inspire_data_cleaning we clean the data (e.g. dealing with different authors with the same name or multiple names for the same author). 
We then engineer relevant features and create the network of collaborators for each author. 
It is especially important that we only use appropriately "normalized" features such as papers per year and the citations per year averaged over all papers. 
For instance, total citations or number of papers are biased features and would lead to label leakage in any predictive model as these implicitly contain time dependence. 

Next in inspire_eda, we explore the data. 
We also employ various selection cuts to remove irrelevent data that might skew our results, e.g. authors in large experimental collaborations which can have >1000 collaborators and a biased citation count. 
We also label the remaining authors as either "Active", "Churn", or unlabeled using a straightforward criteria. 

Finally in inspire_model, we build a predictive model and analyze its results/shortcomings.
We find that a simple random forest classifier is able to achieve 90% accuracy on the training data with cross-validation, and that the most important features for prediction are related to citations of an authors' collaborators and the author's maximum citations per year over all their papers. 
This agrees with the intuition that breakthroughs (i.e. papers with lots of citations) are important, and also that having good/famous collaborators can go a long way. 
It appears the model does not generalize well to authors with a minimum publication record, i.e. graduate students. 
This may due to the sparcity of such examples in the training data. 
However, it does seem to give accurate predictions ("Active" vs. "Churn") to authors that are currently finishing their first postdoc, based on a few test cases.

