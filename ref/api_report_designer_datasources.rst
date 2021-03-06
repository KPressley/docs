

==================================
Report Designer Data Sources APIs
==================================

The **Report Designer** page allows user to

-  create a report
-  select report data sources
-  create report filters
-  design report layout
-  set up report schedule

This page documents the APIs related to data sources, relationships and fields.

List of APIs
------------

.. list-table::
   :class: apitable
   :widths: 35 65
   :header-rows: 1

   * - API
     - Purpose
   * - `GET report/dataSourceCategory/(tenant_id)`_
     - Returns data sources and data source fields grouped by data source categories (filtered by tenant_id if provided). |br| |br|
       See also: :ref:`POST_advancedSetting/category` and :ref:`POST_dataModel` for APIs that create these data source categories.
   * - `POST report/loadDataSourceCategory`_
     - For a report, returns both selected, visible data sources and other available data sources, all grouped by data source categories. |br| |br|
       Compared with `GET report/dataSourceCategory/(tenant_id)`_, this API sets the "selected" and "visible" flags.
   * - `POST report/loadQuerySources`_
     - Returns details for a list of query sources and a list of related query sources.
   * - `POST report/loadRelationships`_
     - Returns an updated list of relationships from an existing report together with an updated list of data sources.
   * - `POST report/validateRelationshipSyntax`_
     - Validates that relationships correctly link selected data sources, have correct alias and data types, and support distinct option if checked.
   * - `POST report/detectRelationshipsChange`_
     - Detects if any relationship should be removed because of changes in data sources.
   * - `POST report/availableQuerySourceFields`_
     - Returns a list of data source fields in selected query sources of a report.
   * - `GET report/availableReportMappingFields/{report_id}`_
     - Returns a list of data source fields, alias fields, and calculated fields in selected query sources of a report.
   * - `GET report/fieldProperties/{query_source_field_id}`_
     - Returns the properties of query source field specified by query_source_field_id.
   * - `POST report/calculatedField`_
     - Saves a calculated field after validating name duplication (does not validate the expression).
   * - `GET report/calculatedField/{calculated_field_id}`_
     - Returns the calculated field specified by calculated_field_id.
   * - `GET report/hasReportUseCalculatedField/{calculated_field_id}`_
     - Returns true if the calculated field is being used in any report part or report filter.
   * - `GET report/hasReportUseRelationship/{relationship_id}`_
     - Returns true if the relationship is being used in report.
   * - `POST report/deleteReportCalculatedField`_
     - Removes a calculated field from report.

GET report/dataSourceCategory/(tenant_id)
------------------------------------------------

Returns data sources and data source fields grouped by data source categories (filtered by tenant_id if provided).

See also: :ref:`POST_advancedSetting/category` and :ref:`POST_dataModel` for APIs that create these data source categories.

**Request**

    No payload

**Response**

    An array of :doc:`models/ReportDataSourceCategory` objects

**Samples**

   .. code-block:: http

      POST /api/report/dataSourceCategory HTTP/1.1

   Sample response::

      [{
         "id": "f28d7175-4cef-478e-b914-ae075c3c33b8",
         "name": "Data Source Category 1",
         "querySource": [{
            "id": "ffd40590-aa27-4a14-8ebf-f32a0567bc08",
            "name": "Department",
            "type": "Table",
            "selected": false,
            "visible": true,
            "querySourceCategoryName": "HumanResources",
            "connectionName": "AdventureWorks2008R2",
            "isAlias": false,
            "fields": [{
                 "id": "5da4090d-9b31-433c-b9bb-e9e82fcc92a8",
                 "name": "DepartmentID",
                 "alias": null,
                 "dataType": "smallint",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "{\"PrimaryKey\":true}",
                 "isParameter": false,
                 "allowDistinct": false
            }, {
                 "id": "2636eeb4-cb65-48f4-9da6-2bfe5cd0659a",
                 "name": "Name",
                 "alias": null,
                 "dataType": "nvarchar",
                 "unitDataType": "Text",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }]
         }]
      }, {
         "id": "00000000-0000-0000-0000-000000000000",
         "name": null,
         "querySource": [{
            "id": "06cc2448-5a09-44db-99b5-5fb7c8863be6",
            "name": "vEmployee",
            "type": "View",
            "selected": false,
            "visible": true,
            "querySourceCategoryName": "HumanResources",
            "connectionName": "AdventureWorks2008R2",
            "isAlias": false,
            "fields": [{
                 "id": "c8840bd0-572f-4243-a840-2d1d20402a43",
                 "name": "BusinessEntityID",
                 "alias": null,
                 "dataType": "int",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }, {
                 "id": "0284b8a5-f97e-4496-9f2e-dd2a6766153a",
                 "name": "EmailAddress",
                 "alias": null,
                 "dataType": "nvarchar",
                 "unitDataType": "Text",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }]
         }]
      }]

