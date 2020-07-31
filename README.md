[![Build Status](https://travis-ci.com/theimpossibleastronaut/configster.svg?branch=trunk)](https://travis-ci.com/theimpossibleastronaut/configster)

# configster

Library for parsing configuration files

[configster on crates.io](https://crates.io/crates/configster)

## Config file format

The 'option' can be any string with no whitespace.

```
arbitrary_option = false
max_users = 30
```

The value for an option following the equal sign may have "attributes"
that are separated by a delimiter. The delimiter is specified when
calling parse_file():

    parse_file("./config_test.conf", ',')


```
option = Blue, light, shiny
# option = nothing, void, empty, commented_out
```

An option is not required to be followed by a value. It can be used to disable a default feature.

```
FeatureOff
```

## API

Calling parse_file() will return a single vector containing a struct
(OptionProperties) for each option line in the config file. The
attributes for a value are stored in a vector within the "Value"
struct.

```
#[derive(Debug, PartialEq)]
pub struct Value {
    pub primary: String,
    pub attributes: Vec<String>,
}

#[derive(Debug, PartialEq)]
pub struct OptionProperties {
    pub option: String,
    pub value: Value,
}
```

## Example Code

```
/// use configster::parse_file;
/// use std::io;
///
/// fn main() -> Result<(), io::Error> {
///
///     let config_vec = parse_file("./config_test.conf", ',');
///     if config_vec.is_err() {
///         return io::Result::Err(config_vec.unwrap_err());
///     }
///
///     for i in &config_vec.unwrap() {
///         println!("Option:'{}' | value '{}'", i.option, i.value.primary);
///
///         for j in &i.value.attributes {
///             println!("attr:'{}`", j);
///         }
///         println!();
///     }
///     Ok(())
/// }
/// ```

```

See [docs.rs/configster/](https://docs.rs/configster/0.1.0/configster/fn.parse_file.html)
for generated API documentation.

# Contributing

See [CONTRIBUTING.md](https://github.com/theimpossibleastronaut/configster/CONTRIBUTING.md)
