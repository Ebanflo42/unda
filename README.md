 <img align="left" src="https://raw.githubusercontent.com/BradenEverson/unda/master/unda.svg" width="75px" height="75px" alt="unda icon">

# Unda

### General purpose neural network crate

[![crates.io](https://img.shields.io/crates/v/unda.svg)](https://crates.io/crates/unda)
[![Documentation](https://docs.rs/unda/badge.svg)](https://docs.rs/unda)
[![Unit Tests](https://github.com/BradenEverson/unda/actions/workflows/rust.yml/badge.svg)](https://github.com/BradenEverson/unda/actions/workflows/rust.yml)

Unda aims to bring the future of deep learning to the world of rust. With dynamic input traits, concurrent minibatch processing, and full Dense network support(with convolutions soon to come), Unda is quickly emerging and making neural network development easy and ***blazingly fast***.

## Installation

Use the package manager [cargo](https://crates.io/) to add [unda](https://crates.io/crates/unda) to your rust project.

```bash
cargo add unda
```

or add the dependency directly in your **cargo.toml** file

```toml
[dependencies]
unda = "{version}"
```

## Usage

```rust
use unda::core::network::Network;
use unda::core::layer::{methods::activations::Activations, layers::{LayerTypes, InputTypes}};
use unda::core::data::input::Input;
use unda::core::layer::{methods::errors::ErrorTypes};

fn main() {
    let inputs = vec![vec![0.0,0.0],vec![1.0,0.0],vec![0.0,1.0], vec![1.0,1.0]];
    let outputs = vec![vec![0.0],vec![1.0],vec![1.0], vec![0.0]];

    let mut new_net = Network::new(4);

    new_net.set_input(InputTypes::DENSE(2))
    new_net.add_layer(LayerTypes::DENSE(3, Activations::RELU, 0.001));
    new_net.add_layer(LayerTypes::DENSE(1, Activations::SIGMOID, 0.001));

    new_net.compile();

    new_net.fit(&inputs, &outputs, 2, ErrorTypes::MeanAbsolute);

    println!("1 and 0: {:?}", new_net.predict(vec![1.0,0.0])[0]);
    println!("0 and 1: {:?}", new_net.predict(vec![0.0,1.0])[0]);
    println!("1 and 1: {:?}", new_net.predict(vec![1.0,1.0])[0]);
    println!("0 and 0: {:?}", new_net.predict(vec![0.0,0.0])[0]);

    new_net.save("best_network.json");
}
```


## Examples
The unda repository hosts a plethora of example ML models to compute a series of common problems. These examples can be found in the ```/examples``` folder and can be run by entering:

```
cargo run --release --example {example_name}
```
where ```example_name``` is the name of the file/folder you wish to run, omitting the .rs

### Currently, Unda has example implementations for XoR, MNIST and a [breast cancer model from Kaggle](https://www.kaggle.com/datasets/yasserh/breast-cancer-dataset)

**Important**! When using running the MNIST example, please make sure to put the appropriate ubyte files into the /src/util/mnist directory of this repository. We are currently working on using reqwest to automatically build the dataset, but for now it must be done manually

Here are google drive links to the necessary ubyte files
- [labels](https://drive.google.com/file/d/191BR4awTN-XvIISPeB4_zaHJZ0EgC4-o/view?usp=drive_link)
- [images](https://drive.google.com/file/d/1vsltbfn7D3ZYFmAhN2fexomUaqr5oG6P/view?usp=drive_link)

## Implications for the future of ML

Using the built in **Input** trait, practically any data type can be mapped to an input for a neural network without the need for cutting corners, and the inner trait for layers allows for a plug and play style to neural network development. Currently, Unda has full support for Dense layers, Adam Optimization for Backprop, Activation functions (Sigmoid, TanH, ReLU and LeakyReLU), and even loss analysis per model and per layer. 

Gradient descent currently can happen both syncronously as stochastic gradient descent or asynchronously through minibatch gradient descent. 

## TODO

Currently, Unda is in a very beta stage, the following features are still in development:

[Neural Network Goals]
- [X] Create abstract representation for layers (Layer trait)
    - [X] Dense
    - [ ] Convolutional
        - [X] Cateogorical Crossentropy
        - [X] SoftMax
    - [ ] Recurrent
- [X] Allow for different activation functions and learning rates on each layer
- [X] Adam Optimization in backprop
- [X] Helper Function for parsing CSV data
- [X] Helper Function for generating the MNIST dataset
- [X] Helper Functions for generating and deriving categorical data

#### If open source development is your thing, we at Unda would love additional work on anything that can be implemented, please contact **eversonb@msoe.edu** if you'd like to help out!

# License
Licensed under the Apache License, Version 2.0 http://www.apache.org/licenses/LICENSE-2.0 or the MIT license http://opensource.org/licenses/MIT, at your option. This file may not be copied, modified, or distributed except according to those terms.