POST report/loadDataSourceCategory
------------------------------------------------

For a report, returns both selected, visible data sources and other available data sources, all grouped by data source categories.

Compared with `GET report/dataSourceCategory/(tenant_id)`_, this API sets the "selected" and "visible" flags.

**Request**

    Payload: a :doc:`models/ReportDataSourceParameter` object

**Response**

    An array of :doc:`models/ReportDataSourceCategory` objects

**Samples**

   .. code-block:: http

      POST /api/report/loadDataSourceCategory HTTP/1.1

   Request payload::

      {
        "tenantId" : null,
        "reportKey" : {
           "key" : "f53b65ba-4d27-45c9-930e-156538f30531",
           "tenantId" : null
        }
      }

   Response::

      [{
         "id": "f28d7175-4cef-478e-b914-ae075c3c33b8",
         "name": "Data Source Category 1",
         "querySource": [{
            "id": "ffd40590-aa27-4a14-8ebf-f32a0567bc08",
            "name": "Department",
            "type": "Table",
            "selected": true,
            "visible": false,
            "querySourceCategoryName": "HumanResources",
            "connectionName": "AdventureWorks2008R2",
            "isAlias": false,
            "fields": [{
                 "id": "5da4090d-9b31-433c-b9bb-e9e82fcc92a8",
                 "name": "DepartmentID",
                 "alias": null,
                 "dataType": "smallint",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "{\"PrimaryKey\":true}",
                 "isParameter": false,
                 "allowDistinct": false
            }, {
                 "id": "2636eeb4-cb65-48f4-9da6-2bfe5cd0659a",
                 "name": "Name",
                 "alias": null,
                 "dataType": "nvarchar",
                 "unitDataType": "Text",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }]
         }]
      }, {
         "id": "00000000-0000-0000-0000-000000000000",
         "name": null,
         "querySource": [{
            "id": "06cc2448-5a09-44db-99b5-5fb7c8863be6",
            "name": "vEmployee",
            "type": "View",
            "selected": false,
            "visible": true,
            "querySourceCategoryName": "HumanResources",
            "connectionName": "AdventureWorks2008R2",
            "isAlias": false,
            "fields": [{
                 "id": "c8840bd0-572f-4243-a840-2d1d20402a43",
                 "name": "BusinessEntityID",
                 "alias": null,
                 "dataType": "int",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }, {
                 "id": "0284b8a5-f97e-4496-9f2e-dd2a6766153a",
                 "name": "EmailAddress",
                 "alias": null,
                 "dataType": "nvarchar",
                 "unitDataType": "Text",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": "",
                 "isParameter": false,
                 "allowDistinct": false
            }]
         }]
      }]

POST report/loadQuerySources
------------------------------------------------

Returns details for a list of query sources and a list of related query sources.

**Request**

    Payload: a :doc:`models/ReportSelectedPagedRequest` object

**Response**

    A :doc:`models/ReportSelectedQuerySourceResult` object

