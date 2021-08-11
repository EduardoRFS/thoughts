# esy

## Manifest

esy supports two manifests, `package.json` like manifests and `package.opam` like manifests, you can even mix them.

For applications you should use `package.json` or `esy.json`.

## Opam dependencies

You can use opam dependencies by getting the package name on opam, and prefixing it as `@opam/package`, examples are `@opam/dune`, `@opam/reason`, `@opam/ppxlib`.

## Variables

esy exposes a couple useful variables on the manifest as `#{package.variable}`.

In the esy environment there is some alias to the current variables as `cur__variable`.

You can always use `self` to talk about your own package as `#{self.install}`.

### Important variables

| variable   | on shell           | manifest                | contains                                      |
| ---------- | ------------------ | ----------------------- | --------------------------------------------- |
| target_dir | `$cur__target_dir` | `#{ocaml.target_dir}`   | folder where build files will be generated    |
| root       | `$cur__root`       | `#{self.root}`          | root folder of a package                      |
| install    | `$cur__install`    | `#{@opam/dune.install}` | folder where files of a package are installed |

## Debugging

### Version

On **Windows** before 0.6.11 it was required to run it as admin.

Update to the latest and **enable developer mode**.

### VSCode

It may be that a locking problem happens which leads to your vscode clashing against an esy instance running. Normally restart language server works.

### shell

Sometimes you're already inside of `esy shell` and trying to run it again, a good way to verify that is to check if some variable like `$cur__install` is present. That means esy shell

### Missing variable

You can only use `#{package.variable}` if the package is a direct dependency, so let's say you want `#{ocaml.install}` but your manifest doesn't include `ocaml` as a dependency it will fail.

### Opam dep not working

If an opam dependency is not working it may be that a patch is missing, you can make one and open a PR at https://github.com/esy-ocaml/esy-opam-override

## FAQ

### Where are the esy variables?

You probably forgot to run `esy shell`.

### Where is my files?

Probably at `$cur__target_dir`, sometimes at the root of your project as `_build`.
