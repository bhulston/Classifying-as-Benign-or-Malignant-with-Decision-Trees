# Classifying-as-Benign-or-Malignant-with-Decision-Trees

This is a dataset with 10 features, that describe features derived from images of breast masses. They describe the characteristics of the cell nuclei

Each object in the dataset has a value of 2 or 4, 2 for benign and 4 for objects that are malignant.

The goal is to correctly classify an object based on its attributes.

## Data cleaning
We remove the id as it is not helpful, remove null values, and set the results column as a binary 0,1 for benign, malignant.

We also standardize the values, though it's helpful to note that for decision trees this step isn't really necessary

## The data

Below is a pair plot that shows the relationships between the size attributes. I thought this was important because from my personal knowledge, size is a common indicator of malignant tumors
<img width="379" alt="size_pair_plot" src="https://user-images.githubusercontent.com/79114425/203873082-dfdfae80-10d9-4ca1-81f7-d0a6d1f92aaf.png">

Below is the distribution of (some!) of the features, split by the benign and malignant. As you can see, malignant tumors are generally associated with larger values for the given attributes
<img width="334" alt="Feature_deistribution" src="https://user-images.githubusercontent.com/79114425/203873400-305029e8-3a7c-4934-877a-e70d10aed3ef.png">


Another thing I checked was the "adequacy" of the data. Based on a research paper, "Induction of Decision Trees" written by J.R. Quinlan, it is ideal for decision trees to be used in datasets where the same set of attributes of an object, always results in the same classification
We check this in the code, finding that though there are some duplicates (where attributes are all the same), there are no instance where the attributes are the same BUT the classification is different.

## The model

The model we use is a classification decision tree from scikit-learn which uses the CART algorithm. Meaning that these are binary trees, where each leaf only has up to two children. If we built this with an ID3 algorithm based model, we might see different results
<img width="821" alt="9depth_decision_tree" src="https://user-images.githubusercontent.com/79114425/203873712-9ff66cf5-8e48-47f9-b4c4-226a91514cbe.png">

Running it initially, we find that there is a depth of 9, and an accuracy of 94.16%. We improve on this by customizing some of the default hyper-parameters in scikitlearn.
First we increase the values of min_samples_leaf and min_samples_split, which increases the value of accuracy to 95.62%

Doing this is a form of regularization, that reduces overfitting of the model when running it against the test set. In general, increasing the min_* features and decreasing the max_* features will regularize the model.

Note that these values might change because there is some probability in how trees are constructed, leading to slightly different results.

## Hyperparameter tuning
We further tune the hyperparameters by optimzing the max_depth. We test depths from 4 to 9, seeing as how there was depth of 9 when no limit was set. This is another form of preventing overfitting.

Because of the differences from each run, we run the models 100 times and take the average to get a more realistic model accuracy from each hyperparameter set


<img width="317" alt="tuning" src="https://user-images.githubusercontent.com/79114425/203874237-ce1276a9-40f4-4bed-8905-b58e81c7e98e.png">

## Conclusion

We find that the optimal max_depth is 6! by a small margin. By regularizing and tuning, we were able to improve the overall model accuracy to nearly 96%


