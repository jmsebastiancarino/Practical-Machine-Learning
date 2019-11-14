<!DOCTYPE html>



<html xmlns="http://www.w3.org/1999/xhtml">



<head>



<meta charset="utf-8" />

<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />

<meta name="generator" content="pandoc" />





<meta name="author" content="Juan Mari Sebastian Carino" />





<title>PML Project</title>













<div class="fluid-row" id="header">







<h1 class="title toc-ignore">PML Project</h1>

<h4 class="author"><em>Juan Mari Sebastian Carino</em></h4>

<h4 class="date"><em>December 8, 2018</em></h4>



</div>





<div id="executive-summary" class="section level3">

<h3>Executive Summary</h3>

<p>In this project, the author has tested four algorithms: Trees, Naive Bayes, Support Vector Machine, and Random Forest in predicting the classe of the test dataset. Cross validation was performed through 10 folds from the training dataset and through partitioning the training dataset into train (60%) and validation (40%). Out of four, only random forest generated a higher accuracy based on the confusionMatrix function of the caret package. With the accuracy as basis of choosing the model, the author concluded to use the random forest in predicting the classe of test dataset. The prediction resulted to a 95% score in the prediction quiz thereby, providing proof that the author has correctly chosen the best model for the prediction of test data.</p>

</div>

<div id="building-the-model-and-cross-validation" class="section level3">

<h3>Building the model and Cross Validation</h3>

<p>In this case, the author will predict the “Classe” for each observation in the testing dataset. There are a number of classification algorithms available in the caret package. With this, the accuracy shall be the basis of choosing the model in predicting the classe of the test dataset. The following classification algorithms that were performed and analyzed are: Trees, Naive Bayes, Support Vector Machine, and Random Forest.</p>

<p>Cross validation is performed through 10 folds. The author chose 10 folds so that, the model will have less variability and less biased. Also, the author partitioned the training dataset into train dataset for 60% and validation dataset (40%). The model that will be created based on the train dataset shall be used for the validation dataset and get the accuracy through the confusionMatrix.</p>

<pre class="r"><code># Load the training and testing datasets

training &lt;- read.csv(&quot;pml-training.csv&quot;)

testing &lt;- read.csv(&quot;pml-testing.csv&quot;)



# Subsetting the datasets (only the accelerometers and classe)

library(plyr)

library(dplyr)

training &lt;- select(training, classe, grep(&quot;accel&quot;, names(training)))

test &lt;- select(testing, grep(&quot;accel&quot;, names(testing)))



# Remove variables with no values

training &lt;- training[ , colSums(!is.na(training)) == nrow(training)]

test &lt;- testing[ , colSums(!is.na(testing)) == nrow(testing)]



# Create train and validation datasets for the cross validation

inTrain &lt;- createDataPartition(training$classe, p = 0.60, list = FALSE)

train &lt;- training[inTrain, ]

validate &lt;- training[-inTrain, ]



# Building the model

library(caret)

library(randomForest)

library(kernlab)



        # Trees

        model_1 &lt;- train(classe ~., data = train, method=&quot;rpart&quot;, trControl = trainControl(method = &quot;cv&quot;, number = 10, verboseIter = TRUE))

        accuracy_1 &lt;- confusionMatrix(predict(model_1, validate), validate$classe)

        

        # Naive Bayes

        model_2 &lt;- train(classe ~., data = train, method=&quot;nb&quot;, trControl = trainControl(method = &quot;cv&quot;, number = 10, verboseIter = TRUE))

        accuracy_2 &lt;- confusionMatrix(predict(model_2, validate), validate$classe)

        

        # Linear SVC 

        model_3 &lt;- train(classe ~., data = train, method=&quot;svmLinear&quot;, trControl = trainControl(method = &quot;cv&quot;, number = 10, verboseIter = TRUE))

        accuracy_3 &lt;- confusionMatrix(predict(model_3, validate), validate$classe)

        

        # Random Forest

        model_4 &lt;- train(classe ~., data = train, method=&quot;rf&quot;, trControl = trainControl(method = &quot;cv&quot;, number = 10, verboseIter = TRUE))

        accuracy_4 &lt;- confusionMatrix(predict(model_4, validate), validate$classe)</code></pre>

</div>

<div id="analysis-results" class="section level3">

<h3>Analysis &amp; Results</h3>

<p>Below shows the accuracy rate for each model after carrying out cross validation. Random Forest, although much slower has proven to be the most accurate garnering very high accuracy percentage. With that, we conclude that Random Forest is the best model to predict the test dataset.</p>

<pre class="r"><code># Comparing the accuracy for each models based 

data.frame(model=c(&quot;Trees&quot;, &quot;Naive Bayes&quot;, &quot;Linear SVC&quot;, &quot;Random Forest&quot;),

           accuracy = c(accuracy_1[[3]][1], accuracy_2[[3]][1], accuracy_3[[3]][1], accuracy_4[[3]][1]))</code></pre>

<pre><code>##           model  accuracy

## 1         Trees 0.4225083

## 2   Naive Bayes 0.5695896

## 3    Linear SVC 0.5535305

## 4 Random Forest 0.9455774</code></pre>

<p>The graph of Random Forest is presented as follows:</p>

<pre class="r"><code># Accuracy (Cross-Validation)

plot(model_4)</code></pre>

<p>The prediction for the test dataset shows the following:</p>

<pre class="r"><code>data.frame(Observation = 1:20, Classe = predict(model_4, testing))</code></pre>

<pre><code>##    Observation Classe

## 1            1      B

## 2            2      A

## 3            3      C

## 4            4      A

## 5            5      A

## 6            6      E

## 7            7      D

## 8            8      B

## 9            9      A

## 10          10      A

## 11          11      B

## 12          12      C

## 13          13      B

## 14          14      A

## 15          15      E

## 16          16      E

## 17          17      A

## 18          18      B

## 19          19      B

## 20          20      B</code></pre>

</div>

<div id="conclusion" class="section level3">

<h3>Conclusion</h3>

<p>In this project, 4 classification algorithms were performed and only Random Forest was able to predict correctly the test dataset (based on the results of the quiz). It is important to take into consideration the application of each algorithms. Random Forest has a slower processing time despite its high accuracy. In performing machine learning, one must carefully take note that in choosing a final model, accuracy, application, errors, variability, and bias must be looked into.</p>

</div>









</div>









</body>

</html>
