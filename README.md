# modrepo
Tools for managing and deploy porteus modules

This lite version deals basically with publishing a repository of modules (.xzm files) and using several repositories to search for modules and install them

From the point of view of modrepo lite a repository is a collection of modules with a special file (the REPO file) listing all modules in repository with metadata

An user may add repositories she's interested in to the source repository list (aka source list or repository list) in modrepo config file. 

Now she can search for modules in all repositories add to the repository list and also download modules based in certain criteria.

Also the user may decide to set up her own repository and make it public, so she can create a repositoy and upload content to a public site.

There's helper actions in order to init a repository (make a REPO file), upload contents and also serve contents from local machine.

## Repositories

Every repository is identified by a repo name which is a short name usually the user name owning the repository

The active repositories are kept in repomod.conf file, one line per repository:

  repo repo-name repo-url [repo-dir]

deactivate a repository implies commenting or deleting its line

Every repository must have a repo-name, usually a repo-url where the repo is publicly available and a local dir with modules (if not provided is set by default)

A repository with url only is the usual case for a public source repository, a repository with local dir only is a local repository (not very used) and a repository with url and local dir is usually a cooking repository where user is making modules to make them public via url (note you can have a cooking repo without listing it in source list if you are not interested in using it as modules source)

The repository consist in a set of files (modules) and a metada file (the REPO file). 

Users wanting to maintain a repository of modules need a folder where put all modules to publish, create the REPO file and upload all modules to a public site

Init a repository means creating the REPO file in the repo directory
Upload a repository means uploading all files in repo to the public site

These two actions are the most common unes for module cookers

Any directory may be used as a repository dir but there's a default location for all repositories in repomod.conf

There's no need to add a repository to the list of repositories if the user only publish it but she's not interested in using it as a source of modules (but it doesn't hurt if you add it)

Users only interested in repositories as a source of modules have to add the wanted repositories to source list in order to use them as source for modules. 


## Actions

- NOTA: dos acciones init y upload o solo una, upload, que haga ambas cosas? -> solo una

| Action  | Parameters	 	      | Description                                             | 
| --------|---------------------------|---------------------------------------------------------|
| upload  | name / dir		      | Uploads all modules in repo to public site              | 
| serve   | name / dir	              | Serves repository content by HTTP (as a web server)     | 
| add     | name url [dir]	      | Adds repo to source list                                | 
| list	  | all / name / regex        | Lists modules of repos                                  | 
| search  | [name/regex:]module/regex | Search for modules in repositories                      | 
| sources |		              | List active repositories                                | 
| get	  | [name/regex:]module/regex |	Downloads selected modules                              | 
| install | [name:]module [opt]	      |	Install modules in porteus system (optional or modules) | 

First two actions are intented for cookers while all the rest are useful for all users.

### action semantics

add     - adds repo to modrepo.conf, if dir is not provided defaults to current dir
get     - download modules from repository, same pattern scheme than search option
          All modules are downloaded to modrepo temp directory without activating
install - Installs a module in porteus system (modules dir by default) 
          module is a literal module name that can be prefixed with a repo name to search only inside that repo
list    - list modules in repos, several search options available:
           * all   - list all modules in all repos
           * name  - list all modules in repo name 
           * regex - list all modules in repos matching regex
repos   - list repositories currently in use
search  - search for modules in repos, search pattern is made of two parts <repo>:<mod> 
          <repo> options:
           * name - search in repository 'name' (literal match)
           * regex - search in repos matching regex
          <mod> options:
           * module - search for module name (literal name search)
           * regex  - search for modules matching regex
upload  - uploads all modules to remote public repository repo-name (it should be listed in modrepo.conf) 
          REPO file is generated every time a repo is uploaded
web     - makes local repository public available by serving it using http
          The url wil be localhost:8888/repo-name and will listen to all address by default


## Tools

There are certain tools included in repomod that can be used standalone:

 * httpup-repgen, generates a REPO file for a directory