**Samples**

   .. code-block:: http

      POST /api/report/loadQuerySources HTTP/1.1

   Request payload::

      {
        "querySources" : [{
            "querySourceId": "39e2a9b9-3be3-4b8b-ae86-0823ecb3c533",
            "selected": true
         }],
        "tenantId" : null,
        "criteria" : null,
        "pageIndex" : 1,
        "pageSize" : 10,
        "sortOrders" : null
      }

   Sample response::

      {
         "relatedQuerySources": [{
            "querySourceId": "39e2a9b9-3be3-4b8b-ae86-0823ecb3c533",
            "isNew": false,
            "physicalChange": 0,
            "selected": false
         }, {
            "querySourceId": "c25dc1d3-8066-4fe2-9adb-179060780088",
            "isNew": false,
            "physicalChange": 0,
            "selected": false
         }, {
            "querySourceId": "2c26efb2-9ff8-43ea-bcc7-6f1063e1f635",
            "isNew": false,
            "physicalChange": 0,
            "selected": false
         }],
         "result": [{
            "id": "39e2a9b9-3be3-4b8b-ae86-0823ecb3c533",
            "category": null,
            "databaseName": "Northwind",
            "schemaName": "dbo",
            "dataObject": "CustomerCustomerDemo",
            "dataObjectType": "Table"
         }],
         "total": 1,
         "pageIndex": 1,
         "pageSize": 10
      }

POST report/loadRelationships
------------------------------------------------

Returns an updated list of relationships from an existing report together with an updated list of data sources.

**Request**

    Payload: a :doc:`models/RelationshipPagedRequest` object

**Response**

    A :doc:`models/ReportRelationshipResult` object

**Samples**

   .. code-block:: http

      POST /api/report/loadRelationships HTTP/1.1

   Request payload (query source id = "65d587e2-71f9-4565-8ad8-e6f532398455" has been selected by user)::

      {
        "objectId" : null,
        "criteria" : [{
              "key" : "All",
              "value" : "",
              "operation" : 1
           }
        ],
        "pageIndex" : 1,
        "pageSize" : 10,
        "querySources" : [{
              "querySourceId" : "65d587e2-71f9-4565-8ad8-e6f532398455",
              "selected" : true,
              "physicalChange" : 2,
              "state" : 1
           }, {
              "querySourceId" : "7d4d81a0-4813-4e77-912d-934333c607e1",
              "selected" : false,
              "physicalChange" : 0,
              "state" : 1
           }
        ]
      }

   Response:

      .. code-block:: json
         :emphasize-lines: 6,7,15,17,19

         {
           "hasRemovedRelationship" : false,
           "result" : [{
                 "joinConnectionId" : "11d2c31c-e726-4f80-8621-2b4856fae1a5",
                 "foreignConnectionId" : "11d2c31c-e726-4f80-8621-2b4856fae1a5",
                 "joinQuerySourceId" : "65d587e2-71f9-4565-8ad8-e6f532398455",
                 "joinQuerySourceName" : "Employees",
                 "joinDataSourceCategoryName" : null,
                 "joinDataSourceCategoryId" : "00000000-0000-0000-0000-000000000000",
                 "foreignDataSourceCategoryName" : null,
                 "foreignDataSourceCategoryId" : "00000000-0000-0000-0000-000000000000",
                 "foreignQuerySourceId" : "65d587e2-71f9-4565-8ad8-e6f532398455",
                 "foreignQuerySourceName" : "Employees",
                 "joinFieldId" : "d198eb03-6dee-4e3d-bc08-4ab11f08d3bd",
                 "joinFieldName" : "ReportsTo",
                 "foreignFieldId" : "f661a585-b463-426c-8849-dc6921139f7c",
                 "foreignFieldName" : "EmployeeID",
                 "alias" : null,
                 "systemRelationship" : true,
                 "joinType" : "Inner",
                 "parentRelationshipId" : "00000000-0000-0000-0000-000000000000",
                 "deleted" : false,
                 "position" : null,
                 "relationshipPosition" : 0,
                 "relationshipKeyJoins" : null,
                 "reportId" : "00000000-0000-0000-0000-000000000000",
                 "foreignAlias" : null,
                 "selectedForeignAlias" : "65d587e2-71f9-4565-8ad8-e6f532398455_Employees",
                 "id" : "65fe4ced-577c-4da5-97a0-5e2903a0a7ab",
                 "state" : 0,
                 "modified" : "2016-04-28T03:33:48.4200000+07:00",
                 "dateTimeNow" : "2016-04-28T04:04:09.0399962Z"
              }
           ],
           "total" : 1,
           "pageIndex" : 1,
           "pageSize" : 10
         }

      The response says that: There is one relationship involving query source id = "65d587e2-71f9-4565-8ad8-e6f532398455" (Employees). That is a system relationship (foreign key) with Employees.ReportsTo self-references Employees.EmployeeID.


