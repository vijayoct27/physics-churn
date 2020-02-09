# physics-churn

My first (real) data project! 

In the world of physics, a major question is whether or not a promising young researcher will leave the field. 
Of course, as I currently wrap up my PhD and look to transition into data science, this question is particularly close to my own experience. 
It is also extremely important for senior faculty members looking to hire postdocs or junior faculty. 
Academic hiring committees want to be certain that their selected candidates do indeed have a strong chance for sustained success. 
Given the sparcity of academic jobs, due diligence is exercised for every hire. 
However, while there are a plethora of metrics which distinguish researchers (e.g. number of papers, citations, h-index, e-index, ...) there is also a high degree of subjectivity in the selection process (e.g. familiarity, letters of recommendation, etc.)

I decided to tackle this problem in a data-driven way using INSPIRE's large database of high-energy physics papers and metadata. 
In a sense, this is basically a problem of predicting "churn" --- is a researcher more likely to leave physics based on their history? 
Further, what are the important features that have served as an indicator for churn in the past?
In inspire_data_cleaning, I preprocess/clean the INSPIRE data and also engineer some requisite features. 
In inspire_eda, I explore the data and employ crucial selection criteria to cut down on irrelevant data. 
Finally in inspire_model, I build a (simple) machine learning model and analyze its results and shortcomings.