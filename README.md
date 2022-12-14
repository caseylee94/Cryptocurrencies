# Cryptocurrencies

The purpose of this report is to use unsupervised machine learning to determine which cryptocurrencies are on the trading market and how they can be grouped to create a classification system to develop a cryptocurrency investment portfolio.

The dataset for this project is sourced from [CyptoCompare](https://min-api.cryptocompare.com/data/all/coinlist) and contains data about the number of coins mined, the coin supply, the algorithm, the proof type, and the name of the coin. This raw data is not ideal and needed to be processed before it could be entered in the machine learning model. A clustering algorithm was used to group the cryptocurrencies and data visualizations were made to show the results.

## Analysis

To process the data using Python, the csv file was loaded into a Jupyter Notebook file as a data frame. First, only the cryptocurrencies that are currently being traded were isolated. Then, of these only the ones with a working algorithm were kept. Already, this has dropped the dataset from an initial 1252 rows to 1144 rows. Then, the "IsTrading" column is removed because only the rows where this value is "True" were kept. All rows that have a null value were dropped. Now the dataset has 532 rows. The "Coin Name" column was made into its own data frame and then dropped from the main data frame because it does not need to go into the machine learning model. Finally, the data was encoded using the "get dummies" pandas function and standardized using the standard scaler function from sklearn. 

The next step in the data analysis is to reduce the dimensions of the dataset using Principal Component Analysis, or PCA. The purpose of PCA is to reduce a large dataset into a smaller, simpler dataset while still preserving as much information as possible. This will allow the data exploration in the machine learning model to run efficiently without extraneous variables to process. The accuracy of the model could be slightly reduced using PCA rather than the original dataset, but it is a fair trade for a more efficient model when using large datasets. In this analysis, the Python library sklearn is used to import a PCA model and apply it to the dataset using three principal components. These PCA results are then saved in a new data frame.

Next, the cryptocurrencies were clustered using a K-means algorithm. The purpose of this model is to find groups within a dataset that have not been labeled already in the dataset (unsupervised). When the model is initialized, the number of clusters must be preset. To find the best value for the number of clusters (K), an elbow curve using hvplot was created from the PCA data frame. Where the elbow occurs is usually the threshold for the clustering the majority of variances in the dataset. The "elbow" curves at four so this will be the cluster number for this dataset. 

![Elbow_curve.png](/Resources/Elbow_curve.png)
*<p align="center">Figure 1: Elbow curve to find the ideal cluster number for the PCA data frame</p>*

Finally, the model is initialized, fit, and run. The output of the K-means unsupervised model gives predictions for which cluster each cryptocurrency is the best fit. These predictions are called the "Class" and are added to the main data frame.

All of these numbers and changes to the dataset can make it hard to follow what exactly has been found after running this model. This is where visualizations come to play! A 3D scatter plot is next created using hvplot (showing the X, Y, and Z axes) of the clusters found in the K-means model. The plot is interactive and shows a clear picture of how the different clusters are broken up. The majority of the data are in clusters zero and three; cluster two only has one data point! This indicates that this data point should be further analyzed and potentially removed as an outlier. Class one is smaller than zero and three but has five data points grouped together so it is a substantial cluster. Each data point on the graph can be hovered over using a mouse and metrics of that point (name, PCA points, cluster, and algorithm) are shown to allow the user to identify single points. 

![3D_scatter.png](/Resources/3D_scatter.png)
*<p align="center">Figure 2: 3D scatter plot of the clusters found with the K-means model</p>*

Next, an interactive table with all the tradable cryptocurrencies is created also using hvplot. Each column name can be selected to sort the data frame, allowing the user to look at whichever metrics that they are most interested in. In the image shown below, it is sorted by the "Coin Name" column in alphabetical order.

![hvdataframe.png](/Resources/hvdataframe.png)
*<p align="center"> Figure 3: Table with cleaned data and cluster number </p>*

The final visualization created is a 2D scatterplot of the scaled "Total Coins Mined" and "Total Coin Supply" columns with the "Coin Name" added as a hover metric, allowing the user to identify each data point. The one data point that is in class two is shown as an even more extreme outlier in this plot! 

![2D_scatter.png](/Resources/2D_scatter.png)
*<p align="center">Figure 4: 2D scatter plot of Total Coins Mined versus Total Coin Supply</p>*

For further analysis of this data it should be removed and the K-means model run again to see how the four clusters will group the data without this data point. Class three also has an outlier, the "Turtle Coin" cryptocurrency. It is clustered within class three yet looking at the scatter plot it is obvious the total coin supply is much higher for this cryptocurrency. Further research into why this is the case should be done and this data point could also be removed, the K-means model run again, and those results analyzed to see the overall impact on that data. It would allow for a larger scale of this 2D scatterplot at the least; with these two outliers, the rest of the data is bunched together.

## Summary

This project has completed the primary objective of using unsupervised machine learning to classify different cryptocurrencies on the market to begin understanding their relationships. This analysis has uncovered the main areas where cryptocurrencies are typically overlapping, as well as some outliers. To further understand these relationships, this model should be tweaked and ran again without the outliers, and the outliers themselves should be researched to try to understand why this discrepancy is so high. 
