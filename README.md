# Description

This repository provides nightly builds for [pandoc](https://github.com/jgm/pandoc/releases). The main purpose is for testing a bug report before filing an issue.
(Or if you want to use the latest features of pandoc, but nightly is not even beta, be warned.)

Go to [the releases page](https://github.com/pandoc-extras/pandoc-nightly/releases/latest) to download pandoc for your platform (`pandoc-amd64-...` is for Linux).
Only the pandoc executables are provided. Once you downloaded it, you can unzip it and run by specifying the absolute path of the pandoc executables explicity:

```bash
# For example:
~/Downloads/pandoc/.stack-work/install/x86_64-osx/lts-7.14/8.0.1/bin/pandoc ... # your usual pandoc options here
```

For Windows users, the binaries are mirrored from [the official nightly build from appveyor](https://ci.appveyor.com/project/jgm/pandoc/build/artifacts), depending on your needs, you can either download the `.msi` for full installation (remember this is nightly, not even beta), or `.zip` for testing.

# Versioning

The commit hash of the pandoc's latest commit is used as the version here, as you can see in the [releases page](https://github.com/pandoc-extras/pandoc-portable/releases). The version follows the pattern of `hash-<commitHash>`.

For example, version (git tag) `hash-1dc4b3925d96e26b89a1a30f36ab809662e283ad` uses the source from <https://github.com/jgm/pandoc/commit/1dc4b3925d96e26b89a1a30f36ab809662e283ad>. The `.zip` also follows the same pattern, so you can easily traced it back to which commit the binary is compiled against.

The versions of the script and travis setup in this repository will be in this `README.md` file. Current version: 0.2

# License

The original license from pandoc is copied here, which is in GPLv2+.

# Potential Improvements

If you feel like improving any of these, please feel free to make a pull request.

## Versioning can be off by up to an hour

The git tag version, the version in the `osx`|`amd64` binaries, and the version in the `windows` binaries might not agree. The `osx`|`amd64` binaries are always built with the latest pandoc commit in the master branch, which can be updated since it was git-tagged. The `windows` binaries are mirrored from the latest appveyor build, which might be out-dated.

In practice, these different versions probably differs by at most an hour. And the hash in the `osx`|`amd64` binaries is always accurate.

## Git submodule

May be a better way to handle this is to add pandoc as a git submodule to this repository. A script in Travis can be added to update the submodule and auto-commit it to the project. Then there's a clear history of commits in this repository reflecting on the commits used from pandoc.
