=====================================
Authentication Authorisation Jio APIs
=====================================

Token
=====

----------------
Creating a token
----------------
Authenticates an identity and generates a token. Uses the password authentication method and scopes authorization to a
project or domain.
The request body must include a payload that specifies the password authentication method, the credentials, and the
project or domain authorization scope.

* Method: POST
* Path: /auth/tokens
* Header: N/A
* Body::

         {
             "auth": {
                 "identity": {
                     "methods": [
                         "password"
                     ],
                     "password": {
                         "user": {
                             "id": "",
                             "password": ""
                         }
                     }
                 },
                 "scope": {
                     "project": {
                         "domain": {
                             "id": ""
                         },
                         "id": ""
                     }
                 }
             }
         }

* Request Parameters

+-------------------------+---------------+--------------+-------------------------------------------------------------+
| Parameter               | Style         | Type         | Description                                                 |
+=========================+===============+==============+=============================================================+
| auth                    | plain         | xsd:dict     | An auth object.                                             |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| identity                | plain         | xsd:dict     | An identity object.                                         |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| methods                 | plain         | xsd:string   | The authentication method. Eg. password                     |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| password                | plain         | xsd:dict     | A password object. If password authentication method used.  |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| scope (Optional)        | plain         | xsd:string   | | The authorization scope. project or domain                |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| user                    | plain         | xsd:dict     | A user object.                                              |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| id (Optional)           | plain         | xsd:string   | ID of the user. Required if user name not specified.        |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| name (Optional)         | plain         | xsd:string   | | User name. Required ID of the user not specified.         |
|                         |               |              | | If you specify the user name, you must also specify the   |
|                         |               |              | | domain, by ID or name.                                    |
+-------------------------+---------------+--------------+-------------------------------------------------------------+
| password                | plain         | xsd:string   | The user password.                                          |
+-------------------------+---------------+--------------+-------------------------------------------------------------+

* Response codes: 200, 401, 403, 405, 413, 503, 404
* Response Body::

                {
                    "token": {
                        "expires_at": "2013-02-27T18:30:59.999999Z",
                        "issued_at": "2013-02-27T16:30:59.999999Z",
                        "methods": [
                            "password"
                        ],
                        "catalog": [
                            {
                                "type": "identity",
                                "id": "100",
                                "endpoints": [
                                    {
                                        "links": {
                                            "self": "https://region-a.example.com:35357/v3/endpoints/130_P"
                                        },
                                        "id": "example-a",
                                        "interface": "public",
                                        "region_id": "region-a.geo-1",
                                        "url": "https://region-a.example.com:35357/v2.0/",
                                        "service_id": "100"
                                    },
                                    {
                                        "links": {
                                            "self": "https://region-a.example.com:35357/v3/endpoints/example-a"
                                        },
                                        "id": "example-a",
                                        "interface": "public",
                                        "region_id": "region-a.geo-1",
                                        "url": "https://region-a.example.com:35357/v3/",
                                        "service_id": "100"
                                    }
                                ]
                            }
                        ],
                        "domain": {
                            "id": "1789d1",
                            "links": {
                                "self": "http://identity:35357/v3/domains/1789d1"
                            },
                            "name": "example.com"
                        },
                        "roles": [
                            {
                                "id": "76e72a",
                                "links": {
                                    "self": "http://identity:35357/v3/roles/76e72a"
                                },
                                "name": "admin"
                            },
                            {
                                "id": "f4f392",
                                "links": {
                                    "self": "http://identity:35357/v3/roles/f4f392"
                                },
                                "name": "member"
                            }
                        ],
                        "user": {
                            "domain": {
                                "id": "1789d1",
                                "links": {
                                    "self": "http://identity:35357/v3/domains/1789d1"
                                },
                                "name": "example.com"
                            },
                            "id": "0ca8f6",
                            "links": {
                                "self": "http://identity:35357/v3/users/0ca8f6"
                            },
                            "name": "Joe"
                        }
                    }
                }

