# Reddit Posts Analysis with Natural Language Processing

## Executive Summary

Pet ownership is often viewed positively in the mainstream media, especially during the COVID-19 pandemic. However, these perspectives can sometimes be overstated and biased, creating unrealistic expectations for pet owners. In 2022, a US-based survey revealed that pets were a major source of stress for a majority of owners during the pandemic. ([source](https://www.avma.org/news/study-explores-pandemic-specific-challenges-pet-ownership)) Hence, a follow-up analysis is required to understand pet owners' pain points to ensure the right measure can be taken to reduce stress and improve their experience.

In this project, we'll be assuming the role of pro-bono data analysts for an animal shelter to address:
1. What are the top concerns for pet owners, specifically dog and cat owners?
2. Which classification model, along with the natural language processing, would be able to predict cat- or dog-specific problems? 

To answer these questions, we'll mine for user-generated content in Reddit and analyze them through natural language processing techniques. Thereafter, we'll provide the animal shelter with recommendations on how they can better support potential pet owners and ensure a positive experience for both owners and pets.

## Data Collection & Methodology

### Data Collection

We collected 6,000 posts from two popular subreddits:

1. [r/CatAdvice](https://www.reddit.com/r/CatAdvice/): This subreddit has over 119,000* cat enthusiasts globally, who actively discuss cat-care topics and exchange best tips for their cats. A total of 3,000 posts were scraped from this subreddit, along with 72 potential feature variables.
2. [r/DogAdvice](https://www.reddit.com/r/DogAdvice/): With over 65,500* members globally, this subreddit is dedicated to discussions on dog training, health, nutrition and other dog-care topics. A total of 3,000 posts were scraped from this subreddit, as well as 80 potential feature variables.

*figures are latest as of December 2022 

### Data Dictionary

After cleaning the data we narrowed now to 2,746 data from r/CatAdvice and 1,353 in r/DogAdvice.

Based on the 72-80 variables pulled, we also narrowed down to a few key features that were pertinent in our investigation:

| Selected Feature 	| Type    	| Dataset                     	| Description                                      	|
|------------------	|---------	|-----------------------------	|--------------------------------------------------	|
| subreddit        	| object  	| r/CatAdvice and r/DogAdvice 	| Refers to the Reddit channel                     	|
| title            	| object  	| r/CatAdvice and r/DogAdvice 	| Reddit title of the post                         	|
| selftext         	| object  	| r/CatAdvice and r/DogAdvice 	| Reddit body post                                 	|
| author           	| object  	| r/CatAdvice and r/DogAdvice 	| Reddit member                                    	|
| created_utc      	| integer 	| r/CatAdvice and r/DogAdvice 	| Time of when the post was created, in UTC format 	|


### Methodology

The subreddit posts were processed and analysed through the following steps:

1. **Web Scraping**: Using Pushshift API, we were able to extract 100 posts per day within 30 days from the subreddits.
2. **Data Cleaning**: We assessed the data by checking the relevance of the variables, removing outliers, dealing with missing values that were key to the investigation.
3. **Exploratory Data Analysis**: We visualized the cleaned data through a series of graphs and plots to better understand the dataset and identify potential keywords for the modeling process.
4. **Data Modeling & Evaluation**: Following the selection of variables, we processed the text data even further and modeled them through these models - Logistic Regression, Naive Bayes, Adaptive Boosting and Random Forest. The best model will then be used to predict whether keywords that would best identify cat vs dog issues. 

## Additional References
1. [Study explores pandemic-specific challenges of pet ownership](https://www.avma.org/news/study-explores-pandemic-specific-challenges-pet-ownership)
    - The article highlights a study conducted in the US, revealing that pets were a source of stress for owners during the pandemic. 
    - I referred to this article to build a compelling story for the analysis as well as to strengthen the conclusion and recommendations. 
2. [Reddit post property unofficial documentation](https://www.reddit.com/r/redditdev/comments/a1dn2p/any_documentation_on_post_properties_such_as/)
    - The subreddit discussion from r/redditdev covered unofficial documentation of the post properties and its definition.
    - I referred to this post to narrow down the relevant features for investigation and analysis. 

## Initial Findings
1. The average length of title and post are shorter in r/CatAdvice compared to r/DogAdvice. Perhaps dog owners are more likely to elaborate the issue faced in detail. 
2. Some of the common phrases mentioned in r/CatAdvice include:
    - Issues faced:
        - "Peeing outside litter box"
        - "Cat keeps peeing"
    - Advice on:
        - "Litter box"
        - "Cat food"
        - "Wet food"
    - Age:
        - Most users seek advice for "month old kitten/cat", "year old cat", "new cat"
        - Some of the uses also seek advice for "old cat"
3. Some of the common phrases mentioned in r/DogAdvice include:
    - Issues faced:
        - "Dog acting weird"
        - "Dog keeps licking"
    - Advice on:
        - "Dog food"
        - "Vet appointment"
    - Age:
        - Most users seek advice for "year old", "month old", "new puppy", "year old dog", "month old puppy"
        - Some of the uses also seek advice for "old dog"
4. We can see that most of the issues are behavioral and training.
5. In both r/CatAdvice and r/DogAdvice, users tend to specify the age of their cats and dogs when seeking advice in the community. This indicate that pet owners are conscious in seeking out appropriate care and solution that works for pets at different life stages. 
6. The timing with most active users on both subreddits is 10AM - suggesting that this would be the best time to engage pet owners.

## Modeling Summary
In our modeling process, we used two main vectorization methods: Count Vectorizer and Term Frequency-Inverse Document Frequency (TF-IDF). 
The data is modeled against four main models: Logistic Regression, Naive Bayes, Random Forest and Adaptive Boosting.
The baseline model accuracy is 67% accurate at predicting that a post is a cat-related issue.

## Modeling Results
1. All of our models outperformed our baseline model of 67.7% accuracy of predicting that a post is cat-related.
2. While we saw a lot of high training scores and reasonably well-performing test and f1 scores - when we look at the ROC, we see only one model exceeds 80% - **Count Vectorizer & Naive Bayes**. This model also had the highest number of true predictions and least number of false positives. Therefore, this is the best model at predicting keywords for cats vs dogs.
3. The best parameter for this model is:
    - Count Vectorizer with 10,000 max feature limit
    - Minimum number of documents to include token is 2
    - Maximum number of documents to include token is 90%
    - Includes unigrams and bigrams
4. The other models that did well in terms of precision: Count Vectorizer & Logistic Regression, Count Vectorizer & Random Forest, TF-IDF & Random Forest. 

### Key Takeaways
1. Based on the top keywords, the best one-word keyword that describes r/CatAdvice would be: "litter". We notice a number of environment-related words, such as "home", "house", "room" - indicating the users are concerned about their cats' behavior or reaction in the environment. 
2. Meanwhile keywords for r/DogAdvice are usually two words, and we can see that a number of them focus on the dog's breed and training. This also reaffirms our earlier observation that users are more concerned about their pets' training and behavior.

## Limitations
When it comes to language and texts, it's always challenging trying to achieve near-perfect predictions on what users may say next, and what it would mean. The best way to improve our modeling outcome would be:
1. The subreddits we mined from were more focused on behavior and the environment surrounding the pets, perhaps we could consider working with moderator to run polls and prompt people to specify their exact challenges faced.
2. Alternatively, we could also work with moderators to ask users to include more information on their pets' breed, age, images, etc in their post so that there are more words we can work with.
3. Mine for data from different subreddits or forums, ideally on topics that are more polarizing so that we can train our model to better detect each topic. For example, pet medical vs pet training. 

## Conclusion
In summary, our model has helped address the challenges faced by the animal shelter:
1. Pet owners are primarily concerned with their pets' surrounding environment and training.
    - r/CatAdvice - owners are concerned with their cats' urinating outside the litter box and overall frequency. They also seek advice on food (wet food in particular).
    - r/DogAdvice - in the EDA process we saw that owners mentioning that their dogs were acting strange but it wasn't clear on the exact behavior. However, following the modeling process we can see that owners are concerned about training their dogs. Interestingly, dog owners tend to elaborate on the details more and often mention their dogs' breed.
2. The most accurate prediction model is Naive Bayes, along with the Counter Vectorization. 
    - Based on our score, we can run this model on similar dataset that are specific to cat- and dog-related behaviors and general care.
    - For the animal shelter, this model will be able to help them better predict issues that potential owners will face and the concerns that they might have.
    
## Recommendation
Through this model, we would recommend that the animal shelter takes the following steps to improve pet owners' experience when it comes to training and creating an environment that works for the owners and pets. They can do this in the following phases:
- Before adoption: Provide a checklist to help new pet owners prepare and adjust, e.g. read up on the pet's breed and history, pet-proof their homes before bringing the new pet home, learn to communicate with their pets.
- During: Provide courses to help owners train their newly adopted pets and ensure that they bond.
- Post-adoption: Schedule check-ins on the progress and offer personalized support to help owners better train/care for their pets. Collect feedback on whether the previous support were sufficient.
    
The animal shelter can also collect more information new pet owners, which can be used to train and improve our model.
