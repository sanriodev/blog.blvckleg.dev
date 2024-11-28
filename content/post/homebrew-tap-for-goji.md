+++
title = 'Homebrew Tap for Goji'
date = 2024-11-28T18:45:49+01:00
draft = false
pin = true
+++

## Goji can now be installed with Homebrew

Hey,

I recently created a CI Pipeline for the [goji repository](https://github.com/sanriodev/goji) that automatically creates a homebrew formulae for goji.

So now the goji binaries can finally be installed without needing go installed on your local machine.

to add the homebrew tap simply run

```zsh
brew tap sanriodev/homebrew-goji
```

and install goji via homebrew like so

```zsh
brew install goji
```

This allows you to simply install goji and future packages from this tap if you wanna try them out on linux and macOS.
