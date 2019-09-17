# Configuration: A Glossary

A guide to the configuration files for projects: where they live and what
they do. 

## The root folder

- `.editorconfig`: Sets the default configuration for certain files across editors. (e.g. indentation)

- `.gitattributes`: Normalizes how `git`, the version control system this boilerplate uses, handles certain files.

- `.gitignore`: Tells `git` to ignore certain files and folders which don't need to be version controlled, like the build folder.

- `.travis.yml` and `appveyor.yml`: Continuous Integration configuration<br/>
  This boilerplate uses [Travis CI](https://travis-ci.com) for Linux environments
  and [AppVeyor](https://www.appveyor.com/) for Windows platforms, but feel free
  to swap either out for your own choice of CI.

- `package.json`: Our `npm` configuration file has three functions:

  1.  It's where Babel and ESLint are configured
  2.  It's the API for the project: a consistent interface for all its controls
  3.  It lists the project's package dependencies

  Baking the config in is a slightly unusual set-up, but it allows us to keep
  the project root as uncluttered and grokkable-at-a-glance as possible.
