# resourcespace
ResourceSpace 7.4.7249 on OpenShift
[http://resourcespace-cscdb.rhcloud.com](http://resourcespace-cscdb.rhcloud.com)

## commit procedure
```bash
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
git pull origin master
# check for unincorporated changes on openshift remote repo
git pull openshift master
# commit the staged files
git commit -m "[message]"
# push the new commit to remote repos on bitbucket and openshift
git push origin master
git push openshift master
```

## ResourceSpace
* [ResourceSpace](http://www.resourcespace.org)
* [Download ResourceSpace](http://resourcespace.org/get)
* [Install ResourceSpace](http://wiki.resourcespace.org/index.php/Installation)
* [Knowledge Base](http://resourcespace.org/knowledge-base/)
* [Google Group](https://groups.google.com/forum/#!forum/resourcespace)

### ResourceSpace permissions
in the _Archivist_ user group, add the 'e0' permission so these users can view files immediately after upload (without waiting for admins to approve the file)  
1. login as admin
2. Team Center > System > Manage User Groups > Archivists > Edit > Launch permissions manager > Resource creation/management > e0  Edit access to workflow state 'Active'  
3. click the checkbox next to 'e0', scroll to the bottom of the page, and click 'Save'

## OpenShift
* Create a free [Red Hat Openshift account](https://openshift.redhat.com)
* [Getting started with OpenShift](https://openshift.redhat.com/app/getting_started)

### OpenShift file system
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
