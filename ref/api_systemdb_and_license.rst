

============================
System DB and License APIs
============================


List of APIs
------------

.. list-table::
   :class: apitable
   :widths: 35 65
   :header-rows: 1

   * - API
     - Purpose
   * - GET databaseSetup/supportedDatabaseType
     - Returns an array of database server types currently supported for installation. |br|
       See also: :ref:`GET_dataAdaptor`
   * - POST databaseSetup/testConnection
     - Tests the database connection and license validity if existing. |br|
       See also: :ref:`POST_connection/verify`
   * - GET databaseSetup/databaseInfo
     - Returns the current database server type and connection string.
   * - POST databaseSetup/databaseInfo
     - Sets up system database and saves the database server type and the connection string.
   * - POST databaseSetup/provisionStaticData
     - Provisions static data into system database.
   * - GET databaseSetup/provisionStaticDataStatus
     - Returns status of provision static data.
   * - POST databaseSetup/staticData/city
     - Returns provision city data.
   * - POST databaseSetup/staticData/postalCode
     - Returns provision postal code data.
   * - GET systemSetting/systemMode
     - Returns the current system or tenant mode of the settings.
   * - GET systemSetting/systemModeSettings
     - Returns data in System Mode Settings section.
   * - POST systemSetting/systemModeSettings
     - Saves data in System Mode Settings section.
   * - GET systemSetting/deploymentMode
     - Returns the deployment mode.
   * - GET systemSetting/systemIndicator/(tenant_id)
     - Returns the number of changes in Connection String and Data Model (filtered by tenant_id if provided).
   * - GET license/status
     - Returns up-to-date expiration status of the license.
   * - GET license/currentToken
     - Returns the details of the current token.
   * - POST license/validate
     - Validates the license for both online and offline modes, then save it in the database.
   * - POST license/requestToken
     - Returns a newly-generated token for the license key in parameter.

.. _GET_databaseSetup/supportedDatabaseType:

GET databaseSetup/supportedDatabaseType
--------------------------------------------------------------

Returns an array of database server types currently supported for installation.

**Request**

    No payload

**Response**

    An array of :doc:`models/DatabaseType` objects

**Samples**

   .. code-block:: http

      GET /api/databaseSetup/supportedDatabaseType HTTP/1.1

   Sample response::

      [
        {name: "[AZSQL] AzureSQL", shortName: "AZSQL", id: "d968e96f-91dc-414d-9fd8-aef2926c9a18"},
        {name: "[MYSQL] MySQL", shortName: "MYSQL", id: "3d4916d1-5a41-4b94-874f-5bedacb89656"},
        {name: "[ORACL] Oracle", shortName: "ORACL", id: "93942448-c715-4f98-85e2-9292ed7ca4bc"},
        {name: "[PGSQL] PostgreSQL", shortName: "PGSQL", id: "f2638ed5-70e5-47da-a052-4da0c1888fcf"},
        {name: "[MSSQL] SQLServer", shortName: "MSSQL", id: "572bd576-8c92-4901-ab2a-b16e38144813"}
      ]

.. _POST_databaseSetup/testConnection:

POST databaseSetup/testConnection
--------------------------------------------------------------

Tests the database connection and license validity if existing.

If success, the connection string will also be saved.

**Request**

    Payload: a :doc:`models/DBSetupInfo` object

**Response**

    A :doc:`models/ConnectionVerificationResult` object

**Samples**

   .. code-block:: http

      POST /api/databaseSetup/testConnection HTTP/1.1

   Request payload::

      {
         "ServerTypeId": " d968e96f-91dc-414d-9fd8-aef2926c9a18",
         "ConnectionString": " server=host01\\instance01;database=db01;User Id=user01;Password=secret;"
      }

   Response in case of a successful call::

      {
        "serverNotValid" : false,
        "databaseNotValid" : false,
        "loginFail" : false,
        "hasValidLicense" : false,
        "success" : true,
        "messages" : []
      }

   Response in case of an invalid connection string error::

      {
        "serverNotValid" : false,
        "databaseNotValid" : false,
        "loginFail" : false,
        "hasValidLicense" : false,
        "success" : false,
        "messages" : ["The connection string is invalid. Please enter a valid one."]
      }

GET databaseSetup/databaseInfo
--------------------------------------------------------------

Returns the current database server type and connection string.

**Request**

    No payload

**Response**

    A :doc:`models/DBSetupInfo` object

**Samples**

   .. code-block:: http

      GET /api/databaseSetup/databaseInfo HTTP/1.1

   Sample response::

      {
        "serverTypeId":"f2638ed5-70e5-47da-a052-4da0c1888fcf",
        "serverTypeName":"[PGSQL] PostgreSQL",
        "connectionString":"Server=izenda-w10-02;Integrated Security=true; Database=db01;"
      }


