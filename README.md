# Description

This repository provides nightly builds for [pandoc](https://github.com/jgm/pandoc/). The main purpose is for testing a bug report before filing an issue.
(Or if you want to use the latest features of pandoc, but nightly is not even beta, be warned.)

Go to [the releases page](https://github.com/pandoc-extras/pandoc-nightly/releases/latest) to download pandoc for your platform (`pandoc-amd64-...` is for Linux).
Only the pandoc executables are provided. Once you downloaded it, you can unzip it and run by specifying the absolute path of the pandoc executables explicity:

```bash
# For example:
~/Downloads/pandoc-osx-5729f1f2eaf1665be3fcbf92917503bdd15d1995/pandoc ... # your usual pandoc options here
```

For Windows users, the binaries are mirrored from [the official nightly build from appveyor](https://ci.appveyor.com/project/jgm/pandoc/build/artifacts), depending on your needs, you can either download the `.zip` for testing or the `.msi` for full installation (remember this is nightly, not even beta).

# Versioning

The commit hash of the pandoc's latest commit is used as the version here, as you can see in the [releases page](https://github.com/pandoc-extras/pandoc-portable/releases). The version follows the pattern of `hash-<commitHash>`.

For example, version (git tag) `hash-1dc4b3925d96e26b89a1a30f36ab809662e283ad` uses the source from <https://github.com/jgm/pandoc/commit/1dc4b3925d96e26b89a1a30f36ab809662e283ad>. The `.zip` also follows the same pattern, so you can easily traced it back to which commit the binary is compiled against.

The versions of the script and travis setup in this repository will be in this `README.md` file. Current version: 0.2

Caveat: if you run `<pathTo>pandoc --version`, it will show the version of the latest stable release (because this is coded in the source code of pandoc). There's nothing much I can do about it, and is another reason you should not install the `.msi` (otherwise you will forget it is not stable).

# License

The original license from pandoc is copied here, which is in GPLv2+.

# Potential Improvements

If you feel like improving any of these, please feel free to make a pull request.

## Versioning can be off by up to an hour

The hash uses in these 3 instances:

1. git tag version (as shown in the [releases page](https://github.com/pandoc-extras/pandoc-portable/releases)),
2. in the names of the `pandoc-osx|amd64-...` binaries, and
3. in the name of the `pandoc-windows-...` binaries

might not agree.

Technically,

1. a daily Travis cron job triggers the creation of the git tag in (1), using the hash of the latest pandoc commit in this build.
2. the new git tag triggers a new Travis job, at the time of the job, it creates (2), where the macOS and Linux build runs in parallel. Each of the hash in the name of the `pandoc-osx|amd64-...` binaries are the hash of the latest pandoc commit in their respective build.
3. In addition, the Appveyor's artifacts are mirrored in the Linux build, and uses the hash found in that build.  

In short, only the hashes in the (2) is guaranteed to be correct.

In practice, these different versions (commit hashes) probably differs by at most an hour.

## Git submodule

May be a better way to handle this is to add pandoc as a git submodule to this repository. A script in Travis can be added to update the submodule and auto-commit it to the project. Then there's a clear history of commits in this repository reflecting on the commits used from pandoc.

## Travis Cron Job

Note to anyone who make a commit:

>Because cron jobs build the latest commit to a particular branch, if that commit message includes `[ci skip]` or `[skip ci]` the cron job will skip that build. From [Cron Jobs - Travis CI](https://docs.travis-ci.com/user/cron-jobs/).

i.e. remember not to use these keywords in your commit, otherwise if the latest commit seen by the cron job includes these keywords, the cron job will always be skipped indefinitely (until a new commit).

e.g. cron job on 2017-01-21 is skipped because of commit b18110a.