* Response parameters

+------------------+--------+--------------+---------------------------------------------------------------------------+
| Parameter        | Style  | Type         | Description                                                               |
+==================+========+==============+===========================================================================+
| X-Subject-Token  | header | xsd:string   | | The authentication token.                                               |
|                  |        |              | | An authentication response returns the token ID in this header rather   |
|                  |        |              | | than in the response body.                                              |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| token            | plain  | xsd:string   | A token object.                                                           |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| methods          | plain  | xsd:string   | The authentication method, which is password, token, or both methods.     |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| roles            | plain  | xsd:string   | A roles object.  Its respective id, name in the object                    |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| project          | plain  | xsd:string   | A project object.                                                         |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| domain           | plain  | xsd:string   | A domain object.                                                          |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| catalog          | plain  | xsd:string   | A catalog object.                                                         |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| endpoints        | plain  |   xsd:string | An endpoints object. Contain id, url, region_id, region, interface        |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| region_id        | plain  | xsd:string   | (Since v3.2) The ID of the region that contains the service endpoint.     |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| interface        | plain  | xsd:string   | | The interface type, which describes the visibility of the endpoint.     |
|                  |        |              | | Value is:                                                               |
|                  |        |              | |                                                                         |
|                  |        |              | | public. Visible by end users on a publicly available network interface. |
|                  |        |              | |                                                                         |
|                  |        |              | | internal. Visible by end users on unmetered internal network interface  |
|                  |        |              | |                                                                         |
|                  |        |              | | admin. Visible by administrative users on a secure network interface.   |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| expires_at       | plain  | xsd:dateTime | The date and time when the token expires.                                 |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| extras           | plain  | xsd:dict     | A set of metadata key and value pairs, if any.                            |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| user             | plain  | xsd:string   | A user object.                                                            |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| domain           | plain  | xsd:dict     | A domain object.                                                          |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| audit_ids        | plain  | xsd:list     | | A list of one or two audit IDs. An audit ID is a unique, randomly       |
|                  |        |              | | generated URL-safe string that you can use to track a token.            |
+------------------+--------+--------------+---------------------------------------------------------------------------+
| issued_at        | plain  | xsd:dateTime | The date and time when the token was issued.                              |
+------------------+--------+--------------+---------------------------------------------------------------------------+


------------------
Validating a token
------------------

There are two ways to validate a token. One is GET and another is HEAD.
The token to be validated is passed with header X-Subject-Token.

- GET

Validates and shows information for a token, including its expiration date and authorization scope.
Pass your own token in the X-Auth-Token request header.
Pass the token that you want to validate in the X-Subject-Token request header.

* Method: GET
* Path: /auth/tokens
* Header: X-Auth-Token, X-Subject-Token
* Body: N/A
* Request parameters

+---------------------------+-----------------+------------+----------------------------------------------------------+
| Parameter                 |     Style       |   Type     | Description                                              |
+===========================+=================+============+==========================================================+
| X-Auth-Token              |     header      | xsd:string | A valid authentication token for an administrative user. |
+---------------------------+-----------------+------------+----------------------------------------------------------+
| X-Subject-Token(Optional) |     header      | xsd:string | | The authentication token for which you want to         |
|                           |                 |            | | perform the operation.                                 |
+---------------------------+-----------------+------------+----------------------------------------------------------+

* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body::
                {
                    "token": {
                        "expires_at": "2013-02-27T18:30:59.999999Z",
                        "issued_at": "2013-02-27T16:30:59.999999Z",
                        "methods": [
                            "password"
                        ],
                        "user": {
                            "domain": {
                                "id": "1789d1",
                                "links": {
                                    "self": "http://identity:35357/v3/domains/1789d1"
                                },
                                "name": "example.com"
                            },
                            "id": "0ca8f6",
                            "links": {
                                "self": "http://identity:35357/v3/users/0ca8f6"
                            },
                            "name": "Joe"
                        }
                    }
                }