POST report/validateRelationshipSyntax
------------------------------------------------

Validates that relationships correctly link selected data sources, have correct alias and data types, and support distinct option if checked.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

    An :doc:`models/OperationResult` object, with **success** field true if syntax is valid

**Samples**

   .. code-block:: http

      POST /api/report/validateRelationshipSyntax HTTP/1.1

   Request payload::

      {
        "reportKey" : {
           "key" : null,
           "modified" : null
        },
        "section" : 0,
        "saveAs" : false,
        "ignoreCheckChange" : false,
        "report" : {
           "name" : "",
           "type" : "Templates",
           "previewRecord" : 10,
           "advancedMode" : true,
           "allowNulls" : false,
           "isDistinct" : false,
           "reportDataSource" : [{
                 "aliasId" : "479be129-338d-45f1-b216-1d95957fe2c8_Order Details",
                 "querySourceId" : "479be129-338d-45f1-b216-1d95957fe2c8",
                 "querySourceName" : "Order Details",
                 "selected" : true
              }, {
                 "aliasId" : "54852be4-5584-4c23-ae5d-4197724059e1_Orders",
                 "querySourceId" : "54852be4-5584-4c23-ae5d-4197724059e1",
                 "querySourceName" : "Orders",
                 "selected" : true
              }
           ],
           "reportRelationship" : [{
                 "tempId" : "16d3b9bf-86cb-45fa-b33d-53e3e2a8a042",
                 "joinConnectionId" : "db19bb46-ffa3-45fd-b205-0dad305fdf98",
                 "foreignConnectionId" : "db19bb46-ffa3-45fd-b205-0dad305fdf98",
                 "joinQuerySourceId" : "479be129-338d-45f1-b216-1d95957fe2c8",
                 "joinQuerySourceName" : "Order Details",
                 "joinDataSourceCategoryName" : "",
                 "foreignDataSourceCategoryName" : "",
                 "foreignQuerySourceId" : "54852be4-5584-4c23-ae5d-4197724059e1",
                 "foreignQuerySourceName" : "Orders",
                 "joinFieldId" : "a0011b48-ef08-45fe-b044-abc68442cd17",
                 "joinFieldName" : "OrderID",
                 "foreignFieldId" : "3caf9c17-abd7-4119-809d-2c3debb8eb37",
                 "foreignFieldName" : "OrderID",
                 "alias" : "",
                 "systemRelationship" : true,
                 "joinType" : "Inner",
                 "parentRelationshipId" : "c55d696b-f25d-4a6f-a951-7a4e6e532c98",
                 "position" : null,
                 "relationshipKeyJoins" : [],
                 "selectedForeignAlias" : "54852be4-5584-4c23-ae5d-4197724059e1_Orders",
                 "id" : "16d3b9bf-86cb-45fa-b33d-53e3e2a8a052",
                 "state" : 1,
                 "validationKey" : "c55d696b-f25d-4a6f-a951-7a4e6e532c98",
                 "relationshipPosition" : 0,
                 "invalidAlias" : null,
                 "hidden" : false,
                 "level" : 1
              }
           ],
           "reportPart" : []
        },
        "expandedLevel" : 0,
        "filters" : []
      }

   Successful response::

      {
         "success": true,
         "messages": [{
            "key": "",
            "messages": ["A valid SQL statement can be constructed from the given relationships."]
         }]
      }

POST report/detectRelationshipsChange
------------------------------------------------

Detects if any relationship should be removed because of changes in data sources.

**Request**

    Payload: a :doc:`models/RelationshipPagedRequest` object

**Response**

    * true if any relationship needs to be removed
    * false if none

**Samples**

   To be updated

POST report/availableQuerySourceFields
------------------------------------------------

Returns a list of data source fields in selected query sources of a report.

**Request**

    Payload: a :doc:`models/ReportSavingParameter` object

**Response**

    An array containing exactly one :doc:`models/ReportDataSourceCategory` object

