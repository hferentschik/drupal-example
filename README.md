Drupal on OpenShift Express
===========================

This git repository helps you get up and running quickly w/ a Drupal installation
on OpenShift Express.  The backend database is MySQL and the database name is the
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
--------------------

Create an account at http://openshift.redhat.com/

Create a php-5.3 application

    rhc-create-app -a drupal -t php-5.3

Add MySQL support to your application

    rhc-ctl-app -a drupal -e add-mysql-5.1

Add this upstream drupal repo

    cd drupal
    git remote add upstream -m master git://github.com/openshift/drupal-example.git
    git pull -s recursive -X theirs upstream master

    # note: if you named your application something other than 'drupal' make sure to edit
    #       php/sites/default/settings.php and modify the database to match the name
    #       of your application.

Then push the repo upstream

    git push

That's it, you can now checkout your application at:
    http://drupal-$yournamespace.rhcloud.com

Default Admin Username: Admin
Default Password: OpenShiftAdmin


Updates
-------

In order to update or upgrade to the latest drupal, you'll need to re-pull
and re-push.

Pull from upstream:
    cd drupal/
    git pull -s recursive -X theirs upstream master
Push the new changes upstream
    git push


Repo layout
-----------

php/ - Externally exposed php code goes here
libs/ - Additional libraries
misc/ - For not-externally exposed php code
../data - For persistent data
deplist.txt - list of pears to install
.openshift/action_hooks/build - Script that gets run every push, just prior to
    starting your app


Notes about layout
------------------

Please leave php, libs and data directories but feel free to create additional
directories if needed.

Note: Every time you push, everything in your remote repo dir gets recreated
please store long term items (like an sqlite database) in ../data which will
persist between pushes of your repo.


deplist.txt
-----------

A list of pears to install, line by line on the server.  This will happen when
the user git pushes.


Additional information
----------------------

Link to additional information will be here, when we have it :)
