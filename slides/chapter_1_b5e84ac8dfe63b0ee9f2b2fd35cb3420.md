---
title: Insert title here
key: b5e84ac8dfe63b0ee9f2b2fd35cb3420

---
## Launch graph and fit the line

```yaml
type: "TitleSlide"
key: "44952a0c6e"
```

`@lower_third`

name: Árpád Tóth
title: Instructor at DataCamp


`@script`
Hi There! This is a test script


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
In the previous section we created our input function, defined the feature columns and our linear regressor. Now we are at the right position to train and evaluate our model. 

Moreover, as a last step we will predict miles per gallon for the first 3 rows.

Ok, let's move on.


---
## Split the data

```yaml
type: "TwoRows"
key: "80a9a62274"
```

`@part1`
Create a training and a validation data set
- Training data is used to train
- Validation is used to evaluate the performance


`@part2`
```r
indices <- sample(1:nrow(mtcars), 
                  size = 0.80 * nrow(mtcars))
train <- mtcars[indices, ]
test  <- mtcars[-indices, ]
```


`@script`
To train the data, first, we need to split our dataset. There are a ton of ways to produce train and validation set. 

Probably, you are already familiar with data partitioning. The model is trained on the training dataset. The fitted model is used to predict the responses in a second dataset called the validation dataset.

Here, we chose one of the simplest solutions where we pick up 80% of the data to train and hold 20% for validation.


---
## Train the model

```yaml
type: "FullSlide"
key: "202c351b8f"
```

`@part1`
We’re now ready to train our model, using the ```train()``` function.

```r
mtcars_input_fn <- function(data, num_epochs = 1) {
  input_fn(data, 
           features = c("disp", "cyl"), 
           response = "mpg",
           batch_size = 32,
           num_epochs = num_epochs)
}
```

```r
model %>% train(mtcars_input_fn(train, 
                                num_epochs = 10))
```{{2}}

```out
[/] Training -- loss: 3743.89, step: 8
```{{3}}


`@script`
Now it's time to call the train function. In the previous section we built an input function called mtcars_input_fn. Now it's time to use it! As you see at the bottom code segment, we specify our input function with 2 parameters. The first, is the data with is the training dataset. The second is the number of epochs. 

We complete 1 epoch when the model has iterated through all the batches once. In practice, we extend the epoch to more than 1. But one epoch is almost always never enough for the loss to converge. In practice, this number is manually tuned.

As you see we define the number of epochs as 10.


---
## Evaluation

```yaml
type: "FullSlide"
key: "2887d96199"
code_zoom: 100
```

`@part1`
We can evaluate the model’s accuracy using the ```evaluate()``` function, using our ‘test’ data set for validation.

```r
model %>% evaluate(mtcars_input_fn(test))
```

```out
[-] Evaluating -- loss: 1491.39, step: 1# A tibble: 1 x 5
  `label/mean` average_loss global_step `prediction/mean`  loss
         <dbl>        <dbl>       <dbl>             <dbl> <dbl>
1         22.5         213.          50              10.2 1491.
```{{2}}


`@script`
In the next step, we evaluate our model accuracy using the evaluate() function. Please note, that we provide test dataset as the argument of the input function.

If you execute this command we got information about our model performance. [Detail the columns here]


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
1 <10.8>     
2 <10.8>     
3 <7.58>   
```{{2}}


`@script`
Finally, let's predict miles per gallon consumption for the first 3 rows in the dataset. The predict function estimate the target values by the linear model we defined. As you see here are the mpg values in the sample output.


---
## Let's practice!

```yaml
type: "FinalSlide"
key: "9b3351d946"
```

`@script`