**Samples**

   .. code-block:: http

      POST /api/report/availableQuerySourceFields HTTP/1.1

   Request payload::

      {
        "reportKey" : {
           "key" : "024b91d3-4896-4191-8d8e-384997746178",
           "tenantId" : null
        }
      }

   Sample response::

      [{
         "id": null,
         "name": "Selected Data Source",
         "querySource": [{
            "id": "58ea6138-2980-46d7-b19a-4b102c359865",
            "name": "Employees",
            "type": "Table",
            "selected": false,
            "visible": true,
            "querySourceCategoryName": "Category_1",
            "connectionName": "Northwind",
            "isAlias": false,
            "fields": [{
                 "id": "343945c3-fbb9-43bb-8d57-f548b5566c35",
                 "name": "EmployeeID",
                 "alias": null,
                 "dataType": "int",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": null,
                 "isParameter": false,
                 "allowDistinct": true
            }]
         },  {
            "id": "5f39b800-47c9-4fca-970b-20e81cb2dbd9",
            "name": "Products",
            "type": "Table",
            "selected": false,
            "visible": true,
            "querySourceCategoryName": "Category_2",
            "connectionName": "Northwind",
            "isAlias": false,
            "fields": [{
                 "id": "bc8c7b39-53c2-49fc-8a4a-20782ad3369d",
                 "name": "ProductID",
                 "alias": null,
                 "dataType": "int",
                 "unitDataType": "Number",
                 "visible": true,
                 "filterable": true,
                 "extendedProperties": null,
                 "isParameter": false,
                 "allowDistinct": true
            }]
         }]
      }]

GET report/availableReportMappingFields/{report_id}
-----------------------------------------------------------

Returns a list of data source fields, alias fields, and calculated fields in selected query sources of a report.

**Request**

    No payload

**Response**

    An array of :doc:`models/ReportField` objects

**Samples**

   .. code-block:: http

      GET /api/report/availableReportMappingFields/45f17b8a-3708-4f36-80ef-9178b7124841 HTTP/1.1

   Response::

      [{
          "fieldId": "1524ea5e-2111-4fd9-b749-f0f9150691a1",
          "originalName": null,
          "fieldName": "CalendarYear",
          "fieldNameAlias": "",
          "dataFieldType": "Numeric",
          "querySourceId": "f56e717c-d45b-4af9-9e98-968c259ee858",
          "querySourceType": "Table",
          "sourceAlias": "DueDate",
          "relationshipId": "78fb49b8-de6f-491b-aab2-fc01a509093e",
          "visible": true,
          "filterable": false,
          "reportId": null,
          "fieldFunctionExpression": "[DueDate].[CalendarYear]",
          "expression": null,
          "grandTotalExpression": null,
          "subTotalExpression": null,
          "sort": "Unsorted",
          "function": null,
          "format": null,
          "functionDataType": null,
          "calculatedTree": null,
          "grandTotalTree": null,
          "isCalculated": false
        }
      ]

GET report/fieldProperties/{query_source_field_id}
---------------------------------------------------------

Returns the properties of query source field specified by query_source_field_id.

**Request**

    No payload

**Response**

    A :doc:`models/ReportQuerySource` object

**Samples**

   .. code-block:: http

      GET /api/report/fieldProperties/bd207050-e2a4-4128-9b5a-89409bee0377 HTTP/1.1

   Sample response::

      {
         "id": "d9728d5f-b6f6-462b-b988-8180bc733972",
         "name": "HumanResources.Employee",
         "type": "Table",
         "selected": false,
         "visible": true,
         "querySourceCategoryName": null,
         "connectionName": null,
         "isAlias": false,
         "fields": [{
            "id": "bd207050-e2a4-4128-9b5a-89409bee0377",
            "name": "Gender",
            "alias": "",
            "dataType": "nchar",
            "izendaDataType": "Text",
            "visible": true,
            "filterable": true,
            "extendedProperties": null,
            "isParameter": false,
            "allowDistinct": false
         }]
      }

POST report/calculatedField
------------------------------------------------

Saves a calculated field after validating name duplication (does not validate the expression).

**Request**

    Payload: a :doc:`models/ReportCalculatedFieldParameter` object

