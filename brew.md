brew install wget

arch -arm64 brew install

brew install Checkmarx/tap/kics

[https://tomlankhorst.nl/brew-bundle-restore-backup/](https://tomlankhorst.nl/brew-bundle-restore-backup/)

[https://apple.stackexchange.com/questions/101090/list-of-all-packages-installed-using-homebrew](https://apple.stackexchange.com/questions/101090/list-of-all-packages-installed-using-homebrew)

[https://www.youdriveai.com/assets/cheatsheets/cheatsheet-homebrew.pdf](https://www.youdriveai.com/assets/cheatsheets/cheatsheet-homebrew.pdf)


## Create BrewFile

`````bash
# install homebrew bundle
brew tap Homebrew/bundle

# create BrewFile
brew bundle dump

# Install packages
brew bundle

`````

## Remove Tap

`````
brew untap abdennebi-forks/homebrew-regula
`````

## Docker 
`````
brew install --cask docker
`````
