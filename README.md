# Rust Workspace Template

This project is a template containing Cargo workspace. Use it if you want to split your big project into several
subpackages, but share same settings and build it all together.

## Adding new subpackage

1. Create folder containing `Cargo.toml` and either `src/main.rs` or `src/lib.rs`
2. `Cargo.toml` content:
    ```toml
    [package]
    name = "example"
    
    version.workspace = true
    description.workspace = true
    documentation.workspace = true
    homepage.workspace = true
    repository.workspace = true
    authors.workspace = true
    keywords.workspace = true
    categories.workspace = true
    
    edition.workspace = true
    rust-version.workspace = true
    
    [lints]
    workspace = true
    
    [dependencies]
    ```
3. Add new subpackage name to `workspace.members` in root `Cargo.toml`
4. If this subpackage is needed to be dependency of other subpackages
    - Add following line to `workspace.dependencies` in root `Cargo.toml`
      ```toml
      [workspace.dependencies]
      example = { path = "example" }
      ```
    - In dependent subpackage, add this to `dependencies`
      ```toml
      [dependencies]
      example.workspace = true
      ```

## Adding external dependency

You should not add external dependency directly in subpackage's `Cargo.toml`. Instead, add it in root `Cargo.toml`:

```toml
[workspace.dependencies]
abc = "1.2.3"
# Or
abc = { version = "1.2.3" }
```

Then, add following line to subpackage's `Cargo.toml`

```toml
[dependencies]
abc.workspace = true
# Or
abc = { workspace = true }
```

## Linting and formatting

### Via command

To lint and format your whole project, use:

```shell
cargo clippy
cargo fmt
```

> Q: Why we use `nightly`?\
> A: Because some features, like disabling `trailing_comma` only available with nightly toolchain.\
> Note: it might require you to install nightly toolchain.

### JetBrains RustRover

`Settings > Rust > External Linters`

- Enable `Run external linter on the fly`
- Select `Clippy` as linter
- Select `[default]` under `Channel` selector

`Settings > Rust > Rustfmt`

- Select `[default]` under `Channel` selector
- Enable `Use Rustfmt instead of the built-in formatter`
- Click `Configure actions on save` and enable following:
    - `Reformat code`
    - `Optimize imports`
    - `Rearrange code`
    - `Run code cleanup`