* Response parameters

+------------------------+----------------+--------------+-------------------------------------------------------------+
| Parameter              | Style          | Type         | Description                                                 |
+========================+================+==============+=============================================================+
| X-Auth-Token           | header         | xsd:string   | A valid authentication token for an administrative user.    |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| X-Subject-Token        | header         | xsd:string   | | The authentication token.An authentication response       |
|                        |                |              | | returns the token ID in this header rather than in the    |
|                        |                |              | | response body.                                            |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| token                  | plain          | xsd:string   | A token object.                                             |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| expires_at             | plain          | xsd:dateTime | The date and time when the token expires.                   |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| issued_at              | plain          | xsd:dateTime | The date and time when the token issued.                    |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| methods                | plain          | xsd:string   | | The authentication method. Eg.password, token, or both    |
|                        |                |              | | methods.                                                  |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| user                   | plain          |  xsd:dict    | A user object.                                              |
+------------------------+----------------+--------------+-------------------------------------------------------------+
| domain (Optional)      | plain          | xsd:string   | Specify either id or name to uniquely identify the domain.  |
+------------------------+----------------+--------------+-------------------------------------------------------------+

- HEAD

* Method: HEAD
* Path: /auth/tokens
* Header: X-Auth-Token, X-Subject-Token
* Request parameters

+---------------------------+-----------------+------------+-----------------------------------------------------------+
| Parameter                 |     Style       |   Type     |             Description                                   |
+===========================+=================+============+===========================================================+
| X-Auth-Token              |     header      | xsd:string | A valid authentication token for an administrative user.  |
+---------------------------+-----------------+------------+-----------------------------------------------------------+
| X-Subject-Token(Optional) |     header      | xsd:string | The authentication token for which you want to            |
|                           |                 |            |  perform the operation.                                   |
+---------------------------+-----------------+------------+-----------------------------------------------------------+

* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body: None

----------------
Deleting a token
----------------

The token to be deleted is passed with header X-Subject-Token.

* Method: DELETE
* Path: /auth/tokens
* Header: X-Auth-Token, X-Subject-Token
* Body: N/A
* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body: None


Credentials
===========

---------------------
Creating a credential
---------------------

* Method: POST
* Path: /credentials
* Header: X-Auth-Token (REQUIRED)
* Request Body:
    {
        "credential": {
        "blob": "{\"access\":\"181920\",\"secret\":\"secretKey\"}",
        "project_id": "731fc6f265cd486d900f16e84c5cb594",
        "type": "ec2",
        "user_id": "bb5476fd12884539b41d5a88f838d773"
        }
    }

* Request parameters

+------------+------------+------------+-------------------------------------------------------------------------------+
| Parameter  |   Style    |   Type     | Description                                                                   |
+============+============+============+===============================================================================+
| credential |   plain    | xsd:dict   | A credential object.                                                          |
+------------+------------+------------+-------------------------------------------------------------------------------+
| blob       |   plain    | xsd:dict   | The credential itself, as a serialized blob.                                  |
+------------+------------+------------+-------------------------------------------------------------------------------+
| project_id |   plain    | csapi:UUID | The UUID for the associated project.                                          |
+------------+------------+------------+-------------------------------------------------------------------------------+
| type       |   plain    | xsd:string | | The credential type, such as ec2 or cert. The implementation determines the |
|            |            |            | | list of supported types.                                                    |
+------------+------------+------------+-------------------------------------------------------------------------------+
| user_id    |   plain    | xsd:string | The ID of the user who owns the credential.                                   |
+------------+------------+------------+-------------------------------------------------------------------------------+

* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body::

    {
        "credential": {
        "user_id": "bb5476fd12884539b41d5a88f838d773",
        "links": {
            "self": "http://localhost:5000/v3/credentials/3d3367228f9c7665266604462ec60029bcd83ad89614021a80b2eb879c572510"
        },
        "blob": "{\"access\":\"181920\",\"secret\":\"secretKey\"}",
        "project_id": "731fc6f265cd486d900f16e84c5cb594",
        "type": "ec2",
        "id": "3d3367228f9c7665266604462ec60029bcd83ad89614021a80b2eb879c572510"
        }
    }

