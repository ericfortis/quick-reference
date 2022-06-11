# macOS


## Typing
- “ Alt + {
- ” Alt + Shift + {
- º Alt + 0

## Show the top level installed apps with homebrew
```shell
brew leaves
```

## Trust Self-Signed cert
- Open Keychain Access
- Select login keychains
- Drop localhost.cert into the app
- Select the Localhost cert, expand Trust, select "Always Trust"
- Restart the browser

## SVG
In Finder, the preview is cropped to the drawing size (as opposed to
page) when it only has viewBox (it shouldn't have width and height)


## Cmd+Ctrl+Click Drag Window
https://mmazzarolo.com/blog/2022-04-16-drag-window-by-clicking-anywhere-on-macos/

Enable
```shell
defaults write -g NSWindowShouldDragOnGesture -bool true   
```

Disable
```shell
defaults delete -g NSWindowShouldDragOnGesture  
```

## Find user full name
```shell
id -F
```