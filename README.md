# Turbo Buildpack

Turbo repos are made up of multiple applications and packages, each application should be run as an individual server in its own isolated runtime. With platforms like Heroku this can be challenging given the limited orchestration support for containerisation.

This buildpack aims to strip a turbo repo down to the minimal runtime requirements for one app in a repository that could have many. The approach is brutish yet simple, given a project of shape:

```
.
└── Project/
    ├── apps/
    │   ├── auth
    │   ├── catalogue
    │   ├── checkout
    │   └── docs
    └── packages/
        ├── analytics
        ├── clients
        ├── common
        ├── linting
        └── ui
```

In the instance you only want to deploy `checkout`, you can simply add a `Procfile` to the app's respective folder, then specify the app name using an evironment variable configured in Heroku `APP_NAME`. This buildpack will remove all other apps from the project, ensure you have this buildpack loaded first in your application's buildpack settings in Heroku. Otherwise, native Heroku buildpacks may try compile all apps prior to them being removed.
