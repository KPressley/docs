

============================
Dashboard List APIs
============================

The **Dashboard List and Viewer** page allows user to

.. hlist::
   :columns: 2

   * browse and search for dashboards
   * copy and move dashboards between categories
   * switch to presentation mode
   * configure sharing access
   * set up subscriptions and scheduled delivery
   * rename and delete dashboards and dashboard categories
   * print dashboard
   * email dashboard
   * design dashboard
   * configure child report parts:
   
     * view, quick edit and design report parts in new window
     * print and export report parts
     * re-select report parts

This page documents the APIs related to dashboard list.

List of APIs
------------

.. list-table::
   :class: apitable
   :widths: 35 65
   :header-rows: 1

   * - API
     - Purpose
   * - `GET dashboard/categories/(tenant_id)`_
     - Returns an array of dashboard categories, filtered by tenant_id if available.
   * - `POST dashboard/category`_
     - Renames a dashboard category.
   * - `DELETE dashboard/category/{dashboard_category_id}`_
     - Removes a dashboard category.
   * - `POST dashboard/validateCategoryName`_
     - Validates dashboard category name.
   * - `GET dashboard/emailTemplates/{isSubscription}`_
     - Returns a list of dashboard email templates.
   * - `POST dashboard/rename`_
     - Renames a dashboard.
   * - `POST dashboard/move`_
     - Moves a dashboard to another category / sub-category.
   * - `POST dashboard/copy`_
     - Copies a dashboard.
   * - `DELETE dashboard/{dashboard_id}`_
     - Deletes a dashboard.
   * - `POST dashboard/loadFilterDescription`_
     - Returns a list of report filters data including a filter description.
   * - `POST dashboard/list`_
     - Returns dashboards for the dashboard list screen.
   * - `POST dashboard/search`_
     - Searches dashboards.
   * - `POST dashboard/loadAccesses`_
     - Returns a list of user permissions for dashboard.


GET dashboard/categories/(tenant_id)
--------------------------------------------------------------

Returns an array of dashboard categories, filtered by tenant_id if available.

**Request**

    No payload

**Response**

    A :doc:`models/Category` object

**Samples**

   .. code-block:: http

      GET /api/dashboard/categories HTTP/1.1

   Response::
      
      [{
           "dashboards" : null,
           "subCategories" : [{
                 "dashboards" : null,
                 "subCategories" : null,
                 "name" : "Subcategory01",
                 "type" : 2,
                 "parentId" : "f5cd474e-0f96-4e59-9612-e98bc3f1d10a",
                 "tenantId" : null,
                 "canDelete" : false,
                 "status" : 2,
                 "id" : "60d72d90-5a1f-48ab-8b83-072a322669fb",
                 "state" : 0,
                 "inserted" : true,
                 "version" : null,
                 "created" : null,
                 "createdBy" : null,
                 "modified" : null,
                 "modifiedBy" : null
              }
           ],
           "name" : "Category01",
           "type" : 2,
           "parentId" : null,
           "tenantId" : null,
           "canDelete" : false,
           "status" : 2,
           "id" : "f5cd474e-0f96-4e59-9612-e98bc3f1d10a",
           "state" : 0,
           "inserted" : true,
           "version" : null,
           "created" : null,
           "createdBy" : null,
           "modified" : null,
           "modifiedBy" : null
        }, {
           "dashboards" : null,
           "subCategories" : null,
           "name" : "Uncategorized",
           "type" : 2,
           "parentId" : null,
           "tenantId" : null,
           "canDelete" : false,
           "status" : 1,
           "id" : "00000000-0000-0000-0000-000000000000",
           "state" : 0,
           "inserted" : true,
           "version" : null,
           "created" : null,
           "createdBy" : null,
           "modified" : null,
           "modifiedBy" : null
        }
      ]
      

POST dashboard/category
--------------------------------------------------------------

Renames a dashboard category.

**Request**

    Payload: a :doc:`models/Category` object

**Response**

    An :doc:`models/OperationResult` object, with **success** field true if the rename is successful

**Samples**

   .. code-block:: http

      POST /api/dashboard/category HTTP/1.1

   Request payload::
      
      {
        "id" : "709742d0-2300-4f99-8cdd-1e1675d7c2e7",
        "type" : 2,
        "name" : "Category02",
        "parentId" : null,
        "tenantId" : null,
        "status" : 2,
        "state" : 0,
        "modified" : null,
        "canDelete" : false,
        "subCategories" : [],
        "dashboards" : [],
        "reports" : []
      }
      
   Sample response::
      
      {
        "success" : true,
        "messages" : null
      }

