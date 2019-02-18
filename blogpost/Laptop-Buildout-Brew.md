# Install Homebrew
Homebrew is a package manager for MacOS

To install Homebrew open up terminal and paste/type in the following.

```zsh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
Verify your install by typing in brew -v to give you the version of Homebrew you installed.

## Install mas-cli
mas-cli stands for Mac App Store Command Line Interface
```
brew install mas
```
Sign into the app store
```
mas signin youremail@example.com
```
After you sign in for the first time with mas-cli, or the regular app store you shouldn’t have to sign back in again.

## Setting up a Brewfile
Automate software installations by using aBrewfile.

To create one type the following into your terminal at your home directory.

```zsh
touch Brewfile
```
```brewfile
tap 'caskroom/cask'

brew 'git'
brew 'npm'
brew 'bash'
brew 'bash-completion'
brew 'tree'
brew 'zsh'
brew 'z'
brew 'python3'brew
brew 'wget'
brew 'nmap'

cask 'visual-studio-code'
cask 'firefox'
cask 'google-chrome'
cask 'google-chrome-canary'
cask 'spectacle'
cask 'vlc'
cask 'postman'
cask 'docker'
cask 'vagrant'
cask 'virtualbox'

mas 'Slack', id: 803453959
mas 'Simplenote', id: 692867256 
mas 'Todoist', id: 585829637
mas 'Amphetamine', id 937984704

```

Type the following into your terminal
```
brew bundle install
```

Making Our Terminal Awesome
While BASH is a fantastic shell, lets take our terminal to the next level by getting zsh up and running using the oh-my-zsh framework.

ITerm2
Firstly, lets get a new terminal installed. The one I’m going to use is ITerm2. Navigate to that site, download ITerm2, and put the application into your Applications folder.


ZSH
We installed ZSH when we ran our Brew Bundle Install earlier now we need to set it as our default shell. This is easy enough to accomplish and we can do so by typing the following into our terminal.
```
chsh -s /usr/local/bin/zsh
```
oh-my-zsh
As I mentioned earlier, my favorite thing about zsh is the amazing oh-my-zsh framework we can use alongside it. In order to install oh-my-zsh type the following into your terminal
```
$ curl -L http://install.ohmyz.sh | sh
```
After running that command you should have a .zshrc file in your home directory. Open it up and you can change your theme. I personally like a solarized look and the agnoster theme lends itself to this nicely.
```
ZSH_THEME="agnoster"
```
In order to get the theme completely setup you need to be using a font with ITerm2 that supports the characters it uses. I recommend Fira Code for this.

Finally to complete the look set the color scheme for ITerm2 to Solarized Dark by going into Preferences > Profiles > Colors > Color Presets.

Reload your terminal or type source ~/.zshrc and you should be good to go, but there are still a couple more fun things we can do.

zsh-syntax-highlighting
Okay so zsh syntax highlighting is really cool, once you have it setup it will tell you if a command is valid before you enter it. to install it type the following into your terminal

cd ~/.oh-my-zsh && git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
then add this line to your .zshrc file

source ~/.oh-my-zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
restart your terminal or run source ~/.zshrc and you’re good to go.

Z Script
Earlier we installed z script, so now lets get it running. All you need to do is add this line to your ~/.zshrc file and hit save.
```
. `brew --prefix`/etc/profile.d/z.sh
```
It’s really cool because it keeps track of the places you’ve cd’d into and stores them in a ~/.z file so that you can easily navigate to them in the future. For example, instead of doing cd ~/someplace/someproject/source you can simply type z source.


Preferences
Before getting started I found setting these preferences helpful.

Keyboard > Text > Disable “Correct Spelling Automatically”
Security and Privacy > FileVault > On (Encrypt your drive)
Security and Privacy > Firewall > On (who doesn’t like a little extra security?)
Security and Privacy > General > App Store and Identified Developers
File Sharing > Off
Users & Groups > Login Items > Spectacle, Flux (I like these on at startup)
Finally some commands to make your life easier

Show Library Directory
```
chflags nohidden ~/Library
```
Show Hidden Files
```
defaults write com.apple.finder AppleShowAllFiles YES
```
Show Path Bar
```
defaults write com.apple.finder ShowPathbar -bool true
```
Show Status Bar
```
defaults write com.apple.finder ShowStatusBar -bool true
```