**Response**

    An array containing exactly one :doc:`models/ReportDataSourceCategory` object

**Samples**

   .. code-block:: http

      POST /api/report/calculatedField HTTP/1.1

   Request payload to add a calculated field [MoneyInStock] from [UnitPrice] * [UnitsInStock]::

      {
        "reportKey" : {
           "key" : "681dc08e-4355-441f-a438-370d5c1a7a99"
        },
        "calculatedField" : {
           "id" : null,
           "name" : "MoneyInStock",
           "functionName" : "[None]",
           "expression" : "[Northwind].[dbo].[Products].[UnitPrice] * [Northwind].[dbo].[Products].[UnitsInStock]",
           "izendaDataType" : "Money"
        }
      }

   Sample response::

      {
        "id" : null,
        "name" : "Calculated Fields",
        "querySource" : [{
              "id" : "00000000-0000-0000-0000-000000000000",
              "name" : "Calculated Fields",
              "originalName" : null,
              "type" : null,
              "selected" : false,
              "visible" : true,
              "querySourceCategoryName" : null,
              "connectionName" : null,
              "isAlias" : false,
              "fields" : [{
                    "name" : "MoneyInStock",
                    "alias" : "",
                    "dataType" : "",
                    "izendaDataType" : "Money",
                    "allowDistinct" : true,
                    "visible" : true,
                    "filterable" : true,
                    "deleted" : false,
                    "querySourceId" : "00000000-0000-0000-0000-000000000000",
                    "parentId" : null,
                    "expressionFields" : [{
                          "fieldId" : "3f79de74-1152-4896-b966-ea82849efece",
                          "fieldName" : "UnitPrice",
                          "fieldNameAlias" : "",
                          "dataFieldType" : "Money",
                          "querySourceId" : "e1bc2021-3874-4e5a-b51e-d799cef5e29a",
                          "querySourceType" : "Table",
                          "sourceAlias" : "Products",
                          "relationshipId" : "00000000-0000-0000-0000-000000000000",
                          "visible" : true,
                          "reportId" : null,
                          "fieldFunctionExpression" : null,
                          "expression" : "[Northwind].[dbo].[Products].[UnitPrice]",
                          "grandTotalExpression" : null,
                          "subTotalExpression" : null,
                          "sort" : "Unsorted",
                          "function" : null,
                          "calculatedTree" : null,
                          "grandTotalTree" : null
                       }, {
                          "fieldId" : "54c13d3b-d8fe-4e78-a710-230d3d794039",
                          "fieldName" : "UnitsInStock",
                          "fieldNameAlias" : "",
                          "dataFieldType" : "Numeric",
                          "querySourceId" : "e1bc2021-3874-4e5a-b51e-d799cef5e29a",
                          "querySourceType" : "Table",
                          "sourceAlias" : "Products",
                          "relationshipId" : "00000000-0000-0000-0000-000000000000",
                          "visible" : true,
                          "reportId" : null,
                          "fieldFunctionExpression" : null,
                          "expression" : "[Northwind].[dbo].[Products].[UnitsInStock]",
                          "grandTotalExpression" : null,
                          "subTotalExpression" : null,
                          "sort" : "Unsorted",
                          "function" : null,
                          "calculatedTree" : null,
                          "grandTotalTree" : null
                       }
                    ],
                    "filteredValue" : "",
                    "type" : 0,
                    "groupPosition" : 0,
                    "position" : 0,
                    "extendedProperties" : "[{" FieldId ":" 3f79de74 - 1152 - 4896 - b966 - ea82849efece "," FieldName ":" UnitPrice "," FieldNameAlias ":" "," DataFieldType ":" Money "," QuerySourceId ":" e1bc2021 - 3874 - 4e5a - b51e - d799cef5e29a "," QuerySourceType ":" Table "," SourceAlias ":" Products "," RelationshipId ":" 00000000 - 0000 - 0000 - 0000 - 000000000000 "," Visible ":true," ReportId ":null," FieldFunctionExpression ":null," Expression ":"[Northwind].[dbo].[Products].[UnitPrice]"," GrandTotalExpression ":null," SubTotalExpression ":null," Sort ":" Unsorted "," Function ":null," CalculatedTree ":null," GrandTotalTree ":null},{" FieldId ":" 54c13d3b - d8fe - 4e78 - a710 - 230d3d794039 "," FieldName ":" UnitsInStock "," FieldNameAlias ":" "," DataFieldType ":" Numeric "," QuerySourceId ":" e1bc2021 - 3874 - 4e5a - b51e - d799cef5e29a "," QuerySourceType ":" Table "," SourceAlias ":" Products "," RelationshipId ":" 00000000 - 0000 - 0000 - 0000 - 000000000000 "," Visible ":true," ReportId ":null," FieldFunctionExpression ":null," Expression ":"[Northwind].[dbo].[Products].[UnitsInStock]"," GrandTotalExpression ":null," SubTotalExpression ":null," Sort ":" Unsorted "," Function ":null," CalculatedTree ":null," GrandTotalTree ":null}]",
                    "physicalChange" : 0,
                    "approval" : 0,
                    "existed" : false,
                    "matchedTenant" : false,
                    "functionName" : "[None]",
                    "expression" : "[Northwind].[dbo].[Products].[UnitPrice] * [Northwind].[dbo].[Products].[UnitsInStock]",
                    "fullName" : null,
                    "calculatedTree" : null,
                    "reportId" : "00000000-0000-0000-0000-000000000000",
                    "originalName" : null,
                    "isParameter" : false,
                    "isCalculated" : true,
                    "querySource" : null,
                    "id" : "fc6ea2e3-8a30-4f2a-b2ba-6f33dd2fdb07",
                    "state" : 0,
                    "modified" : "2016-06-24T00:46:25.63744"
                 }
              ]
           }
        ]
      }

