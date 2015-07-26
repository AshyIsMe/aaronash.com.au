```
title: Hello World in Haskell: The easy way
layout: post
tags: ['Haskell', 'OSX', 'post']
```

## New package management tool Stack
Recently FPComplete released a new package management tool called [stack](https://github.com/commercialhaskell/stack).
This makes getting up and running with Haskell development a hell of a lot nicer than it was previously with cabal alone.
Cabal is so hairy that there is a well know term "cabal hell" used to describe some of the common problems that it can cause.

Haskell is a pretty amazing language that has a lot of good ideas and I think it should be used a lot more in the industry.
One of the things that has been holding it back from wider adoption has been the confusing and frustrating state of the main package management tool cabal.
Stack is looking like a very nice solution to that problem and I hope a lot more people start experimenting with Haskell now.


## How to get up and running as a newbie (on osx/linux)

Daniel Mlot recently wrote an article explaining a nice workflow with stack: [casual hacking with stack](https://duplode.github.io/posts/casual-hacking-with-stack.html).
It can be a bit strange to get set up with Haskell at first so I thought I would put together a few simple steps for Haskell beginners.

### Install stack
Follow the [instructions](https://github.com/commercialhaskell/stack#how-to-install) for installing stack on your platform.  This should be nice and easy for OSX/linux.  (I haven't set up Haskell on windows yet so I'm not sure how well supported it is there.)

You should now have the stack binary on your $PATH somewhere (most likely in ~/bin/).
```bash
$ stack --version
Version 0.1.2.1, Git revision 09a9fa46fd1d8174ececf4a53774a05b36641457
```
Note: You may have a different version but that's ok.

### Create a new project
Now you are ready to make a new haskell project!
```bash
$ mkdir hello-haskell
$ cd hello-haskell/
$ stack new

Writing: LICENSE
Writing: Setup.hs
Writing: app/Main.hs
Writing: hello-haskell.cabal
Writing: src/Lib.hs
Writing: test/Spec.hs
Writing default config file to: /Users/aaron/CodeBases/hello-haskell/stack.yaml
Basing on cabal files:
- /Users/aaron/CodeBases/hello-haskell/hello-haskell.cabal

Checking against build plan lts-2.17
Selected resolver: lts-2.17
Wrote project config to: /Users/aaron/CodeBases/hello-haskell/stack.yaml

```

We make a new directory hello-haskell and then run `stack new` within it.  Stack creates a template project named after the directory.
```bash
$ ls -R
LICENSE             Setup.hs            app                 hello-haskell.cabal src                 stack.yaml          test

./app:
Main.hs

./src:
Lib.hs

./test:
Spec.hs

```

Let's build and run the new project:
```bash
$ stack build
hello-haskell-0.1.0.0: configure
Configuring hello-haskell-0.1.0.0...
hello-haskell-0.1.0.0: build
Building hello-haskell-0.1.0.0...
Preprocessing library hello-haskell-0.1.0.0...
[1 of 1] Compiling Lib              ( src/Lib.hs, .stack-work/dist/x86_64-osx/Cabal-1.18.1.5/build/Lib.o )
In-place registering hello-haskell-0.1.0.0...
Preprocessing executable 'hello-haskell-exe' for hello-haskell-0.1.0.0...
[1 of 1] Compiling Main             ( app/Main.hs, .stack-work/dist/x86_64-osx/Cabal-1.18.1.5/build/hello-haskell-exe/hello-haskell-exe-tmp/Main.o )
Linking .stack-work/dist/x86_64-osx/Cabal-1.18.1.5/build/hello-haskell-exe/hello-haskell-exe ...
hello-haskell-0.1.0.0: install
Installing library in
/Users/aaron/CodeBases/hello-haskell/.stack-work/install/x86_64-osx/lts-2.17/7.8.4/lib/x86_64-osx-ghc-7.8.4/hello-haskell-0.1.0.0
Installing executable(s) in
/Users/aaron/CodeBases/hello-haskell/.stack-work/install/x86_64-osx/lts-2.17/7.8.4/bin
Registering hello-haskell-0.1.0.0...

$ stack exec hello-haskell-exe
someFunc
```

So the template app is built and prints out "someFunc" when you run it.  Let's change that.
Open up app/Main.hs in your favourite editor (Vim and Emacs seem to be the most popular for Haskell development currently).

This is what it looks like currently:
```haskell
module Main where

import Lib

main :: IO ()
main = someFunc

```

Change the main function to this:
```haskell
main = do
  putStrLn "hello world!"

```

Now build and run it again:
```haskell
$ stack build
$ stack exec hello-haskell-exe
hello world!
```

If you want to add a library from the list of stackage packages all you need to do is add it to the `build-depends` section of the `haskell-hello.cabal` file.
The list of available packages is here: https://www.stackage.org/lts-2.19
```
...

executable hello-haskell-exe
  hs-source-dirs:      app
  main-is:             Main.hs
  ghc-options:         -threaded -rtsopts -with-rtsopts=-N
  build-depends:       base
                     , hello-haskell
  default-language:    Haskell2010

...
```

For example let's display some [cpu information](https://www.stackage.org/lts-2.19/package/cpu-0.1.2).  Modify the `build-depends` section to look like this:
```
...
  build-depends:       base
                     , cpu
                     , hello-haskell
...
```

Save the file and run `stack build` again. Stack will download the package and install it ready to use.
We can now use the cpu package in the Main.hs file.  Alternatively we can also open up the ghci repl and import the cpu package.
```
$ stack ghci
Configuring GHCi with the following packages: hello-haskell
GHCi, version 7.8.4: http://www.haskell.org/ghc/  :? for help
Loading package ghc-prim ... linking ... done.
Loading package integer-gmp ... linking ... done.
Loading package base ... linking ... done.
Loading package cpu-0.1.2 ... linking ... done.
[1 of 1] Compiling Lib              ( /Users/aaron/CodeBases/hello-haskell/src/Lib.hs, interpreted )
Ok, modules loaded: Lib.
λ: import System.Arch
λ: getSystemArch
X86_64
```

The ghci repl can be quite handy for quick testing cycles while you're working on a project.
Let's add a simple function to the Main module and reload the code into the ghci repl without having to restart ghci:
```haskell
module Main where

import Lib
import System.Arch

main :: IO ()
main = do
  putStrLn "hello world!"

showSystemArch = "The cpu architecture is: " ++ (show getSystemArch)

```

Now back in ghci we can run `:reload` to recompile and then have access to our new function:
```
λ: :reload
Ok, modules loaded: Lib.
λ: :l Main
[1 of 2] Compiling Lib              ( /Users/aaron/CodeBases/hello-haskell/src/Lib.hs, interpreted )
[2 of 2] Compiling Main             ( /Users/aaron/CodeBases/hello-haskell/app/Main.hs, interpreted )
Ok, modules loaded: Lib, Main.
λ: showSystemArch
"The cpu architecture is: X86_64"
```

You now should have the basic tools set up to start hacking on Haskell projects easily.  There's a lot of great free resources for learning Haskell, some good ones are:

- http://www.seas.upenn.edu/~cis194/spring13/lectures.html
- http://learnyouahaskell.com/

Once you have the basics down [Ollie Charles'](https://ocharles.org.uk/blog/) blog has a great exploration of some of the more popular Haskell libraries currently in use:

- [24 Days of Hackage 2012](https://ocharles.org.uk/blog/pages/2012-12-01-24-days-of-hackage.html)
- [24 Days of Hackage 2013](https://ocharles.org.uk/blog/pages/2013-12-01-24-days-of-hackage.html)



