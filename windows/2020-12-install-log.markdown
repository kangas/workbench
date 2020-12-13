## 2020-12-12

I have a new personal Windows box, a Lenovo ThinkCentre M75n Nano with

- AMD® Ryzen™ 3 PRO 3300U Processor
- Integrated Radeon™ Vega Graphics
- 8 GB DDR4 2400MHz (Soldered)
- Windows 10 Pro 64

It seems like a surprisingly powerful little postage stamp of a machine!

Let's install stuff manually the first time. I'd like to try using Ansible to configure a Windows system! But right now I just want to get something usable quickly.

## Initial short list of apps

Just use the consumer-friendly installers for these

- [1Password](https://1password.com/downloads/windows/)
- [Dropbox](https://www.dropbox.com/install)
- [Firefox](https://www.mozilla.org/)
- [Chrome](https://www.google.com/chrome/)
- [Spotify](https://www.spotify.com/us/download/)
- [Sublime Text 3](https://www.sublimetext.com/)

Modern Windows hacker goodies

- [Windows Package Manager aka winget.exe](https://github.com/microsoft/winget-cli)

Utilities that I had on my last Windows system

- Sharpkeys to remap CapsLock  
- 7-Zip File Manager  
- ConEmu (improved console)

"winget install" makes installing these remarkably easy in 2020!
```
    winget install sharpkeys
    winget install 7zip
    winget install conemu
```

Lastly, some stuff to dabble in CNC:
- [Carbide3D CarbideCreate](https://carbide3d.com/carbidecreate/)
- [Vectric VCarve](https://www.vectric.com/products) ... or, another one of their products
- [Adobe Fusion 360](https://www.autodesk.com/products/fusion-360/personal) ... but license is frustrating
- Maybe [Trimble Sketchup](https://www.sketchup.com/)
- Something else?


## Remap Caps Lock to Ctrl

Ensure that Sharpkeys is installed. Launch the app.

- "Add"
  - Map this key (From key):  
    `Special: Caps Lock (00_3A)`
  - To this key (To key):  
    `Special: Left Ctrl (00_1D)`
  - "Ok"
- "Write to Registry"
- Log out, then log in and check if it works!


## Remove apps via Powershell "Run as Administrator"

- Remove Skype  
  ```
  get-appxpackage *Microsoft.SkypeApp* | remove-appxpackage   
   ```
- Remove Groove Music  
  ```
  get-appxpackage *Microsoft.ZuneMusic* | remove-appxpackage   
   ```

