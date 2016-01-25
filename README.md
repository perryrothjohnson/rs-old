# resourcespacedev
ResourceSpace 7.5.7458 on OpenShift
[http://resourcespacedev-cscdb.rhcloud.com](http://resourcespacedev-cscdb.rhcloud.com)

## commit procedure
```bash
# ssh into openshift remote repo
rhc ssh resourcespacedev
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

### configure user permissions
in the _Archivist_ user group, add the 'e0' permission so these users can view files immediately after upload (without waiting for admins to approve the file)  
Team Center > System > Manage User Groups > Archivists > Edit > Launch permissions manager > Resource creation/management > e0  Edit access to workflow state 'Active'  
click the checkbox next to _e0_, scroll to the bottom of the page, and click 
_Save_

### configure email with SMTP using SendGrid
* add SendGrid from the [OpenShift Marketplace](https://marketplace.openshift.com/apps/9628?restoreSearch=true#!overview)  
* click _Manage Product_, then click the _Settings_ tab  
* set a new password, and note the username assigned to you  
* configure ResourceSpace to send emails using SendGrid; ref: [SMTP Relay](https://sendgrid.com/docs/Integrate/index.html#-SMTP-Relay)  
```bash
# ssh into openshift remote repo
rhc ssh resourcespace
# edit the config file
vim ${OPENSHIFT_REPO_DIR}include/config.php
```
* add the following lines to the end of _config.php_  
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

### configure cron jobs
* create a script called _run_resourcespace_cronjob_ in the directory _.openshift/cron/daily_  
* paste the following code into the script  
```bash
#!/bin/bash
# run scheduled tasks included with ResourceSpace by default
php ${OPENSHIFT_REPO_DIR}batch/cron.php
# include 'php' at the beginning of the line above
# (ref: http://stackoverflow.com/questions/10097609/how-to-run-php-file-using-cron-jobs)
date >> ${OPENSHIFT_PHP_LOG_DIR}run_resourcespace_cronjob.log
# write the date string to the file 'run_resourcespace_cronjob.log'
```
* make the script executable  
`chmod +x run_resourcespace_cronjob`
* add the new script file to the local git repo, then push it up  
* wait a while, then check if the cron job is running  
`rhc tail resourcespacedev`  
refs: [Getting started with CRON jobs](https://blog.openshift.com/getting-started-with-cron-jobs-on-openshift/), [Managing background jobs](https://developers.openshift.com/en/managing-background-jobs.html)

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
