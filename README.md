# User Actions
When users want to download a package from a private registry, they should go through the next steps:
 
The user needs to add the registry to the list of registries by env var or by adding a config file.

example config file:
```toml
[registries]
my-registry = { index = "https://github.com/yardenfi/cargo-index.git" }   
```

This file should be located in `~/.cargo/config`
The name of the registry we have just added is "my-registry"...

Now the user can use the new registry to download the package. 
Example `Cargo.toml` file:
```toml
[package]
name = "my_lib"
version = "0.1.0"

[dependencies]
hello-world-x-rust-x-library = {version = "0.1.2", registry = "my-registry"}
```

Notice that in the last line, where we have added a new dependency, we've used the private registry.  
 
# Running our own registry
The registry consists of 3 parts:

    1. A git repo containing metadata about all the packages and the urls of the servers below (clauses 2. and 3.)
    2. A server that will serve the `.crate` files (crates compiled and packed for installation)
    3. (optional) An API that will allow the user to upload packages dynamically.

### The git repo
The git repo what we'll need will contain both the metadata about our package and where to get it from.
It should contain a file named `config.json` at it's root.
This file will contain the url of the server that will serve the `.crate` 
files and optionally the url to the server that will allow uploads.

