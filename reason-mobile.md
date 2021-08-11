# Reason Mobile

## Debugging

### Clash between interfaces

Probably happens because the package OR some of it's dependencies was compiled to the host instead of to the target.

To fix it you should try to use a **dune-universe** version of the package if available.

- https://github.com/dune-universe/

If that is not available, try porting the package to dune and if possible opening a PR on upstream.

If not possible or too complex, making a patch for reason-mobile is also okay.

### Patch failed

Quite likely that a dependency was updated and the patch no longer works. Fix the patch locally and open a PR.

## FAQ

### Why it takes so long to build?

It's essentially rebuilding the universe for the platform target, so it takes a while, but rebuilds should be quite fast, still slower than building to host.
