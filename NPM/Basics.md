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