DELETE dashboard/category/{dashboard_category_id}
--------------------------------------------------------------

Removes a dashboard category.

**Request**

    No payload

**Response**

    An :doc:`models/OperationResult` object, with **success** field true if the removal is successful

**Samples**

   .. code-block:: http

      DELETE /api/dashboard/category/709742d0-2300-4f99-8cdd-1e1675d7c2e7 HTTP/1.1

   Sample response::
      
      {
        "success" : true,
        "messages" : null
      }

POST dashboard/validateCategoryName
--------------------------------------------------------------

Validates dashboard category name.

**Request**

    Payload: a :doc:`models/Category` object

**Response**

    An :doc:`models/OperationResult` object, with **success** field true if the category name is valid

**Samples**

   To be updated

GET dashboard/emailTemplates/{isSubscription}
--------------------------------------------------------------

Returns a list of dashboard email templates.

**Request**

    No payload
    
    isSubscription
    
      * 1 = for Subcriptions
      * 0 = not

**Response**

    An array of following objects

    .. list-table::
       :header-rows: 1

       *  -  Field
          -  Description
          -  Note
       *  -  **key** |br|
             string
          -  The type of the template
          -
       *  -  **value** |br|
             string
          -  The content of the template
          -

**Samples**

   .. code-block:: http

      GET /api/dashboard/emailTemplates/0 HTTP/1.1

   Sample response::
      
      [{
           "key" : "Attachment",
           "value" : "Dear {currentUserName},\n <br/>\n <br/> \n Please see report in the attachment.\n <br/>\n <br/>\n Regards,"
        }, {
           "key" : "Embedded HTML",
           "value" : "Dear {currentUserName},\n <br/>\n <br/> \n Please see the following report.\n <br/>\n <br/> \n {embedReportHTML}\n <br/>\n <br/>\n Regards,"
        }, {
           "key" : "Link",
           "value" : "Dear {currentUserName},\n <br/>\n <br/> \n Please see report in the following link.\n <br/>\n <br/> \n {reportLink}\n <br/>\n <br/> \n Regards,"
        }
      ]

POST dashboard/rename
--------------------------------------------------------------

Renames a dashboard.

**Request**

    Payload: a :doc:`models/DashboardRenameParameter` object

**Response**

    * true if the rename was successful
    * false if not

**Samples**

   .. code-block:: http

      POST /api/dashboard/rename HTTP/1.1

   Request payload::
      
      {
        "tenantId" : null,
        "dashboardId" : "a496ad94-fe92-48d5-a285-e45be738921f",
        "name" : "TestDashboard02"
      }
      
   Response::
      
      true

POST dashboard/move
--------------------------------------------------------------

Moves a dashboard to another category / sub-category.

**Request**

    Payload: a :doc:`models/DashboardDefinition` object

**Response**

    * true if the move was successful
    * false if not

**Samples**

   .. code-block:: http

      POST /api/dashboard/move HTTP/1.1

   Request payload::
      
      {
        "id" : "a496ad94-fe92-48d5-a285-e45be738921f",
        "name" : "TestDashboard01",
        "categoryId" : null,
        "categoryName" : "Category03",
        "subCategoryId" : null,
        "subCategoryName" : ""
      }
      
   Response::
      
      true

POST dashboard/copy
--------------------------------------------------------------

Copies a dashboard.

**Request**

    Payload: a :doc:`models/DashboardDefinition` object

**Response**

    A :doc:`models/DashboardDefinition` object

