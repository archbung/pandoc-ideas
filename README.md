# Aim
We would like `pandoc` to be fully extensible with Lua.


# Ideas

## Exposing `http-client` to Lua

When working with scientific documents, it is often convenient to be able to look up author informations (given some ID) automatically, for example via ORCID. 
It is therefore necessary for pandoc to have a http client subsystem.
`http-client` is a popular library for writing http clients in Haskell and thus a good choice for implementing a http client subsytem in pandoc and to expose its functions to Lua.


# Technical Details

We rely on `hslua`, which provides bindings between Lua and Haskell, to expose Haskell functions to Lua.
Exposing Haskell functions to Lua amounts to making them an instance of `ToHaskellFunction` typeclass, with the following instances

  ```haskell
  ToHaskellFunction HaskellFunction
  Pushable a => ToHaskellFunction (Lua a)
  (Peekable a, ToHaskellFunction b) => ToHaskellFunction (a -> b)
  ```

- `HaskellFunction` is the type of Haskell functions that can be called from Lua.
  It is actually an alias for `Lua NumResults`, where `Lua` represents a Lua computation and `NumResults` is the number of results of that Lua computation.
- `Pushable` is the type of values that can be pushed into the Lua stack.
- `Peekable` represents the type of values that can be read from the Lua stack.


