---
layout: post
title: "Gaussion Processes Regression. Step by Step" 
date: 2020-10-01 10:33:00
categories: jekyll update
---

I try to explain a step-by-step process of Gaussion process (GP) regression by a one dimentional numerial example. You might know the maths behind the GP but explain a few equations which is needed for the example.

Lets say we have $$n$$ independent features and one dependent variable. Independent features are $$x_{1} = [x_{11}, x_{12}, x_{13}], x_{2} = [x_{21}, x_{22}, x_{23}] $$ and dependent variable is $$y = [y_{1}, y_{2}, y_{3}]$$. This means we have a multivariable Gaussian of three variables. 
GP regression is an 'interpolation' and gives errors based on a multivariate Gaussion distribution of dependent variable. 

We are considering a function $$f(x_{1})$$ with $$x_{1}$$ as the independent variable. This means that $$f(x_{1})$$ has a normal errors, i.e. $$ e_{x11}, e_{x12}, e_{x13} $$ at $$x_1$$. There is another variable $$x_{2}$$ and $$f(x_{2})$$ has errors which is different from the errors $$f(x_{1})$$ and so on. What it is useful is that we need these errors to understand the multivariate Gaussion. Eg. we have only two variables and $$y(x_{11}, x_{21})$$ is observed in that surface of $$x_1$$ and $$x_{2}$$. If we have only one variable then as you that the normal error is in the 1D. However, for a

Let's say we have a 1d example

<style>
table {
    width:10%;
}
</style>


|x|y
|---|---|
{% for gp in site.data.gp -%}
|{{ gp.x }} | {{ gp.y}} 
{% endfor %}  




I used text [data](/assets/recipes.csv) from [allrecipes](https://www.allrecipes.com) for this analysis. I have scraped food recipes of different cuisines from the website. Apart from the text I used a few different numerical parameters. After cleaning up the text I used a tokenizer to find the most used words in different cuisines. I found that pepper is the most popular ingredient in all the cuisines. I used TFIDF procedure to classify the recipes to different cuisines based on its ingredients.  I found that the correlation between cuisine can be found from confusion matrix based on their ingredients. You can view my analysis in this [Jupyter notebook](https://github.com/vinuvikraman/nlp/blob/master/recipes.ipynb)
  

## Future direction
This project is equally fun and interesting. Currently, chefs are classifying the recipes based on their own cuisine. Even though there are closer ingredients found in recipes of different cuisines. There is also an increasing availability of fusion food. It will be interesting if we can classify the recipe to any cuisine  based on their ingredients.

## Detailed figures and their explaination

# Most used words in different cuisines

* Words in these figures are appropriate for these cuisines. Here I show the number of words in Italian cuisins which have a lot of cheese, olive oil, black pepper etc. If you are interested to such figures from different cuisins please go to this [link]({% post_url 2019-07-29-recipes-more-figs %})

![Italian](/assets/italian.png)



* I count the words in these cuisines and the distribition is shown. It was found that the pepper is the most used ingredients. However, most of the dishes don't use pepper or garlic as main ingredients but many use it for flavoring the recipe which makes pepper or garlic as their unavoidable ingredient. This result made me happier as I am from a part of India where the first ever pepper was traded in the world!

![Word count](/assets/word_count.png)

Please see in the bottom of this page to find out more count separated by other food related parameters such as calories, ratings etc


# Modeling cuisins from ingredients
* I use these word count to check whether I can classify their respective cuisines. I used scikit-learn TFIDF to find features from the ingredient. Then I found a model which classify cuisins from the ingredients. I have used a few different model on the TFIDF such as logistic regression, MultinomialNB based Bayasian and XGBoost. For these algorithms I obtained 0.56, 0.55 and 0.6 scores. I should remind you that the correlation matrix, as shown below, have identical ingredients which make the classification difficult. Since there are not much data I didn't want to go for cross validation.  

# Confusion matrix
 
* Confusion matrix as correlation of ingredients between different cuisines

![Confusion matrix](/assets/confusion_matrix_xgb.png)  

After the classification I generated this interesting plot. I looked at the data and found that there are many sweet recipes in Danish cuisine. There are mostly similar ingredients in several sweet recipe. This confusion shows that for sweet recipes there are much more similarity in different cuisines. One of my future analysis is to remove sweet dishes.



# More figures on word count
* There are many recipes are in the high calorie area. I took recipies which gives calorie greater than 800 per person and found the ingredients. You can find some of the ingredients from all recipes but many product from milk, oil are in top list.
 
![High Calorie Word count](/assets/high_calorie_word_count.png)

* Found words from recipes which are highly rated

![High rating word count](/assets/high_review_word_count.png)

* Found words from recipes which are highly made (> 5000). It is found that the words are mostly matching the sweet recipes.  

![High made word count](/assets/high_made_word_count.png)

* Found words from recipes which are highly rated (>= 4). 

![Highly rated word count](/assets/high_review_word_count.png)


 
