# NPM Playbook (Node Package Manager)

__Course:__ 
https://app.pluralsight.com/library/courses/npm-playbook

---------------------

### What is a package?

__Module__ - a *single* javascript file that has some reasonable functionality

__Package__ - a *directory* w/ one or more modules inside of it and a *package.json* file that has metadata about the package.

-packages can be very simple (may have a single js file or dozens)

### Typical usage 

#### npm install
-clone a user's source code then do the below command to install their dependencies. 
-This will install any dependencies noted in the package.json file for that directory.

```
npm install
```

#### npm start
-runs a project if the script is setup in the package.json file under the 'scripts' object.

#### npm test
-runs tests created for that project if the script is setup in package.json (like above).

### Getting Help

- help <command> will open a html page will helpful facts and documentation about the command.

```
npm help <command>

npm help install

```

#### Searching for help

-use help-search command to find help topics

```
 npm help-search <list of terms to search>

 npm help-search remove package

```

### Shortcuts

https://docs.npmjs.com/misc/config

-------------

## Reasons for Package.JSON file

1) Track Dependencies  2) Create scripts (helps you avoid installing grunt/gulp just to start your project up)

### Create Package.JSON

```

//create new package.json through wizard

npm init

//create new package.json and auto-accept all defaults

npm init -y


```

### Install Dependency
-use --save or -S flags during *npm install* command to instruct npm to add installed module to package.json file.

```
npm i lodash -S //i = install, -S = save dependency
```

###Install DEV-only dependency
-to install dev-only dependency (like karma testing framework), you'll need to save it into your dev dependencies list in package.json.

```
npm install karma --save-dev 

npm i karma -D //same thing
```

### Check installed packages
-use the *list* command to get a list of installed dependencies (and their dependencies) in a graphical tree format.

```
npm list
npm ls //same thing

```

### Install package globally
-package gets installed into a special directory on your pc (instead of your current working directory)

```
npm install <package> -g


npm ls -g --depth 0 //see global packages you've installed
```

### Uninstall package 

```
npm uninstall <package> //removes package from node_modules

npm un <package> --save //removes package and removes dependency from package.json

npm un <package> -g //removes package from global

```

### Install specific version
-without version number on install, latest version is used.

```
npm i underscore@1.8.3 //install specific patch version (v3)

npm i underscore@1.8 //install latest patch version under 1.8

npm i underscore@1.x //install latest minor version of major version 1 (1.8.3)

npm i underscore@">=1.1.0 <1.4.0" //install latest version greater than or equal to 1.1.0, but less than 1.4.0

npm i express@"<4" //install latest express less than version 4 (3.9.9)

```

-when you install a package w/ latest version, it'll save it in package.json as 
*"dep": "^1.8.3"* meaning that npm will update this to the latest version whenever an update to it is made

-to save an exact version and *not* allow npm to update it, we use the below command. This will install the dep w/ a version number *without* the upcarret.

```
npm i <package>@1.8.2 --save --save-exact //save exact here will keep npm from updating the version

```

-latest version of major release is __^__ ("^1.8" = "1.8.3")

-latest version of minor release is __~__ ("~1.5.1" = "1.5.3")

-exact version is __no extra character__


### Updating packages

```
npm update //installs latest version of dependencies 

npm update <package> //installs latest version for only this package

```