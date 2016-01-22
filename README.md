# resourcespacedev
ResourceSpace 7.5.7458 on OpenShift

## commit procedure
```
# ssh into openshift remote repo
rhc ssh resourcespace
# backup config.php and filestore
cp ${OPENSHIFT_REPO_DIR}include/config.php ${OPENSHIFT_DATA_DIR}
cp -R ${OPENSHIFT_REPO_DIR}filestore ${OPENSHIFT_DATA_DIR}
# exit from openshift ssh
exit
# check for local file changes
git status
# stage files for commit
git add ...
# check for unincorporated changes on bitbucket remote repo
git pull origin dev
# check for unincorporated changes on openshift remote repo
git pull openshiftdev dev
# commit the staged files
git commit -m "[message]"
# push the new commit to remote repos on bitbucket and openshift
git push origin dev
git push openshiftdev dev
```

## ResourceSpace
* [ResourceSpace](http://www.resourcespace.org)
* [Download ResourceSpace](http://resourcespace.org/get)
* [Install ResourceSpace](http://wiki.resourcespace.org/index.php/Installation)
* [Knowledge Base](http://resourcespace.org/knowledge-base/)
* [Google Group](https://groups.google.com/forum/#!forum/resourcespace)

## OpenShift
* Create a free [Red Hat Openshift account](https://openshift.redhat.com)
* [Getting started with OpenShift](https://openshift.redhat.com/app/getting_started)

## OpenShift file system
```
.
|-- .env
|-- app-root
|   |-- data  ($OPENSHIFT_DATA_DIR)
|   |-- repo -> runtime/repo
|   `-- runtime
|       |-- data
|       `-- repo  ($OPENSHIFT_REPO_DIR)
|           |-- ...deployed application code
|           `-- include
|               |-- config.php
|--  app-deployments
|   |-- current
|   |   |-- build-dependencies
|   |   |-- dependencies
|   |   |-- metadata.json
|   |   `-- repo
|   `-- ...application deployments
|--  git
|   `-- [APP_NAME].git
|       `-- hooks
|       |   |--  post-receive
|       |   |--  pre-receive
|       |   `-- ... sample hooks
|       `-- ... other git directories
`-- ...cartridge directories
```
