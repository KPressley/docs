

===================
OtherSetting
===================

.. list-table::
   :header-rows: 1
   :widths: 25 65 10

   *  -  Field
      -  Description
      -  Note
   *  -  **sortColumnNameValue** |br|
         boolean
      -  Sort Column Name - whether drop-downs with column names are sorted alphabetically.
      -
   *  -  **sortColumnNameDefaultValue** |br|
         boolean
      -  The default value for Sort Column Name (``false``)
      -
   *  -  **trimTimeInJoinsValue** |br|
         boolean
      -  Trim Time in Joins - whether joins using DateTime database fields will use the time portion of the field value or not when constructing the join.
      -
   *  -  **trimTimeInJoinsDefaultValue** |br|
         boolean
      -  The default value for Trim Time in Joins (``true``)
      -
   *  -  **timezoneForDataOffsetValue** |br|
         real number
      -  Timezone for Data Offset - in Hours.
      -
   *  -  **timezoneForDataOffsetDefaultValue** |br|
         real number
      -  The default value for Timezone for Data Offset (``0``)
      -
   *  -  **timezoneForTimestampOffsetValue** |br|
         real number
      -  Timezone for Timestamp Offset - in Hours.
      -
   *  -  **timezoneForDataOffsetDefaultValue** |br|
         real number
      -  The default value for Timezone for Timestamp Offset (``0``)
      -
   *  -  **convertNullToEmptyStringValue** |br|
         boolean
      -  Convert Null to Empty String - whether System shows blank for NULL values.
      -
   *  -  **convertNullToEmptyStringDefaultValue** |br|
         boolean
      -  The default value for Convert Null to Empty String (``false``)
      -
   *  -  **showSchemaNameValue** |br|
         boolean
      -  Show Schema Name - whether System shows schema name together with data source name.
      -
   *  -  **showSchemaNameDefaultValue** |br|
         boolean
      -  The default value for Show Schema Name (``false``)
      -
   *  -  **showIntroductionTextValue** |br|
         boolean
      -  Show Introduction Text - whether System shows introduction text in Report Designer Data Sources tab.
      -
   *  -  **showIntroductionTextDefaultValue** |br|
         boolean
      -  The default value for Show Introduction Text (``false``)
      -
   *  -  **introductionTextValue** |br|
         string
      -  Introduction Text - to be showed in Report Designer Data Sources tab.
      -
   *  -  **introductionTextDefaultValue** |br|
         string
      -  The default value for Introduction Text (``""``)
      -
   *  -  **sendToDiskPathValue** |br|
         string
      -  Send to Disk Path - the default location where scheduled instances will save files to.
      -
   *  -  **sendToDiskPathDefaultValue** |br|
         string
      -  The default value for Send to Disk Path (``""``)
      -
   *  -  **tenantId** |br|
         string (GUID)
      -  The id of the tenant |br|
         null if the setting belongs to system
      -
   *  -  **modified** |br|
         datetime
      -  The last modified datetime
      -

.. container:: toggle

   .. container:: header

      **Sample**:

   .. code-block:: json

      {
         "sortColumnNameValue": false,
         "sortColumnNameDefaultValue": false,
         "trimTimeInJoinsValue": true,
         "trimTimeInJoinsDefaultValue": true,
         "timezoneForDataOffsetValue": 0,
         "timezoneForDataOffsetDefaultValue": 0,
         "timezoneForTimestampOffsetValue": 0,
         "timezoneForTimestampOffsetDefaultValue": 0,
         "convertNullToEmptyStringValue": false,
         "convertNullToEmptyStringDefaultValue": false,
         "showSchemaNameValue": false,
         "showSchemaNameDefaultValue": false,
         "showIntroductionTextValue": false,
         "showIntroductionTextDefaultValue": false,
         "introductionTextValue": "",
         "introductionTextDefaultValue": "",
         "sendToDiskPathValue": "",
         "sendToDiskPathDefaultValue": "",
         "tenantId": null,
         "modified": "2017-02-15T07:29:25.3300651"
      }
