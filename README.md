# xLearn

[xLearn](https://github.com/aksnzhy/xlearn) - the high performance machine learning library - for Ruby

:fire: Uses the C API for blazing performance

Supports:

- Linear models
- Factorization machines
- Field-aware factorization machines

[![Build Status](https://travis-ci.org/ankane/xlearn.svg?branch=master)](https://travis-ci.org/ankane/xlearn)

## Installation

First, [install xLearn](https://xlearn-doc.readthedocs.io/en/latest/install/index.html). On Mac, copy `build/lib/libxlearn_api.dylib` to `/usr/local/lib`.

Add this line to your application’s Gemfile:

```ruby
gem 'xlearn'
```

## Getting Started

Prep your data

```ruby
x = [[1, 2], [3, 4], [5, 6], [7, 8]]
y = [1, 2, 3, 4]
```

Train a model

```ruby
model = XLearn::Linear.new(task: "reg")
model.fit(x, y)
```

Use `XLearn::FM` for factorization machines and `XLearn::FFM` for field-aware factorization machines

Make predictions

```ruby
model.predict(x)
```

Save the model to a file

```ruby
model.save_model("model.bin")
```

Load the model from a file

```ruby
model.load_model("model.bin")
```

Save a text version of the model [master]

```ruby
model.save_txt("model.txt")
```

Pass a validation set

```ruby
model.fit(x_train, y_train, eval_set: [x_val, y_val])
```

Get the bias term, linear term, and latent factor [master]

```ruby
model.bias_term
model.linear_term
model.latent_factor # fm and ffm only
```

## Parameters

Specify parameters

```ruby
model = XLearn::Linear.new(k: 20, epoch: 50)
```

Supports the same parameters as [Python](https://xlearn-doc.readthedocs.io/en/latest/all_api/index.html)

## Cross-Validation [master]

Cross-validation

```ruby
model.cv(x, y)
```

Specify the number of folds

```ruby
model.cv(x, y, folds: 5)
```

## Data

Data can be an array of arrays

```ruby
[[1, 2, 3], [4, 5, 6]]
```

Or a Daru data frame

```ruby
Daru::DataFrame.from_csv("houses.csv")
```

Or a Numo NArray

```ruby
Numo::DFloat.new(3, 2).seq
```

## Performance

For large datasets, read data directly from files

```ruby
model.fit("train.txt", eval_set: "validate.txt")
model.predict("test.txt")
model.cv("train.txt")
```

For linear models and factorization machines, use CSV:

```txt
label,value_1,value_2,...,value_n
```

Or the `libsvm` format (better for sparse data):

```txt
label index_1:value_1 index_2:value_2 ... index_n:value_n
```

> You can also use commas instead of spaces for separators

For field-aware factorization machines, use the `libffm` format:

```txt
label field_1:index_1:value_1 field_2:index_2:value_2 ...
```

> You can also use commas instead of spaces for separators

You can also write predictions directly to a file

```ruby
model.predict("test.txt", out_path: "predictions.txt")
```

## xLearn Installation

There’s an experimental branch that includes xLearn with the gem for easiest installation.

```ruby
gem 'xlearn', github: 'ankane/xlearn', branch: 'vendor', submodules: true
```

Please file an issue if it doesn’t work for you.

You can also specify the path to xLearn in an initializer:

```ruby
XLearn.ffi_lib << "/path/to/xlearn/lib/libxlearn_api.so"
```

> Use `libxlearn_api.dylib` for Mac and `xlearn_api.dll` for Windows

## Credits

This library is modeled after xLearn’s [Scikit-learn API](https://xlearn-doc.readthedocs.io/en/latest/python_api/index.html).

## History

View the [changelog](https://github.com/ankane/xlearn/blob/master/CHANGELOG.md)

## Contributing

Everyone is encouraged to help improve this project. Here are a few ways you can help:

- [Report bugs](https://github.com/ankane/xlearn/issues)
- Fix bugs and [submit pull requests](https://github.com/ankane/xlearn/pulls)
- Write, clarify, or fix documentation
- Suggest or add new features
