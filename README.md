# 2015-2017-UK-General-Election-Forecast-via-Twitter-network-analysis

The motivation behind this project is to collect candidates' information from twitter and verify the feasibility of using candidates' twitter information to forecast UK General Election.  
In this work, we collected unique candidate based Twitter datasets from API. For each candidate, we also collected every tweet they posted one year before the public polling. In the feature extraction section, we did sentiment analysis, topic extraction, topic analysis, and network analysis to investigate and extract possible features that might be relevant to the election forecast. In the end, by using different machine learning classifiers, we proved that the twitter features played an important role in producing better prediction results.

The project makes the following contributions:

We collected unique candidate based Twitter datasets for 2015 and 2017 UK General Election. The datasets can be used for other 

Simple information (incumbent and age) could produce a good prediction, especially the incumbent information is an important factor.

The standalone candidate based twitter information can make accurate prediction. After combining the simple information mentioned above, we can achieve better prediction results
and our prediction results are perhaps much better than most polls. 

Twitter, and online social networks, in general, will play an increasingly more important role; we need more study on social networks, such as second-order connectivity.   

## United Kingdom General Elections
The general election is to give people opportunity to chose their local Member of Parliament (MP), the person who will represent their local constituency and have a seat in the House of Commons.  There are 650 electoral districts, and each of them will elect one MP. Usually, the political party has the most seats will form the government, and its leader will be the Prime Minister\cite{uk_parliament}. If the election result shows no single majority party has an absolute majority of seats in the parliament, a hung parliament (which is also called balanced parliament) will form a government.

## Models
* Random Forest Classifier
Random forest is an ensemble learning method for classification or regression tasks. It can construct decision trees at training time,  and then output the class that is the mode of the classes (classification) or mean prediction (regression) of the individual trees. In the Random Forest Classifier, each tree in the ensemble is built from a sample drawn with replacement from the training set if bootstrap=True (default). When splitting a node during the construction of the tree, the split that is picked is the best split among a random subset of the features rather than the best split among all features. Because of the random pick, the bias of the forest usually slightly increases (concerning the bias of a single non-random tree) but, due to averaging, its variance also decreases, generally more than compensating for the increase in bias, hence yielding an overall better model.

* Logistic Regression
Logistic regression is a statistical method to analyze and predict the outcome based on the values of the independent variables. The logistic function, which is also called the sigmoid function, is useful because it can take any kind of input, and then the output will always between 0 and 1.

* Factorization Machines
The Factorisation Machine (FM), proposed by Rendle, is a state-of-the-art machine learning approach has high performance on the recommendation system task. It can estimate the parameters of sparse data by factorisation and especially works well in category features.
A open-sourced python FM package called pyfm can be easily used for python developers. The following are the mathematical explanation of factorisation machine.

## Results
To see whether the twitter features can have a positive effect on the prediction, I trained the prediction models only by Twitter irrelevant features, like 'former\_MP', 'gender'. 
The the second experiment is to see the prediction results of models trained only by Twitter features. In the third experiment, all the features will be used. For the last experiment, the model will be trained by 2015 election data, and then the model will make predictions to the 2017 general election data.

### Model trained by Twitter irrelevant features
|             Model            | Average polling accuracy |
|:----------------------------:|:------------------------:|
| Linear Regression (baseline) |          0.7368          |
|    Support Vector Machine    |          0.7843          |
|     Factorization Machine    |          0.7523          |
|         Random Forest        |          0.8274          |
|            Xgboost           |          0.8199          |

### Model trained by Twitter relevant features
|             Model            | Average polling accuracy |
|:----------------------------:|:------------------------:|
| Linear Regression (baseline) |          0.8722          |
|    Support Vector Machine    |          0.8419          |
|     Factorization Machine    |          0.8623          |
|         Random Forest        |          0.8957          |
|            Xgboost           |          0.8843          |

### Model trained by all the features
|             Model            | Average polling accuracy |
|:----------------------------:|:------------------------:|
| Linear Regression (baseline) |          0.8933          |
|    Support Vector Machine    |          0.8734          |
|     Factorization Machine    |          0.8910          |
|         Random Forest        |          0.9181          |
|            Xgboost           |          0.9102          |

The MP twitter account based method gives us a new view to make UK General Election prediction. The datasets are combined by different sub-datasets, including datasets collected from Twitter API, the online resources, and  UK government websites. More over, the second-degree connection features are extracted and have played an important role in the prediction result. The result shows the twitter and social networks information can have a positive effect to the polling forecasting. 