* Response parameters

+------------+------------+------------+------------------------------------------------------------------------------+
| Parameter  |  Style     | Type       |  Description                                                                 |
+============+============+============+==============================================================================+
| credential |  plain     | xsd:dict   |  A credential object.                                                        |
+------------+------------+------------+------------------------------------------------------------------------------+
| user_id    |  plain     | xsd:string |  The ID of the user who owns the credential.                                 |
+------------+------------+------------+------------------------------------------------------------------------------+
| links      |  plain     | xsd:dict   |  The links for the credential resource.                                      |
+------------+------------+------------+------------------------------------------------------------------------------+
| blob       |  plain     | xsd:dict   |  The credential itself, as a serialized blob.                                |
+------------+------------+------------+------------------------------------------------------------------------------+
| project_id |  plain     | csapi:UUID |  The UUID for the associated project.                                        |
+------------+------------+------------+------------------------------------------------------------------------------+
| type       |  plain     | xsd:string | | The credential type, such as ec2 or cert. The implementation determines    |
|            |            |            | | the list of supported types.                                               |
+------------+------------+------------+------------------------------------------------------------------------------+
| id         |  plain     | csapi:UUID |  The UUID for the credential.                                                |
+------------+------------+------------+------------------------------------------------------------------------------+


-------------------------------
Getting details of a credential
-------------------------------

* Method: GET
* Path: /credentials/{credential_id}
* Header: X-Auth-Token
* Body: N/A
* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body::

    {
        "credential": {
            "user_id": "bb5476fd12884539b41d5a88f838d773",
            "links": {
                "self": "http://localhost:5000/v3/credentials/207e9b76935efc03804d3dd6ab52d22e9b22a0711e4ada4ff8b76165a07311d7"
            },
            "blob": "{\"access\": \"a42a27755ce6442596b049bd7dd8a563\", \"secret\": \"71faf1d40bb24c82b479b1c6fbbd9f0c\", \"trust_id\": null}",
            "project_id": "6e01855f345f4c59812999b5e459137d",
            "type": "ec2",
            "id": "207e9b76935efc03804d3dd6ab52d22e9b22a0711e4ada4ff8b76165a07311d7"
        }
    }

---------------------
Updating a credential
---------------------

* Method: PATCH
* Path: /credentials/{credential_id}
* Header: X-Auth-Token
* Body::

    {
        "credential": {
        "blob": "{\"access\":\"181920\",\"secrete\":\"secretKey\"}",
        "project_id": "731fc6f265cd486d900f16e84c5cb594",
        "type": "ec2",
        "user_id": "bb5476fd12884539b41d5a88f838d773"
        }
    }

* Request parameters

+-----------------------+-------------+------------+------------------------------------------------------------------+
| Parameter             |   Style     |   Type     |  Description                                                     |
+=======================+=============+============+==================================================================+
| credential            |   plain     | xsd:dict   |  A credential object.                                            |
+-----------------------+-------------+------------+------------------------------------------------------------------+
| user_id (optional)    |   plain     | xsd:string |  The ID of the user who owns the credential.                     |
+-----------------------+-------------+------------+------------------------------------------------------------------+
| blob (optional)       |   plain     | xsd:dict   |  The credential itself, as a serialized blob.                    |
+-----------------------+-------------+------------+------------------------------------------------------------------+
| project_id (optional) |   plain     | csapi:UUID |  The UUID for the associated project.                            |
+-----------------------+-------------+------------+------------------------------------------------------------------+
| type (optional)       |   plain     | xsd:string | | The credential type, such as ec2 or cert. The implementation   |
|                       |             |            | | determines the list of supported types.                        |
+-----------------------+-------------+------------+------------------------------------------------------------------+


* Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
* Response Body::

    {
        "credential": {
        "user_id": "bb5476fd12884539b41d5a88f838d773",
        "links": {
            "self": "http://localhost:5000/v3/credentials/207e9b76935efc03804d3dd6ab52d22e9b22a0711e4ada4ff8b76165a07311d7"
        },
        "blob": "{\"access\":\"181920\",\"secrete\":\"secretKey\"}",
        "project_id": "731fc6f265cd486d900f16e84c5cb594",
        "type": "ec2",
        "id": "207e9b76935efc03804d3dd6ab52d22e9b22a0711e4ada4ff8b76165a07311d7"
        }
    }

Response parameter already explained above.

---------------------
Deleting a credential
---------------------

  * Method: DELETE
  * Path: /credentials/{credential_id}
  * Header: X-Auth-Token
  * Body: N/A
  * Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
  * Response Body: N/A


New Token Validation
====================

----------------
Token validation
----------------
  * Method: GET
  * Path:  /token-auth?action={action_name}&resource={resource_id}
  * Header: X-Auth-Token, X-Subject-Token(optional)
  * Body: N/A
  * Response Codes: 200, 400, 401, 403, 405, 413, 503, 404
  * Response Body::

    {
        "token": {
        "methods": [
            "token"
        ],
        "expires_at": "2015-11-05T22:00:11.000000Z",
        "extras": {},
        "user": {
            "domain": {
                "id": "default",
                "name": "Default"
            },
            "id": "10a2e6e717a245d9acad3e5f97aeca3d",
            "name": "admin"
        },
        "audit_ids": [
            "mAjXQhiYRyKwkB4qygdLVg"
        ],
        "issued_at": "2015-11-05T21:00:33.819948Z"
        }
    }

  * Response parameters

+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   Parameter               |     Style       |   Type       | Description                                            |
+===========================+=================+==============+========================================================+
|   X-Auth-Token            |     header      | xsd:string   | A valid authentication token for an administrative user|
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   X-Subject-Token         |     header      | xsd:string   | | The authentication token. An authentication response |
|                           |                 |              | | returns the token ID in this header rather than in   |
|                           |                 |              | | the response body.                                   |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   token                   |     plain       | xsd:string   | A token object.                                        |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   expires_at              |     plain       | xsd:dateTime | The date and time when the token expires.              |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   issued_at               |     plain       | xsd:dateTime | The date and time when the token issued.               |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   methods                 |     plain       | xsd:string   | | The authentication method, which is password, token, |
|                           |                 |              | | or both methods.                                     |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   user                    |     plain       |  xsd:dict    | A user object.                                         |
+---------------------------+-----------------+--------------+--------------------------------------------------------+
|   domain (Optional)       |     plain       | xsd:string   | Specify either id or name to uniquely identify domain. |
+---------------------------+-----------------+--------------+--------------------------------------------------------+


--------------------
Signature Validation
--------------------

  * Method: POST
  * Path: /sign-auth?action={action_name}&resource={resource_id}
  * Body:

      {
          "credentials": {
              "access": "",
              "signature": "",
              "token": ""
          }
      }
  * Response Codes: 200, 400, 401, 403, 413, 503, 404
  * Response Body:

                 {
                     "token": {
                         "token_id": "0fecd40c53e04a9f96a5e2cf68ea125f"
                         "expires_at": "2013-02-27T18:30:59.999999Z",
                         "issued_at": "2013-02-27T16:30:59.999999Z",
                         "methods": [
                             "password"
                         ],
                         "user": {
                             "domain": {
                                 "id": "1789d1",
                                 "links": {
                                     "self": "http://identity:35357/v3/domains/1789d1"
                                 },
                                 "name": "example.com"
                             },
                             "id": "0ca8f6",
                             "links": {
                                 "self": "http://identity:35357/v3/users/0ca8f6"
                             },
                             "name": "Joe"
                         }
                     }
                 }