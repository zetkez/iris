# iris

[![GitHub CI](https://github.com/chshersh/iris/workflows/CI/badge.svg)](https://github.com/chshersh/iris/actions)
[![Hackage](https://img.shields.io/hackage/v/iris.svg?logo=haskell)](https://hackage.haskell.org/package/iris)
[![MPL-2.0 license](https://img.shields.io/badge/license-MPL--2.0-blue.svg)](LICENSE)

**Iris** is a Haskell framework for building CLI applications that follow
[Command Line Interface Guidelines](https://clig.dev/).

> 🌈 Iris (/ˈaɪrɪs/) is a Greek goddess associated with communication, messages,
> the rainbow, and new endeavors.

<picture>
  <source media="(prefers-color-scheme: dark)"  srcset="https://raw.githubusercontent.com/chshersh/iris/main/images/iris-dark.png">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/chshersh/iris/main/images/iris-light.png">
  <img alt="Iris changing her workflow and hair colour depending on the time of day." src="https://raw.githubusercontent.com/chshersh/iris/main/images/iris-dark-always.png">
</picture>

> ℹ️ **DISCLAIMER #1:** Currently, Iris is in experimental phase and
> mostly for early adopters. It may lack documentation or have
> significant breaking changes. We appreciate anyone's help in
> improving the documentation! At the same time, the maintainers will
> strive to provide helpful migration guides.

> ℹ️ **DISCLAIMER #2:** Iris is developed and maintained in free time
> by volunteers. The development may continue for decades or may stop
> tomorrow. You can use
> [GitHub Sponsorship](https://github.com/sponsors/chshersh) to support
> the development of this project.

## Goals

Iris development is guided by the following principles:

1. **Support [Command Line Interface Guidelines](https://clig.dev/).**
   Features or changes that violate these guidelines are not accepted
   in the project.
2. **Beginner-friendliess.** Haskell beginners should be able to build
   CLI applications with Iris. Hence, the implementation of Iris API
   that uses less fancy Haskell features are preferred. When the
   complexity is justified, the cost of introducing this extra
   complexity should be mitigated by having better documentation.
3. **Reasonable batteries-included.** Iris is not trying to be
   minimalistic as possible, it strives to provide out-of-the-box
   solutions for most common problems. But at the same time, we don't
   want to impose unnecessary heavy dependencies.
4. **Excellent documentation.** Iris documentation should be as
   helpful as possible in using the framework.

   > **NOTE:** Currently, Iris may lack documentation but there's an
   > ongoing effort to improve the situation.

🧱 Iris focuses solely on CLI applications. If you're interested in
building TUI app with Haskell, check out
[brick](https://hackage.haskell.org/package/brick).

## Features

CLI apps built with Iris offer the following features for end users:

* Automatic detection of colouring support in the terminal
* Ability to check required external tools if you need e.g. `curl` or
  `git`
* Support for standard CLI options out-of-the-box:
    * `--help`
    * `--version`
    * `--numeric-version`: helpful for detecting required tools versions
    * `--no-input`: for disabling all interactive features
* Utilities to open files in a browser

## How to use?

`iris` is compatible with the following GHC
versions - [supported versions](https://matrix.hackage.haskell.org/#/package/iris)

In order to start using `iris` in your project, you
will need to set it up with these steps:

1. Add the dependency on `iris` in your project's
   `.cabal` file. For this, you should modify the `build-depends`
   section according to the below section:

   ```haskell
   build-depends:
     , base ^>= LATEST_SUPPORTED_BASE
     , iris ^>= LATEST_VERSION
   ```

2. To use this package, refer to the below example.

   ```haskell
   {-# LANGUAGE GeneralizedNewtypeDeriving #-}

   module Main (main) where

   import Control.Monad.IO.Class (MonadIO (..))

   import qualified Iris


   newtype App a = App
       { unApp :: Iris.CliApp () () a
       } deriving newtype
           ( Functor
           , Applicative
           , Monad
           , MonadIO
           )

   appSettings :: Iris.CliEnvSettings () ()
   appSettings = Iris.defaultCliEnvSettings
       { Iris.cliEnvSettingsHeaderDesc = "Iris usage example"
       , Iris.cliEnvSettingsProgDesc = "A simple 'Hello, world!' utility"
       }

   app :: App ()
   app = liftIO $ putStrLn "Hello, world!"

   main :: IO ()
   main = Iris.runCliApp appSettings $ unApp app
   ```

## For contributors

Check [CONTRIBUTING.md](https://github.com/chshersh/iris/blob/main/CONTRIBUTING.md)
for contributing guidelines.

## Development

To build the project and run the tests locally, you can use either
`cabal` or `stack`.

> See the [First time](#first-time) section if you don't have Haskell
> development environment locally.

### Cabal

Build the project:

```shell
cabal build all
```

Run all unit tests:

```shell
cabal test --enable-tests --test-show-details=direct
```

### Stack

Build the project:

```shell
stack build --test --no-run-tests
```

Run all unit tests:

```shell
stack test
```

### First time

If this is your first time dealing with Haskell tooling, we recommend
using [GHCup](https://www.haskell.org/ghcup/).

During the installation, GHCup will suggest you installing all the
necessary tools. If you have GHCup installed but miss some of the
tooling for some reason, type the following commands in the terminal:

```shell
ghcup install ghc 8.10.7
ghcup set     ghc 8.10.7
ghcup install cabal 3.6.2.0
```

> If you are using Linux or macOS, you may find `ghcup tui` command a
> more attractive option.
