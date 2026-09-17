# Themes Base
    
## Install Themes 

Install base
```sh
curl -fsSL https://linuxthemes.org/install | sh
```

Safe to run on a machine where `$HOME` already contains a git repository:
the installer adopts the existing repo, points `origin` at the Themes remote,
backs up your current settings in a local branch, then switches
to `base`.

## Use Themes

Update themes 
```sh
themes-update
```

Switch themes 
```sh
themes-switch nord
```

Make your own theme
```sh
themes-create thebesttheme
```
Push to your own git repository
```sh
themes-remote https://YOUR_RESPOSITORY.com/base
```



https://linuxthemes.org/

![Alt](https://repobeats.axiom.co/api/embed/ff6ce752ed4e193227810cd238820d385fcda408.svg "Repobeats analytics image")



