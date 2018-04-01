# malformed mach-o: load commands size (...) > 32768

The way GHC constructs dylibs, and the way cabal places dylibs with `cabal new-build`, can
result in load-command-size blowups.

This repository is a reproduction case to illustrate the issue.

# Build

```shell
$ cat cabal.project|grep -v "^packages"|xargs cabal unpack
$ cabal new-build
```

the first command will unpack all packages listed in the `cabal.project` file; the second will then build the project with `cabal new-build`. This should (while the issue persists) yield the following (or similar error):

```
ghc: panic! (the 'impossible' happened)
  (GHC version 8.4.1 for x86_64-apple-darwin):
	Loading temp shared object failed: dlopen(/var/folders/fv/xqjrpfj516n5xq_m_ljpsjx00000gn/T/ghc75192_0/libghc_19.dylib, 5): no suitable image found.  Did find:
	/var/folders/fv/xqjrpfj516n5xq_m_ljpsjx00000gn/T/ghc75192_0/libghc_19.dylib: malformed mach-o: load commands size (34104) > 32768

Please report this as a GHC bug:  http://www.haskell.org/ghc/reportabug
```
