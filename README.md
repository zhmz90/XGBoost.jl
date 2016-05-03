XGBoost.jl
==========

[![Build Status](https://travis-ci.org/zhmz90/XGBoost.jl.svg?branch=master)](https://travis-ci.org/zhmz90/XGBoost.jl)
[![XGBoost](http://pkg.julialang.org/badges/XGBoost_0.4.svg)](http://pkg.julialang.org/?pkg=XGBoost&ver=0.4)
[![XGBoost](http://pkg.julialang.org/badges/XGBoost_0.5.svg)](http://pkg.julialang.org/?pkg=XGBoost&ver=0.5)

eXtreme Gradient Boosting Package in Julia

## Abstract

This package is a Julia interface of [XGBoost](https://github.com/tqchen/xgboost),
which is short for eXtreme gradient Gradient Boosting.  It is an efficient and scalable implementation of
gradient boosting framework.The package includes efficient linear model
solver and tree learning algorithms. The library is parallelized using OpenMP,
and it can be more than 10 times faster some of than existing gradient boosting packages.
It supports various objective functions, including regression, classification and ranking.
The package is also made to be extensible, so that users are also allowed to define their own objectives easily.

## Features
* Sparse feature format, it allows easy handling of missing values, and improve computation efficiency.
* Advanced features, such as customized loss function, cross validation, see [demo folder](demo) for walkthrough examples.

## Installation
```julia
Pkg.add("XGBoost")
```
or
```julia
Pkg.clone("https://github.com/antinucleon/XGBoost.jl.git")
Pkg.build("XGBoost")
```


The `XGBoost` package also depends on the `BinDeps`


## Minimal examples

To show how XGBoost works, here is an example of dataset Mushroom

- Prepare Data

XGBoost support Julia ```Array```, ```SparseMatrixCSC```, libSVM format text and XGBoost binary file as input. Here is an example of Mushroom classification. This example will use the function ```readlibsvm``` in [basic_walkthrough.jl](demo/basic_walkthrough.jl#L5). This function load libsvm format text into Julia dense matrix.

```julia
using XGBoost

train_X, train_Y = readlibsvm("data/agaricus.txt.train", (6513, 126))
test_X, test_Y = readlibsvm("data/agaricus.txt.test", (1611, 126))

```

- Fit Model
```julia
num_round = 2
bst = xgboost(train_X, num_round, label=train_Y, eta=1, max_depth=2)
```

## Predict
```julia
pred = predict(bst, test_X)
print("test-error=", sum((pred .> 0.5) .!= test_Y) / float(size(pred)[1]), "\n")
```

## Cross-Validation
```julia
nfold=5
param = ["max_depth"=>2, "eta"=>1, "objective"=>"binary:logistic"]
metrics = ["auc"]
nfold_cv(train_X, num_round, nfold, label=train_Y, param=param, metrics=metrics)
```

## Feature Walkthrough
Check [demo](https://github.com/antinucleon/XGBoost.jl/blob/master/demo/)

- [Basic walkthrough of features](demo/basic_walkthrough.jl)
- [Cutomize loss function, and evaluation metric](demo/custom_objective.jl)
- [Boosting from existing prediction](demo/boost_from_prediction.jl)
- [Predicting using first n trees](demo/predict_first_ntree.jl)
- [Generalized Linear Model](demo/generalized_linear_model.jl)
- [Cross validation](demo/cross_validation.jl)


## Model Parameter Setting
Check [XGBoost Wiki](https://github.com/tqchen/xgboost/wiki)