GET report/calculatedField/{calculated_field_id}
------------------------------------------------

Returns the calculated field specified by calculated_field_id.

**Request**

    No payload

**Response**

    A :doc:`models/QuerySourceField` object

**Samples**

   .. code-block:: http

      GET /api/report/calculatedField/52c55f01-b347-4a23-b089-32f8e1db05fe HTTP/1.1

   Sample response::

      {
         "name": "MoneyInStock",
         "alias": "",
         "dataType": "",
         "izendaDataType": "Money",
         "allowDistinct": true,
         "visible": true,
         "filterable": true,
         "deleted": false,
         "querySourceId": "00000000-0000-0000-0000-000000000000",
         "parentId": null,
         "expressionFields": [{
            "fieldId": "3f79de74-1152-4896-b966-ea82849efece",
            "fieldName": "UnitPrice",
            "fieldNameAlias": "",
            "dataFieldType": "Money",
            "querySourceId": "e1bc2021-3874-4e5a-b51e-d799cef5e29a",
            "querySourceType": "Table",
            "sourceAlias": "Products",
            "relationshipId": "00000000-0000-0000-0000-000000000000",
            "visible": true,
            "reportId": null,
            "fieldFunctionExpression": null,
            "expression": "[Northwind].[dbo].[Products].[UnitPrice]",
            "grandTotalExpression": null,
            "subTotalExpression": null,
            "sort": "Unsorted",
            "function": null,
            "calculatedTree": null,
            "grandTotalTree": null
         }, {
            "fieldId": "54c13d3b-d8fe-4e78-a710-230d3d794039",
            "fieldName": "UnitsInStock",
            "fieldNameAlias": "",
            "dataFieldType": "Numeric",
            "querySourceId": "e1bc2021-3874-4e5a-b51e-d799cef5e29a",
            "querySourceType": "Table",
            "sourceAlias": "Products",
            "relationshipId": "00000000-0000-0000-0000-000000000000",
            "visible": true,
            "reportId": null,
            "fieldFunctionExpression": null,
            "expression": "[Northwind].[dbo].[Products].[UnitsInStock]",
            "grandTotalExpression": null,
            "subTotalExpression": null,
            "sort": "Unsorted",
            "function": null,
            "calculatedTree": null,
            "grandTotalTree": null
         }],
         "filteredValue": "",
         "type": 0,
         "groupPosition": 0,
         "position": 0,
         "extendedProperties": "[{\"FieldId\":\"3f79de74-1152-4896-b966-ea82849efece\",\"FieldName\":\"UnitPrice\",\"FieldNameAlias\":\"\",\"DataFieldType\":\"Money\",\"QuerySourceId\":\"e1bc2021-3874-4e5a-b51e-d799cef5e29a\",\"QuerySourceType\":\"Table\",\"SourceAlias\":\"Products\",\"RelationshipId\":\"00000000-0000-0000-0000-000000000000\",\"Visible\":true,\"ReportId\":null,\"FieldFunctionExpression\":null,\"Expression\":\"[Northwind].[dbo].[Products].[UnitPrice]\",\"GrandTotalExpression\":null,\"SubTotalExpression\":null,\"Sort\":\"Unsorted\",\"Function\":null,\"CalculatedTree\":null,\"GrandTotalTree\":null},{\"FieldId\":\"54c13d3b-d8fe-4e78-a710-230d3d794039\",\"FieldName\":\"UnitsInStock\",\"FieldNameAlias\":\"\",\"DataFieldType\":\"Numeric\",\"QuerySourceId\":\"e1bc2021-3874-4e5a-b51e-d799cef5e29a\",\"QuerySourceType\":\"Table\",\"SourceAlias\":\"Products\",\"RelationshipId\":\"00000000-0000-0000-0000-000000000000\",\"Visible\":true,\"ReportId\":null,\"FieldFunctionExpression\":null,\"Expression\":\"[Northwind].[dbo].[Products].[UnitsInStock]\",\"GrandTotalExpression\":null,\"SubTotalExpression\":null,\"Sort\":\"Unsorted\",\"Function\":null,\"CalculatedTree\":null,\"GrandTotalTree\":null}]",
         "physicalChange": 0,
         "approval": 0,
         "existed": false,
         "matchedTenant": false,
         "functionName": "[None]",
         "expression": "[Northwind].[dbo].[Products].[UnitPrice] * [Northwind].[dbo].[Products].[UnitsInStock]",
         "fullName": null,
         "calculatedTree": null,
         "reportId": "ba7cc132-689c-43fd-8fc8-272c5162d263",
         "originalName": null,
         "isParameter": false,
         "isCalculated": true,
         "querySource": null,
         "id": "52c55f01-b347-4a23-b089-32f8e1db05fe",
         "state": 0,
         "modified": "2016-06-24T08:38:51.367"
      }