## Requirements
* Python 3.5
* scikit-learn, pyfm, pandas, numpy, Xgboost, NetworkX
* Jupyter notebook
* Gephi

# Experiment Setup
## Twitter API Setting
Twitter provides a helpful interface for the developer to collect Twitter data. But the first step to access the data is to register an API key. Here are steps to get API keys from Twitter:
1.Sign in the Twitter account at https://apps.twitter.com.  Click the Create New App button and fill the required information.

2.Create Read and Write access to the new Twitter application.
3.Go to Keys and Access Token page and remember the access token as follow:

app\_key=YOUR CONSUMER KEY\\  
app\_secret=YOUR CONSUMER SECRET \\ 
oauth\_token=YOUR ACCESS TOKEN \\ 
oauth\_token\_secret=YOUR ACCESS TOKEN SECRET\\


## Python-twitter Crawler

Python-twitter is a well developed Twitter crawler library. It's a Python interface for Twitter API and friendly to developers. It can retrieve by $screen\_name$ and $user\_id$ which is frequently used in this project. After adding the access token to the API setting and the Twitter wrapper can collect twitter object from Twitter API.
Twitter API has limits to the different request. For user information, it only permits 15 calls every 15 minutes.
In this project, I applied three twitter applications to speed up data collecting procedure.


# Dataset
The dataset in this project was collected from the Twitter application program interface (API). The Twitter Developer application allows researchers develop their application connections by OAuth. The crawler in this project is python-twitter. The data was retrieved by Twitter screen name, can be found in the next url: https://candidates.democracyclub.org.uk/media/.
There are two collected election datasets, 2015 and 2017. For each dataset, there are five sub-datasets: Twitter account dataset, second-degree dataset, tweets dataset, network feature dataset and candidate information. 

## MP candidates' account dataset
This dataset is crawled by MP's $screen\_name list$. Crawled data are https://dev.twitter.com/overview/api/users and https://dev.twitter.com/overview/api/users. There are many account contexts and not all of them are useful (for example we do not interest in profile\_background\_color and profile\_image\_url because it is irrelevant for election forecast). The following contexts are saved:

- created\_at: UTC time when this Tweet was created.
- text: The actual UTF-8 text of the status update.
- screen\_name: The screen name, handle, or alias that this user identifies themselves with.
- followers\_count: The number of followers this account currently has.
- friends\_count: The number of users this account is following (AKA their “followings”).



## Network feature dataset
This dataset is network relevant features. As I have mentioned at experiment setup, Twitter API has rate limits of different type of requests every 15 minutes; it's very slow to get the friends or followers user information (15 requests per 15 minutes). An alternative way to construct a network is to build a mentioned, retweeted, or quoted network. Hence this dataset can be used for both network analysis and election forecasting. Data label and explanation are as follow:



- user\_mentions: An array of mentioned Twitter screen names extracted from the Tweet text.
- quoted: An array of quoted Twitter screen names extracted from the Tweet text.
- retweet: An array of retweet Twitter screen names extracted from the Tweet text.

- retweet\_count: Number of times this Tweet has been retweeted.
- favorite\_count : Nullable Indicates approximately how many times this Tweet has been liked by Twitter users.



## Second-degree dataset
In the social network analysis, we usually focus on our direct friends (first-degree). But what about the relationship (connection) between our friends? In the social network, if two people have an interaction, then the importance of the most prominent person each knows is very likely to be the same. Dr Shi's paper examined and discussed a larger variety of other network properties which may cause second-order assortative mixing, including power law distribution, cluster coefficient, and bipartite graphs. But one of these properties was found to provide an adequate explanation. Thus I think this new network property can be unique features of my election prediction model. 

The second-degree features are measured by node degree, friends degree and followers degree. For each MP candidate, use the python twitter crawler get their friends and followers' node degree, record the maximum, minimum, and calculate the average degree and variance. 

## Election result dataset
The election result data set is download from https://candidates.democracyclub.org.uk/media/. It has MP candidates' information including name, gender, birth date, party name,  election area, incumbent, and even photo. One important feature I think is incumbent. It's commended sense that incumbent MP may likely to be reappointed because he or she has work experience and reputation. I will train a classier only based on these data to see whether these features can work pretty well in the election prediction.
