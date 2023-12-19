# Chrome

## Fullscreen in macOS 
Ctrl + Cmd + F

## For live-reload speed:
Set up a persona without plugins. `ReactDevTools` is
rarely needed, so it's better to enable it when needed.

## Webpack caches
There's no need to disable caches in the Network Panel.
It's ok because our resources are hash-named, even in dev using the following:
```js
const Chunks = ['node_modules', 'card', 'entry']
const ChunkSuffix = '-chunk.js'

webpackConfig.setupMiddlewares = (middlewares, { app }) {
    app.use(`/*${ChunkSuffix}`, (_, response, next) => {
        response.setHeader('Cache-Control', 'max-age=31536000')
        next()
    })
    return middlewares
}
    
webpackConfig.optimization: {
    runtimeChunk: true,
    splitChunks: {
        filename: `[name]-[chunkhash]${ChunkSuffix}`,
        cacheGroups: Chunks.reduce((acc, dir) => {
            acc[dir] = {
                test: RegExp(dir),
                name: dir,
                chunks: 'initial',
                enforce: true
            }
            return acc
        }, {})
    }
}
```


## Filtering the Console
Newer Chrome versions no longer support regex filter but to exclude a word, **prefix it with '-'**

```text
-HMR -link/react-devtools
```

## Uninstalling PWAs (and settings)
```
chrome://apps
```


## Firefox
https://stackoverflow.com/questions/11704828/how-to-allow-keyboard-focus-of-links-in-firefox

## Safari
Advanced -> A11y -> Press Tab to highlight




## Flush Browser DNS Cache
### Hosts file
`sudoedit /etc/hosts` needs to exit `:wq` (`:w` alone doesn't work)

### Chrome
chrome://net-internals/#dns

### Firefox
about:networking#dns


## Chrome
#### Redirect to https in dev
On the console just Right-Click → Clear Browser Cache (it won't
delete the IndexedDB). In this case trying to delete the HSTS record in
Chrome's internals (chrome://net -internals/#hsts) doesn't do anything.


#### Opening a new Chrome instance
Handy if it's stuck in the one bound to WebStorm's Debug.
- `google-chrome -na` (Ubuntu)
- `open -n https://example.com` (macOS)

#### Override System Proxy Settings
```shell
alias chrome='open -a "Google Chrome" --args --proxy-server='
```



## Safari
#### Allow tabbing to buttons
Preferences → Advanced → "Press Tab to highlight each item on a webpage"


## Remote Debugging (Android)
For developing/testing touch with a Galaxy phone using Chrome's Remote devices.
chrome://inspect/#devices

Phone setup:
- Ensure **Developer options** appears in **Settings** (the last item)
    - if not Settings → About → **Build Number** (click it 7 times)
- **Developer options** → **USB Debugging**
- **Developer options** → **USB configuration** → PTP
- A good USB is important, not all of them work.