GET report/hasReportUseCalculatedField/{calculated_field_id}
-------------------------------------------------------------------

Returns true if the calculated field is being used in any report part or report filter.

**Request**

    No payload

**Response**

    * true if the calculated field is being used in any report part or report filter
    * false if none

**Samples**

   .. code-block:: http

      GET /api/report/hasReportUseCalculatedField/BC0E2AA2-8310-429E-8212-00FC4863A559 HTTP/1.1

   Response::

      false

GET report/hasReportUseRelationship/{relationship_id}
--------------------------------------------------------------

Returns true if the relationship is being used in report.

**Request**

    No payload

**Response**

    * true if the relationship is being used in report
    * false if none

**Samples**

   .. code-block:: http

      GET /api/report/hasReportUseRelationship/f7ee0950-f203-4c56-b2db-c04728edae36 HTTP/1.1

   Response::

      true

POST report/deleteReportCalculatedField
------------------------------------------------

Removes a calculated field from report.

**Request**

    Payload: a :doc:`models/ReportCalculatedFieldParameter` object

**Response**

    * true if the calculated field was removed successfully
    * an :doc:`models/OperationResult` object with **messages** field populated if not

**Samples**

   .. code-block:: http

      POST /api/report/deleteReportCalculatedField HTTP/1.1

   Request payload::

      {
        "reportKey" : {
           "key" : "aef4b8eb-1b4c-41e3-b1c5-d227970007c3"
        },
        "calculatedField" : {
           "id" : "fe94ab0d-2063-4d2d-8931-0d2a9185658b"
        }
      }

   Response if success::

      true

   Response in case of error::

      {
         "success": false,
         "messages": [{
            "key": "",
            "messages": ["There is an error while process request. Please contact administrator."]
         }]
      }