POST databaseSetup/databaseInfo
--------------------------------------------------------------

Sets up system database and saves the database server type and the connection string.

.. note::

   It will take some time to set up the system database

**Request**

    Payload: a :doc:`models/DBSetupInfo` object

**Response**

    An :doc:`models/OperationResult` object with **success** field true if the setup is successful

**Samples**

   To be updated

POST databaseSetup/provisionStaticData
--------------------------------------------------------------

Provisions static data into system database.

**Request**

    No payload

**Response**

    * 0 = Not started
    * 1 = Provisioning in progresss
    * 2 = Provision success
    * 3 = Provision error

**Samples**

   .. code-block:: http

      POST /api/databaseSetup/provisionStaticData HTTP/1.1

   Sample response::

      1


GET databaseSetup/provisionStaticDataStatus
--------------------------------------------------------------

Returns status of provision static data.

**Request**

    No payload

**Response**

    * 0 = Not started
    * 1 = Provisioning in progresss
    * 2 = Provision success
    * 3 = Provision error

**Samples**

   .. code-block:: http

      GET /api/databaseSetup/provisionStaticDataStatus HTTP/1.1

   Sample response::

      2


POST databaseSetup/staticData/city
--------------------------------------------------------------

Returns provision city data.

**Request**

    Payload:

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **criterias** |br|
             array of strings
          -  The fields to filter data

             .. hlist::
                :columns: 2

                *  GeonameId
                *  Name
                *  AsciiName
                *  AlternateNames
                *  Latitude
                *  Longitude
                *  FeatureClass
                *  FeatureCode
                *  CountryCode
                *  Cc2
                *  Admin1Code
                *  Admin2Code
                *  Admin3Code
                *  Admin4Code
                *  Population
                *  Elevation
                *  Dem
                *  Timezone
          -
       *  -  **values** |br|
             array of strings
          -  The values to filter data (using case-insensitive string equal operator), in exact same order with the fields
          -


**Response**

    An array of :doc:`models/City` objects

**Samples**

   .. code-block:: http

      POST /api/databaseSetup/staticData/city HTTP/1.1

   Request payload::

      {"criterias":[],"values":[]}

   Sample response::

      To be updated


POST databaseSetup/staticData/postalCode
--------------------------------------------------------------

Returns provision postal code data.

**Request**

    Payload:

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **criterias** |br|
             array of strings
          -  The fields to filter data

             .. hlist::
                :columns: 2

                *  PostalCode
                *  PlaceName
                *  Province
                *  Latitude
                *  Longitude
          -
       *  -  **values** |br|
             array of strings
          -  The values to filter data (using case-insensitive string equal operator), in exact same order with the fields
          -


**Response**

    An array of :doc:`models/PostCode` objects

**Samples**

   .. code-block:: http

      GET /api/databaseSetup/staticData/postalCode HTTP/1.1

   Request payload::

      {"criterias":[],"values":[]}

   Sample response::

      To be updated


GET systemSetting/systemMode
--------------------------------------------------------------

Returns the current system or tenant mode of the settings.

**Request**

    No payload

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **systemMode** |br|
             integer
          -  The system mode

             * 0 = Multiple tenant
             * 1 = Single tenant
          -

**Samples**

   .. code-block:: http

      GET /api/systemSetting/systemMode HTTP/1.1

   Sample response::

      { "systemMode" : 1 }


GET systemSetting/systemModeSettings
--------------------------------------------------------------

Returns data in System Mode Settings section.

**Request**

    No payload

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **systemMode** |br|
             integer
          -  The system mode

             * 0 = Multiple tenant
             * 1 = Single tenant
          -
       *  -  **allowDuplicateUser** |br|
             boolean
          -  Whether to allow duplicated user names in multi-tenant mode
          -

**Samples**

   .. code-block:: http

      GET /api/systemSetting/systemModeSettings HTTP/1.1

   Sample response::

      {
       "systemMode": 0,
       "allowDuplicateUser": true
      }


POST systemSetting/systemModeSettings
--------------------------------------------------------------

Saves data in System Mode Settings section.

**Request**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **systemMode** |br|
             integer
          -  The system mode

             * 0 = Multiple tenant
             * 1 = Single tenant
          -
       *  -  **allowDuplicateUser** |br|
             boolean
          -  Whether to allow duplicated user names in multi-tenant mode
          -

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **success** |br|
             boolean
          -  Is the save successful
          -

**Samples**

   .. code-block:: http

      POST /api/systemSetting/systemModeSettings HTTP/1.1

   Request payload::

      {
        "systemMode": 0,
        "allowDuplicateUser": true
      }

   Sample response::

      {
        "success": true
      }


