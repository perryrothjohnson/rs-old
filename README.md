# resourcespace
ResourceSpace 7.4.7249 on OpenShift

## commit procedure
<pre><code># check for file changes
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
</code></pre>

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
<pre><code>.
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
</code></pre>
