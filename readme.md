
# foundation_remapping

foundation_remapping is [Hammerspoon](http://www.hammerspoon.org/) configuration script which remaps keys at lower level than usual.

This is simple one-key to one-key remapper, but almost all keys can be replaced, including left and right individually recognized modefier keys and PC spcific keys.

Remapping mechanism is here:
[Technical Note TN2450: Remapping Keys in macOS 10.12 Sierra)](https://developer.apple.com/library/content/technotes/tn2450/)

This script requires Sierra.

## Install

Place `foundation_remapping.lua` into `~/.hammerspoon/`

## Constructor

```lua
local FRemap = require('foundation_remapping')
local remapper = FRemap.new()
```

Constructor without option returns global remapper. Remapping will be applied to any device.  
To apply only to a specific device, pass the `vendorID` and `productID` to the constructor.

```lua
local remapper = FRemap.new({vendorID=0x4d9, productID=0x4545})
```
## Adding configuration

```lua
-- syntax
-- :remap(fromKey, toKey)
remapper:remap('a', 'b') -- when press a, type b
remapper:remap('NFER', 0x66) -- Muhenkan to Eisuu
remapper:remap('lalt', 'lcmd'):remap('lcmd', 'lalt') -- swap left cmd and alt
```

Pass original keyCode and new keyCode normally used in Hammerspoon. In addition you can use the following PC spcific keys.

`'PrintScreen'` `'ScrollLock'` `'Pause'` `'Insert'` `'Application'` `'XFER'` `'NFER'` `'Henkan'` `'Muhenkan'` `'PCKana'`

and alias of individual modefier keys.

`'lctrl'` `'lshift'` `'lalt'` `'lcmd'` `'rctrl'` `'rshift'` `'ralt'` `'rcmd'`

## Registraion

It is not yet valid by just `remap()`. Ultimately `register()` will allow to use.  
**NOTICE: Remapping is effective until system termination even after quit Hammerspoon.**

```lua
remapper:register()
```

Disabling remapping temporarily or permanently, call `unregister()`.

```lua
remapper:unregister()
```

## Other


To check currently registered values, execute this command on the terminal.

```sh
hidutil property --get 'UserKeyMapping'
```
or
```sh
hidutil property --filter '{"ProductID":xxxx,"VendorID":xxxx}' --get 'UserKeyMapping'
```

Example (my configuration)
```lua
FRemap = require('foundation_remapping')
remapper = FRemap.new({vendorID=0x4d9, productID=0x4545})
remapper:remap('Muhenkan', 0x66):remap('Henkan', 'f18')
remapper:remap('PCKana', 'rcmd')
remapper:remap('lalt', 'lcmd'):remap('lcmd', 'lalt')
remapper:remap('Application', 'escape')
remapper:register()
```

## License

WTFPL 2.0

## Author

hetima  
https://twitter.com/hetima
