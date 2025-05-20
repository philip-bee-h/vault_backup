
### Reduce Window Animation Duration

```sh
defaults write NSGlobalDomain NSWindowResizeTime -float 0.05

```


### Speed Up Mission Control Animations

```sh
defaults write com.apple.dock expose-animation-duration -float 0.1; killall Dock

```



### Disable Dock Bounce Animations

```sh
defaults write com.apple.dock no-bouncing -bool TRUE; killall Dock
```


Re-enable

```sh
defaults write com.apple.dock no-bouncing -bool FALSE; killall Dock
```


### Speed Up Dock Auto-Hide Animation

To remove a slight delay when auto-hiding is enabled.

```sh
defaults write com.apple.dock autohide-delay -float 0; killall Dock
```

For an ultra-fast pop-up effect, also speed up the animation:

```sh
defaults write com.apple.dock autohide-time-modifier -float 0.1; killall Dock
```

To reset to default:

```sh
defaults delete com.apple.dock autohide-delay
defaults delete com.apple.dock autohide-time-modifier
killall Dock
```


