# Autorator

We are often asked to give star reviews and/or comments on goods and services we have received. However, star ratings and sentiment expressed in comments often don't match.

Here comes **AutoRator**, a machine learning-based algorithm that analyses the sentiment of a comment and categorises it as a positive, neutral or negative comment.

There were several stages to developing AutoRator:  

## Dataset
A dataset of 8 million Yelp reviews was used. Restuarant reviews that had a user comment of useful were extracted leaving 930.000 reviews and a further filter based on origion of food e.g. Italian or Vietnamese ws applied leaving a dataset of 105.000 reviews.  
Originally this was done in an sql database though for ease of use a python script is included to extract the above **notebooks/extract_data.jpynb**  

## Raw data analysis
Data ananalysis on the subset produced a few intersting insights.  
	* 	People are much more inclined top leave a review if the experience was positive
	![rating_distribution](https://github.com/Reinierandrew/Autorator/assets/112833174/dd2dc70c-82b7-4600-9b21-4c8ffc60fbcb)  
	
* 	 Mexican, Italian and Chinese show the most businesses and reviews though a lot less reviews are left for Chinese restaurants.   
* 	The top 3 also show a less than average star rating.  
* 	Japanese, Thai and particularly French restaurants receive more frequent reviews.  
* 	Spanish and Cuban and French restaurants receive the highest average ratings

 ![Origin_analysis_tableau](https://github.com/Reinierandrew/Autorator/assets/112833174/32a1e730-65d7-40c0-8f5d-2c80b4a8f383)
 
## Sentiment analysis
Two sentiment word analyses were used on the reviews. First VADER which is a rule based text analyser which rates a 'text' either positive, negative or neutral.
![compoundvaderscores](https://github.com/Reinierandrew/Autorator/assets/112833174/cb87c29a-092b-48f8-b85e-34556bef60ff)
The second analysis used on the text is EMOLEX which compares the 'text' to list of words which are ranked according to 8 emotions and 2 sentoments, positive and negative.
![emolexeallcores](https://github.com/Reinierandrew/Autorator/assets/112833174/df1acaa5-2864-4968-b9f4-90ce4419b8f8)

The graphs above show there is a much larger correlation between the Vader analysis scores and the stars given to a review than the Emolex analysis where the differences are a lot more subtle.

## Machine learning
Various models were used to attempt to generate an algorithm to predict the stars of any given review:
	1. Random Forest
	2. Keras Tokeniser
	3. Linear Support Vector
	4. Linear Regression
	5. Naive Bayes Multinomial

The results were discouraging and in all models the precision threshold of 80% was easily reached for the 1 and 5 star reviews, the remainder and particularly the 3 star reviews were well below the threshold. 

When splitting the reviews into just neagtive and positive an accuracy of 91% was achieved, and when adding a 'neutral' rating an accuracy of 84% was achieved.

The decision was made to use the positive, negative and neutral algorithm resulting from the SVM learning model.

<img width="419" alt="Screenshot 2023-07-16 at 10 18 16 am" src="https://github.com/Reinierandrew/Autorator/assets/112833174/f1322564-9eb3-4f0a-910f-a50caa5b78aa">

## Website
The final element is to apply the learning model to a review on a website. The opening page askes the user to leave a review.

<img width="1389" alt="Screenshot 2023-07-16 at 10 49 54 am" src="https://github.com/Reinierandrew/Autorator/assets/112833174/1cf8df39-e383-4180-9916-355fc4157eb4">

Using google search the user can select any location in the world to leave a review for. After input of the review the page tells the user if the review was positive, neutral or nagative and collects 5 recent reviews for the location from google and presents the sentiment of thoise reviews as well as a wordcloud of the words used in the other reviews.

 <img width="999" alt="Screenshot 2023-07-16 at 10 50 34 am" src="https://github.com/Reinierandrew/Autorator/assets/112833174/b543e181-fa6e-4249-b065-545911cfd294"><img width="998" alt="Screenshot 2023-07-16 at 10 50 53 am" src="https://github.com/Reinierandrew/Autorator/assets/112833174/ea3789b5-00a4-40a0-bcdd-514620a66657">


## Notes on how to use

Download the reviews and business csv files from `https://www.yelp.com/dataset`

Run the notebook `extract_data.jpnyb` to create the `filtered_full.csv` file which you can the use to run the `create_vader_emolex.ipynb`. Use the output `vader_emolex.csv` to run the `LR_model.ipynb` which creates the machine learning joblib files.

Navigate to the main folder in your terminal and first run `chmod a+x run.sh` and then `./run.sh` to run the flask app on a development server.


