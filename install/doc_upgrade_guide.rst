

==========================
Upgrade Guide
==========================

Izenda provides monthly maintenance releases with bug fixes and performance increases. At least 2 point releases should be expected each year but our goal is to release quarterly (4) point releases annually.

An automatic upgrade process is still in progress. In the mean time, an upgrade includes the following manual steps.

Upgrade Preparations
--------------------

-  Izenda installation packages

   *  .. _Database_upgrade_scripts:

      .. figure:: /_static/images/Database_upgrade_scripts.png
         :align: right
         :width: 455px

         Version-specific folders containing upgrade scripts

      Izenda System Database upgrade scripts.

         The scripts are organized into separate folders, each for upgrading to a specific database version. |br|

   *  .. _Upgrade_Izenda_App_folder:

      .. figure:: /_static/images/Izenda_App_folder.png
         :align: right
         :width: 482px

         Front-end package

      Izenda Front-end package in an "App" folder. |br|

   *  .. _Upgrade_Izenda_API_folder:

      .. figure:: /_static/images/Izenda_API_folder.png
         :align: right
         :width: 482px

         Back-end package

      Izenda Back-end package in an "API" folder. |br|

-  Permissions and Tools

   *  Database client GUI to connect to Izenda System Database.

      e.g. Management Studio for MS SQL Server.

   *  Izenda System Database administrator account.

      e.g. a user with db\_owner and db\_backupoperator roles in SQL Server.

   *  Izenda Web Server administrator account.

      This account needs remote desktop and IIS service permissions.

   *  Maintenance notice during the upgrade process.

System Backup
-------------

Izenda System Database Backup
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

    This is a step-by-step guide for SQL Server using Management Studio
    GUI.

#. .. _IzendaTest_Backup:

   .. figure:: /_static/images/IzendaTest_Backup.png
      :align: right
      :width: 359px

      Backup Menu

   Log in to the server with Izenda System
   Database using an administrator account.
#. Right-click the Izenda database then Tasks > Back Up... |br|
#. .. _IzendaTest_Backup_Pop-up:

   .. figure:: /_static/images/IzendaTest_Backup_Pop-up.png
      :align: right
      :width: 518px

      Backup

   In the pop-up, choose Backup type Full and Backup
   component Database.

      (Full backup option since Izenda System Database stays at a relatively small size)

#. Give any name for the backup set.
#. Choose to back up to Disk and click Add. |br|
#. .. _IzendaTest_Backup_Filename:

   .. figure:: /_static/images/IzendaTest_Backup_Filename.png
      :align: right
      :width: 299px

      Backup Filename

   Select File name and click ... button to select a path and file name. |br|
#. .. _IzendaTest_Backup_Success:

   .. figure:: /_static/images/IzendaTest_Backup_Success.png
      :align: right
      :width: 455px

      Backup Success

   Click OK 3 times to run the backup. |br|

Izenda Web Backup
~~~~~~~~~~~~~~~~~

 

    Remote desktop to the Web Server to perform this step.

#. Open the Izenda deployment folder in IIS.

       A typical location is at C:\\inetpub\\wwwroot\\Izenda...

#. Back up Izenda Front-end and Back-end files.

       Copy the current API and App folders to a safe location.

#. Back up configuration files to avoid being overwritten.

       Copy the following configuration files to a temporary location.

   -  ``API\izendadb.config``
   -  ``API\Web.config`` if there are custom configurations
   -  ``App\izenda_config.js``

Izenda System Database Upgrade
------------------------------

    This is a step-by-step guide for SQL Server using Management Studio
    GUI.

Identify the Current Izenda System Database Version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

#. Right-click the Izenda database then New Query.
#. Run the query ``select Version from IzendaDBVersion``.
#. The result is the current database version.

Upgrade Izenda System Database Gradually to Latest Version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

#. Open the Izenda System Database upgrade script folder.
#. In SQL Server Management Studio, open the script in the folder that
   upgrades the current version to the next.

       e.g. if current version is 0.22.\ **4**, then open the script
       "IzendaDBSchema.sql" in "0.22.\ **4**-0.22.5" folder.

#. Check that the target database is correct.
#. Run the script to upgrade database to next version.
#. Continue to run the scripts for each next version.

       e.g. if current version is "0.22.\ **5**" and latest version is
       "0.22.\ **8**", then run the scripts in folders
       "0.22.\ **5**-0.22.6", "0.22.6-0.22.7" and "0.22.7-0.22.\ **8**" in
       that order.

#. After all the scripts, re-run the query
   ``select Version from IzendaDBVersion`` to verify the version.

Izenda Web Upgrade
------------------

    Remote desktop to the Web Server to perform these steps.

Replace Current Packages
~~~~~~~~~~~~~~~~~~~~~~~~

  The configuration files must have been backed up before in `Izenda Web
Backup`_ since they will be overwritten in this
step.

#. Download the Izenda Front-end and Back-end Packages to Web Server.
#. Stop the web site process to avoid Izenda DLL files being used.
#. Remove all files in current API and App folders.
#. Copy the files in downloaded API and App folders to the current API
   and App folders respectively.

Restore the Current Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

#. Back-end: copy the configuration files from temporary location in
   `Izenda Web Backup`_ step to overwrite default
   ones in ``API`` folder.

   -  ``izendadb.config``
   -  ``Web.config`` if it has been backed up.

#. Front-end: in ``App\izenda_config.js``, update the value of
   ``WebApiUrl`` to the current address of back-end APIs (e.g.
   ``http://localhost:8888/api/``).

Restart the Web Server
~~~~~~~~~~~~~~~~~~~~~~

 

Restore Steps in case of Error
------------------------------

 

#. Restore the database using the back up file in `Izenda System
   Database Backup`_ step.
#. Empty the API and App folders then copy back the contents from the
   location in `Izenda Web Backup`_ step.