**Samples**

   .. code-block:: http

      POST /api/dashboard/copy HTTP/1.1

   Request payload::
      
      {
        "id" : "57ce3bb7-3d13-415f-88b6-51dc476008ae",
        "name" : "TestDashboard02",
        "categoryId" : null,
        "categoryName" : "Category02",
        "subCategoryId" : null,
        "subCategoryName" : ""
      }
      
   .. container:: toggle

      .. container:: header

         Sample response:

      .. code-block:: json

         {
           "commonFilterFields" : [],
           "accesses" : [],
           "subscriptions" : [],
           "dashboardParts" : [{
                 "dashboardId" : "1b4317fd-490a-4c34-bc61-dcbd7a5ff9dc",
                 "type" : null,
                 "title" : null,
                 "reportId" : null,
                 "reportPartId" : null,
                 "filterDescription" : null,
                 "numberOfRecord" : -1,
                 "positionX" : 0,
                 "positionY" : 4,
                 "width" : 6,
                 "height" : 4,
                 "filters" : [],
                 "dashboardPartContent" : {
                    "contentTitle" : {
                       "text" : "",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentDescription" : {
                       "text" : "",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentFromPreset" : true,
                    "bodyContent" : {
                       "text" : "",
                       "config" : {
                          "fontFamily" : "Roboto",
                          "fontSize" : 14,
                          "bold" : false,
                          "italic" : false,
                          "underline" : false,
                          "strikethrough" : false,
                          "textColor" : "",
                          "backgroundColor" : "",
                          "alignleft" : false,
                          "aligncenter" : false,
                          "alignright" : false,
                          "alignjustify" : false,
                          "bullet" : "",
                          "numbered" : "",
                          "alignTop" : false,
                          "alignMiddle" : false,
                          "alignBottom" : false
                       }
                    }
                 },
                 "id" : "fba896ff-14ed-4576-911d-96ba78b2214a",
                 "state" : 0,
                 "inserted" : false,
                 "version" : 1,
                 "created" : "2016-08-11T03:20:08.793",
                 "createdBy" : null,
                 "modified" : "2016-08-11T03:20:08.793",
                 "modifiedBy" : null
              }, {
                 "dashboardId" : "1b4317fd-490a-4c34-bc61-dcbd7a5ff9dc",
                 "type" : null,
                 "title" : null,
                 "reportId" : null,
                 "reportPartId" : null,
                 "filterDescription" : null,
                 "numberOfRecord" : -1,
                 "positionX" : 6,
                 "positionY" : 4,
                 "width" : 6,
                 "height" : 4,
                 "filters" : [],
                 "dashboardPartContent" : {
                    "contentTitle" : {
                       "text" : "",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentDescription" : {
                       "text" : "",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentFromPreset" : true,
                    "bodyContent" : {
                       "text" : "",
                       "config" : {
                          "fontFamily" : "Roboto",
                          "fontSize" : 14,
                          "bold" : false,
                          "italic" : false,
                          "underline" : false,
                          "strikethrough" : false,
                          "textColor" : "",
                          "backgroundColor" : "",
                          "alignleft" : false,
                          "aligncenter" : false,
                          "alignright" : false,
                          "alignjustify" : false,
                          "bullet" : "",
                          "numbered" : "",
                          "alignTop" : false,
                          "alignMiddle" : false,
                          "alignBottom" : false
                       }
                    }
                 },
                 "id" : "ca9dec28-3a4a-48f0-bfe3-cb420eeca25f",
                 "state" : 0,
                 "inserted" : false,
                 "version" : 1,
                 "created" : "2016-08-11T03:20:08.793",
                 "createdBy" : null,
                 "modified" : "2016-08-11T03:20:08.793",
                 "modifiedBy" : null
              }, {
                 "dashboardId" : "1b4317fd-490a-4c34-bc61-dcbd7a5ff9dc",
                 "type" : "text",
                 "title" : "text",
                 "reportId" : null,
                 "reportPartId" : null,
                 "filterDescription" : null,
                 "numberOfRecord" : -1,
                 "positionX" : 0,
                 "positionY" : 0,
                 "width" : 12,
                 "height" : 4,
                 "filters" : [],
                 "dashboardPartContent" : {
                    "contentTitle" : {
                       "text" : "A Title",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentDescription" : {
                       "text" : "desc",
                       "settings" : {
                          "fontFamily" : "",
                          "fontSize" : 14,
                          "fontBold" : true,
                          "fontItalic" : false,
                          "fontUnderline" : false,
                          "fontColor" : "",
                          "fontHighlightColor" : "",
                          "alignment" : ""
                       }
                    },
                    "contentFromPreset" : true,
                    "bodyContent" : {
                       "text" : "",
                       "config" : {
                          "fontFamily" : "Roboto",
                          "fontSize" : 14,
                          "bold" : false,
                          "italic" : false,
                          "underline" : false,
                          "strikethrough" : false,
                          "textColor" : "",
                          "backgroundColor" : "",
                          "alignleft" : false,
                          "aligncenter" : false,
                          "alignright" : false,
                          "alignjustify" : false,
                          "bullet" : "",
                          "numbered" : "",
                          "alignTop" : false,
                          "alignMiddle" : false,
                          "alignBottom" : false
                       }
                    }
                 },
                 "id" : "01ff4872-812a-495f-a8ea-52923162b350",
                 "state" : 0,
                 "inserted" : false,
                 "version" : 1,
                 "created" : "2016-08-11T03:20:08.777",
                 "createdBy" : null,
                 "modified" : "2016-08-11T03:20:08.777",
                 "modifiedBy" : null
              }
           ],
           "name" : "TestDashboard02",
           "description" : null,
           "categoryId" : "4c74e214-9891-460a-9571-8f6bd65bc72b",
           "categoryName" : null,
           "subCategoryId" : null,
           "subCategoryName" : null,
           "tenantId" : null,
           "imageUrl" : null,
           "stretchImage" : false,
           "backgroundColor" : null,
           "showFilterDescription" : true,
           "lastViewed" : null,
           "id" : "1b4317fd-490a-4c34-bc61-dcbd7a5ff9dc",
           "state" : 0,
           "inserted" : true,
           "version" : 1,
           "created" : "2016-08-11T03:20:08.777",
           "createdBy" : null,
           "modified" : "2016-08-11T03:20:08.777",
           "modifiedBy" : null
         }

DELETE dashboard/{dashboard_id}
--------------------------------------------------------------

Deletes a dashboard.

**Request**

    No payload

**Response**

    * true if the deletion was successful
    * false if not

**Samples**

   .. code-block:: http

      DELETE /api/dashboard/1b4317fd-490a-4c34-bc61-dcbd7a5ff9dc HTTP/1.1

   Sample response::
      
      true

POST dashboard/loadFilterDescription
--------------------------------------------------------------

Returns a list of report filters data including a filter description.

**Request**

   The following object
    
   .. list-table::
      :header-rows: 1

      *  -  Field
         -  Description
         -  Note
      *  -  **reportIds** |br|
            array of strings (GUIDs)
         -  An array of ids of reports
         -
      *  -  **dashboardPart** |br|
            object
         -  A :doc:`models/DashboardPart` object
         -

**Response**

    A :doc:`models/DashboardPart` object, with the **filters** field populated 

**Samples**

   .. code-block:: http

      POST /api/dashboard/loadFilterDescription HTTP/1.1

   Request payload::
      
      {
        "reportIds" : [],
        "dashboardPart" : {
           "reportId" : "babe2f8c-a9b9-4a28-98b9-426b8c15497c",
           "reportPartId" : "48c238bb-1296-44bc-bd16-c7e09bdad1ac",
           "filters" : [{
                 "filterFieldId" : "d192bde7-0e51-4daa-8113-d3d79b539337",
                 "value" : "USA",
                 "operatorId" : "737307d1-1e5f-407f-889f-1b3c9a66dd6f",
                 "displayName" : "ShipCountry"
              }
           ]
        }
      }
      
   Sample response::
      
      {
        "dashboardId" : null,
        "type" : null,
        "title" : null,
        "reportId" : "babe2f8c-a9b9-4a28-98b9-426b8c15497c",
        "reportPartId" : "48c238bb-1296-44bc-bd16-c7e09bdad1ac",
        "filterDescription" : "ShipCountry = USA",
        "numberOfRecord" : 0,
        "positionX" : 0,
        "positionY" : 0,
        "width" : 0,
        "height" : 0,
        "filters" : [{
              "filterFieldId" : "d192bde7-0e51-4daa-8113-d3d79b539337",
              "value" : "USA",
              "operatorId" : "737307d1-1e5f-407f-889f-1b3c9a66dd6f",
              "displayName" : "ShipCountry",
              "dashboardPartId" : "00000000-0000-0000-0000-000000000000",
              "filterField" : null,
              "isCommon" : false,
              "id" : null,
              "state" : 0,
              "inserted" : true,
              "version" : null,
              "created" : null,
              "createdBy" : null,
              "modified" : null,
              "modifiedBy" : null
           }
        ],
        "dashboardPartContent" : null,
        "id" : null,
        "state" : 0,
        "inserted" : true,
        "version" : null,
        "created" : null,
        "createdBy" : null,
        "modified" : null,
        "modifiedBy" : null
      }

POST dashboard/list
--------------------------------------------------------------

Returns dashboards for the dashboard list screen.

**Request**

    Payload: a :doc:`models/ReportDashboardSearchCriteria` object

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      POST /api/dashboard/list HTTP/1.1

   Request payload::
      
      {
        "tenantId" : null,
        "isUncategorized" : false,
        "criterias" : [{
              "key" : "CategoryId"
           }
        ]
      }
      
   Sample response::
      
      [{
           "dashboards" : [],
           "subCategories" : [{
                 "dashboards" : [{
                       "name" : "Sample Dashboard",
                       "description" : null,
                       "categoryId" : "aba44e94-ffbb-4435-83fa-5ca659589fc7",
                       "categoryName" : "Category01",
                       "subCategoryId" : null,
                       "subCategoryName" : null,
                       "tenantId" : "00000000-0000-0000-0000-000000000000",
                       "imageUrl" : null,
                       "stretchImage" : false,
                       "backgroundColor" : null,
                       "showFilterDescription" : false,
                       "lastViewed" : null,
                       "id" : "f464b993-f632-4e4b-9462-1e2bfc1cace1",
                       "state" : 0,
                       "inserted" : true,
                       "version" : 2,
                       "created" : null,
                       "createdBy" : null,
                       "modified" : "2016-08-23T03:21:22.9100000-07:00",
                       "modifiedBy" : null
                    }
                 ],
                 "subCategories" : null,
                 "name" : null,
                 "type" : 0,
                 "parentId" : null,
                 "tenantId" : null,
                 "canDelete" : false,
                 "status" : 0,
                 "id" : "00000000-0000-0000-0000-000000000000",
                 "state" : 0,
                 "inserted" : true,
                 "version" : null,
                 "created" : null,
                 "createdBy" : null,
                 "modified" : null,
                 "modifiedBy" : null
              }
           ],
           "name" : "Category01",
           "type" : 0,
           "parentId" : null,
           "tenantId" : null,
           "canDelete" : false,
           "status" : 2,
           "id" : "aba44e94-ffbb-4435-83fa-5ca659589fc7",
           "state" : 0,
           "inserted" : true,
           "version" : null,
           "created" : null,
           "createdBy" : null,
           "modified" : null,
           "modifiedBy" : null
        }, {
           "dashboards" : [],
           "subCategories" : [{
                 "dashboards" : [{
                       "name" : "Dashboard123",
                       "description" : null,
                       "categoryId" : null,
                       "categoryName" : null,
                       "subCategoryId" : null,
                       "subCategoryName" : null,
                       "tenantId" : "00000000-0000-0000-0000-000000000000",
                       "imageUrl" : null,
                       "stretchImage" : false,
                       "backgroundColor" : null,
                       "showFilterDescription" : false,
                       "lastViewed" : null,
                       "id" : "70e9555c-34c4-44e4-b4d0-8a60f0e73a6c",
                       "state" : 0,
                       "inserted" : true,
                       "version" : 4,
                       "created" : null,
                       "createdBy" : null,
                       "modified" : "2016-08-23T03:19:59.8930000-07:00",
                       "modifiedBy" : null
                    }, {
                       "name" : "Dashboard4",
                       "description" : null,
                       "categoryId" : null,
                       "categoryName" : null,
                       "subCategoryId" : null,
                       "subCategoryName" : null,
                       "tenantId" : "00000000-0000-0000-0000-000000000000",
                       "imageUrl" : null,
                       "stretchImage" : false,
                       "backgroundColor" : null,
                       "showFilterDescription" : false,
                       "lastViewed" : null,
                       "id" : "79b09ae9-de5d-4e52-b441-66f494511de1",
                       "state" : 0,
                       "inserted" : true,
                       "version" : 2,
                       "created" : null,
                       "createdBy" : null,
                       "modified" : "2016-08-23T03:20:10.5630000-07:00",
                       "modifiedBy" : null
                    }
                 ],
                 "subCategories" : null,
                 "name" : null,
                 "type" : 0,
                 "parentId" : null,
                 "tenantId" : null,
                 "canDelete" : false,
                 "status" : 0,
                 "id" : "00000000-0000-0000-0000-000000000000",
                 "state" : 0,
                 "inserted" : true,
                 "version" : null,
                 "created" : null,
                 "createdBy" : null,
                 "modified" : null,
                 "modifiedBy" : null
              }
           ],
           "name" : null,
           "type" : 0,
           "parentId" : null,
           "tenantId" : null,
           "canDelete" : false,
           "status" : 0,
           "id" : "00000000-0000-0000-0000-000000000000",
           "state" : 0,
           "inserted" : true,
           "version" : null,
           "created" : null,
           "createdBy" : null,
           "modified" : null,
           "modifiedBy" : null
        }
      ]

POST dashboard/search
--------------------------------------------------------------

Searches dashboards.

**Request**

    Payload: a :doc:`models/ReportDashboardSearchCriteria` object

**Response**

    An array of :doc:`models/Category` objects

**Samples**

   .. code-block:: http

      POST /api/dashboard/search HTTP/1.1

   Request payload::
      
      {
        "criterias": [
          {
            "key": "All",
            "value": "1"
          }
        ],
        "isUncategorized": false,
        "sortCriteria": {
          "key": "DashboardName",
          "descending": false
        },
        "tenantId": null
      }
      
   Sample response::
      
      [
        {
          "dashboards": [],
          "name": "ABC",
          "type": 0,
          "parentId": null,
          "tenantId": null,
          "canDelete": false,
          "editable": false,
          "savable": false,
          "subCategories": [
            {
              "dashboards": [
                {
                 "name": "Dashboard 1",
                 "description": null,
                 "categoryId": "f0fd52d8-eef9-4ba7-b89d-6267be5e6b66",
                 "categoryName": "ABC",
                 "subCategoryId": "309dbfab-193d-48b7-9a76-c209c507d9d5",
                 "subCategoryName": "abc",
                 "tenantId": null,
                 "imageUrl": null,
                 "stretchImage": false,
                 "backgroundColor": null,
                 "showFilterDescription": false,
                 "lastViewed": "2016-11-17T04:08:56.9000000+14:00",
                 "owner": "Pa system admin",
                 "ownerId": "0fa44ace-abd7-4a8d-928e-c84ec2999dfe",
                 "createdById": "0fa44ace-abd7-4a8d-928e-c84ec2999dfe",
                 "modifiedById": null,
                 "numberOfView": 1,
                 "renderingTime": 13010,
                 "deletable": true,
                 "editable": true,
                 "movable": true,
                 "copyable": true,
                 "accessPriority": 1,
                 "id": "a087f614-d55e-4c53-89f5-04e4fddd173a",
                 "state": 0,
                 "deleted": false,
                 "inserted": true,
                 "version": 1,
                 "created": "2016-11-12T10:35:32.3500000+14:00",
                 "createdBy": "Pa system admin",
                 "modified": "2016-11-12T10:35:32.3500000+14:00",
                 "modifiedBy": "Pa system admin"
                }
              ],
              "name": "abc",
              "type": 0,
              "parentId": null,
              "tenantId": null,
              "canDelete": false,
              "editable": false,
              "savable": false,
              "subCategories": [],
              "status": 2,
              "id": "309dbfab-193d-48b7-9a76-c209c507d9d5",
              "state": 0,
              "deleted": false,
              "inserted": true,
              "version": null,
              "created": null,
              "createdBy": null,
              "modified": null,
              "modifiedBy": null
            }
          ],
          "status": 2,
          "id": "f0fd52d8-eef9-4ba7-b89d-6267be5e6b66",
          "state": 0,
          "deleted": false,
          "inserted": true,
          "version": null,
          "created": null,
          "createdBy": null,
          "modified": null,
          "modifiedBy": null
        }
      ]

POST dashboard/loadAccesses
--------------------------------------------------------------

Returns a list of user permissions for dashboard.

**Request**

    Payload: an :doc:`models/AccessPagedRequest` object

**Response**

    A :doc:`models/PagedResult` object, with **result** field containing an array of :doc:`models/UserPermission` objects

**Samples**

   .. code-block:: http

      POST /api/dashboard/loadAccesses HTTP/1.1

   Request payload::
      
      {
        "dashboardId": "a3243533-166d-4377-90eb-add25edf6563",
        "criteria": [
          {
            "key": "All",
            "value": "",
            "operation": 1
          }
        ],
        "pageIndex": 1,
        "pageSize": 10,
        "sortOrders": [
          {
            "key": "shareWith",
            "descending": true
          }
        ]
      }
      
   Sample response::
      
      {
        "result": [],
        "pageIndex": 1,
        "pageSize": 10,
        "total": 0
      }
