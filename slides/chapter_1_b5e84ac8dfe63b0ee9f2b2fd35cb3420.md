---
title: Insert title here
key: b5e84ac8dfe63b0ee9f2b2fd35cb3420

---
## Training and evaluation

```yaml
type: "TitleSlide"
key: "44952a0c6e"
```

`@lower_third`

name: Árpád Tóth
title: Instructor at DataCamp


`@script`



---
## Our workflow

```yaml
type: "FullSlide"
key: "b031f40ed3"
```

`@part1`
1. Create an input function{{1}}
2. Define a linear model{{1}}
3. **Train and evaluate**{{2}}
4. Predict{{2}}


`@script`
In the previous section, we created our input function, defined the feature columns and our linear regressor. Now we are in the right position to train and evaluate our model. 

Moreover, as the last step we will predict miles per gallon values for the first 3 rows.

Ok, let's move on.


---
## Split the data

```yaml
type: "TwoRows"
key: "80a9a62274"
```

`@part1`
Create a training and a validation data set. Training data is used to train while validation is used to evaluate the performance.

- mtcars has 32 rows
	- train dataset has 25 rows (80%)
    - test dataset has 7 rows (20%)


`@part2`
```r
indices <- sample(1:nrow(mtcars), 
                  size = 0.80 * nrow(mtcars))
train <- mtcars[indices, ]
test  <- mtcars[-indices, ]
```


`@script`
To train the data, first, we need to split our dataset. There are a ton of ways to produce train and validation dataset. 

Probably, you are already familiar with data partitioning. The model is trained on the training dataset. The fitted model is used to predict the responses in a second dataset called the validation dataset.

Here, we chose one of the simplest solutions where we pick up 80% of the data to train and hold 20% for validation.

The mtcars dataset has 32 rows, so we create a training dataset with 25 rows (which is 80%) and a validation dataset with 7 rows (which the 20% of the original data).


---
## Recap input function

```yaml
type: "FullSlide"
key: "202c351b8f"
```

`@part1`
Take a look to our input function and batch_size.

```r
mtcars_input_fn <- function(data, num_epochs = 1) {
  input_fn(data, 
           features = c("wt", "qsec", "am"),
           response = "mpg",
           batch_size = 5,
           num_epochs = num_epochs)
}
```


`@script`
Before we call the train function, let's recap the input function we built in the previous section. As you see, we define the batch size as 5. This is because we do not feed all of our data to our model at once. However, mtcars is a small dataset, we follow real-life cases where we input small chunks to the train function. If we have 25 rows in training data, 5 batches mean we cover all of that data points in 5 steps. Since 5 times 5 is 25.

Actually, this input function will be used for evaluation and prediction too. That's why we defined it earlier to avoid code duplication.


---
## Train the model

```yaml
type: "FullSlide"
key: "1b7289f318"
```

`@part1`
We’re now ready to train our model by using the `train()` function.

```r
model %>% train(mtcars_input_fn(train, 
                                num_epochs = 10))
```

```out
[\] Training -- loss: 200.85, step: 50
```


`@script`
Now it's time to call the train function! As you see, we specify our input function with 2 parameters. The first is the data which is the training dataset. The second is the number of epochs. 

An epoch consists of going through all your training samples once. And one step refers to training over a single batch. In practice, we extend the epoch to more than 1 because one epoch is almost always never enough for minimizing loss. In practice, this number is manually tuned.

As you see, we define the number of epochs as 10. Since 1 epoch contains 5 steps, 10 epochs mean 50 steps.

The loss here is the sum of the batch losses. In the case of linear regressor the loss is defined as a mean-squared error.


---
## Evaluation

```yaml
type: "FullSlide"
key: "2887d96199"
code_zoom: 80
```

`@part1`
We can evaluate the model’s accuracy using the ```evaluate()``` function, using our ‘test’ data set for validation.

```r
model %>% evaluate(mtcars_input_fn(test))
```

```out
[\] Evaluating -- loss: 169.74, step: 2# A tibble: 1 x 5
  `label/mean` average_loss global_step `prediction/mean`  loss
         <dbl>        <dbl>       <dbl>             <dbl> <dbl>
1         19.8         29.4          50              18.6  103.
```{{2}}


`@script`
In the next step, we evaluate our model performance using the evaluate() function. Please note, that we provide test dataset as the argument of the input function.

If we execute this command, we got information about our model performance in the test dataset. The first row returns the sum of losses and the number of steps. We have only 7 rows in test dataset, so 2 steps are enough to evaluate since the batch size is 5. It means that the first batch includes 5 rows while the second only 2 rows.

The tibble what we see returns some metrics about the training status. 

While training and evaluation is often an iterative process the global_step is counter which indicates how many times we optimize the loss after process a batch. Since we call train function only once, global_step is 50 now.

Label/mean equals the mean of the mpg values in the training dataset. While prediction/mean is the mean of the predicted mpg values. We can compare them and see that our predictions are slightly lower than the real ones. 

The average_loss is the mean of the batch losses while the loss is the sum of the batch losses.


---
## Prediction

```yaml
type: "FullSlide"
key: "c71f7602b9"
```

`@part1`
After we’ve finished training our model, we can use it to generate predictions from new data.

```r
obs <- mtcars[1:3, ]
model %>% predict(mtcars_input_fn(obs))
```

```out
# A tibble: 3 x 1
  predictions
  <list>     
1 <18.5>     
2 <19.1>     
3 <20.2>  
```{{2}}


`@script`
Finally, let's predict miles per gallon values for the first 3 rows in the dataset. The predict function estimate the target values by the linear regressor we defined. As you see, here are the predicted mpg values in case of the first 3 rows in our original dataset.

Now it's your turn to practice what you learned!


---
## Let's practice!

```yaml
type: "FinalSlide"
key: "9b3351d946"
```

`@script`