GET systemSetting/deploymentMode
--------------------------------------------------------------

Returns the deployment mode.

**Request**

    No payload

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **deploymentMode** |br|
             integer
          -  Integration modes

             * 0 = AllStandAlone
             * 1 = BEStandAloneFEIntegrated
             * 2 = BEIntegratedFEStandAlone
             * 3 = AllIntegrated
          -

**Samples**

   .. code-block:: http

      GET /api/systemSetting/deploymentMode HTTP/1.1

   Sample response::

      {
        "deploymentMode": 0
      }


GET systemSetting/systemIndicator/(tenant_id)
--------------------------------------------------------------

Returns the number of changes in Connection String and Data Model (filtered by tenant_id if provided).

**Request**

    No payload

**Response**

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **key** |br|
             string
          -  Either "ConnectionString" or "DataModel"
          -
       *  -  **value** |br|
             integer
          -  The number of changes for each type
          -

**Samples**

   .. code-block:: http

      GET /api/systemSetting/systemIndicator HTTP/1.1

   Sample response::

      [{
        "key" : "ConnectionString",
        "value" : 1
      }, {
        "key" : "DataModel",
        "value" : 2
      }]


GET license/status
--------------------------------------------------------------

Returns up-to-date expiration status of the license.

**Request**

    No payload

**Response**

    A :doc:`models/LicenseStatusResult` object

**Samples**

   .. code-block:: http

      GET /api/license/status HTTP/1.1

   Sample response::

      {
         "licenseStatus": {
            "disabled": false,
            "meetExprireWarningPeriod": false,
            "numberOfDayToExpire": 88,
            "numberOfDayToValid": 0,
            "exceedLostConnectionAllowPeriod": false,
            "isAdminUser": false,
            "trialLicense": false
         },
         "success": true,
         "messages": null
      }


GET license/currentToken
--------------------------------------------------------------

Returns the details of the current token.

**Request**

    No payload

**Response**

    A :doc:`models/ValidateTokenResult` object

**Samples**

   .. code-block:: http

      GET /api/license/currentToken HTTP/1.1

   Sample response::

      {
         "tokenKey":"1aBcD+=",
         "licenseKey":"1aBcD+=",
         "startDate":"2016-03-01T00:00:00",
         "endDate":"2017-03-01T23:59:59",
         "modules":[
            {
                 "id":"256b555f-58ef-4418-be6c-048d2fc1f691",
                 "name":"Alerting"
            }
         ],
         "companyId":"70d1037a-401a-446b-ae10-a5bb0144c611",
         "previousStartDate":null,
         "previousEndDate":null,
         "previousModules":null,
         "licenseOnlineMode":false,
         "licenseTrial":false,
         "licenseEnable":true,
         "licenseEndDate":"2017-03-01T23:59:59",
         "numberOfDayToValid":0,
         "success":true,
         "messages":null
      }


POST license/validate
--------------------------------------------------------------

Validates the license for both online and offline modes, then save it in the database.

**Request**

    Payload: a :doc:`models/TokenRequest` object

**Response**

    A :doc:`models/ValidateTokenResult` object

**Samples**

   .. code-block:: http

      POST /api/license/validate HTTP/1.1

   For Online mode, Request Payload includes LicenseKey only::

      {"LicenseKey":"1aBcD+=="}

   For Offline mode, Request Payload includes both LicenseKey and TokenKey::

      {"LicenseKey":"1aBcD+==","TokenKey":"1aBcD+="}


POST license/requestToken
--------------------------------------------------------------

Returns a newly-generated token for the license key in parameter.

**Request**

    Payload: a :doc:`models/TokenRequest` object

**Response**

    A :doc:`models/ValidateTokenResult` object

**Samples**

   .. code-block:: http

      POST /api/license/requestToken HTTP/1.1

   Request payload::

      {"LicenseKey":"1aBcD+=="}

   Sample response::

      {
         "success":true,
         "messages":null,
         "tokenKey":"1aBcD+==",
         "licenseKey":"1aBcD+=",
         "startDate":"2016-03-01T00:00:00",
         "endDate":"2017-03-01T23:59:59",
         "modules":[
             {
                 "id":"256b555f-58ef-4418-be6c-048d2fc1f691",
                 "name":"Alerting"
             }
         ],
         "companyId":"70d1037a-401a-446b-ae10-a5bb0144c611",
         "previousStartDate":null,
         "previousEndDate":null,
         "previousModules":null,
         "licenseOnlineMode":false,
         "licenseTrial":false,
         "licenseEnable":true,
         "licenseEndDate":"2017-03-01T23:59:59",
         "numberOfDayToValid":0
     }
