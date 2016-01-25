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

### configure user permissions
in the _Archivist_ user group, add the _e0_ permission so these users can view files immediately after upload (without waiting for admins to approve the file)  
1. login as admin  
2. Team Center > System > Manage User Groups > Archivists > Edit > Launch permissions manager > Resource creation/management > e0  Edit access to workflow state 'Active'  
3. click the checkbox next to _e0_, scroll to the bottom of the page, and click _Save_

### configure email with SMTP using SendGrid
1. add SendGrid from the [OpenShift Marketplace](https://marketplace.openshift.com/apps/9628?restoreSearch=true#!overview)  
2. click _Manage Product_, then click the _Settings_ tab  
3. set a new password, and note the username assigned to you  
4. configure ResourceSpace to send emails using SendGrid; ref: [SMTP Relay](https://sendgrid.com/docs/Integrate/index.html#-SMTP-Relay)  
```bash
# ssh into openshift remote repo
rhc ssh resourcespace
# edit the config file
vim ${OPENSHIFT_REPO_DIR}include/config.php
```
5. add the following lines to the end of _config.php_  
```php
# Use an external SMTP server for outgoing emails (e.g. Gmail).
# http://wiki.resourcespace.org/index.php/Email_using_an_external_SMTP_server
$use_smtp=true;
# SMTP settings:
$smtp_secure='ssl';
$smtp_host='smtp.sendgrid.net';
$smtp_port=465;
$smtp_auth=true;
$smtp_username='[username]';
$smtp_password='[password]';
# enable php mailer - this allows HTML format emails
$use_phpmailer=true;
```

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
