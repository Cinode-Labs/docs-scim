# Cinode SCIM API

Provision and manage user accounts and groups from existing identity management system or Identity Provider (IdP) with our SCIM API. SCIM is used by variety of IdPs to manage users across applications. It's also possible to write your own apps using the SCIM API to programatically manage your users, teams and roles in Cinode. Cinode will be reffered to as the Service Provider (SP).

Our SCIM implementation targets version 2.0 of the protocol. More information can be found in the [SCIM 2.0 specification](http://www.simplecloud.info/).

## SCIM API Endpoints
The SCIM API is RESTful and the endpoints are different than other Cinode API endpoints.
All access to our SCIM API must be within a tenant, accept for SCIM protocol specific endpoints.

The base URL for tenant endpoints are `https://api.cinode.com/scim/tenants/{tenantId}/v2` and for non tenant calls and SCIM discovery endpoints are `https://api.cinode.com/scim/v2`. TenantId can be found under `Administration -> Tokens` in Cinode after a token for SCIM is created.

## SCIM API Authentication token
In the application navigate to `Administration -> Tokens` and create a token for Audience `https://api.cinode.app/scim/v2`. Set a expire date that your are comfortable with. Don't forget to store the token in a safe place. It can't be retrieved again.

> Cinode SCIM API is a feature addon, please contanct Cinode Customer Support for further assistance.

## SCIM Discovery endpoints

### GET /ServiceProviderConfigs
Returns Cinode Service provider config for SCIM, includes what operations that are supported.

> Requires authentication
#### Example
```
GET /scim/v2/ServiceProviderConfigs HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

### GET /Schemas 
Returns one or more supported schemas and their attributes

> Requires authentication
#### Example
```
GET /scim/v2/Schemas HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

#### GET /ResourceTypes
Returns supported resource types

> Requires authentication
#### Example
```
GET /scim/v2/ResourceTypes HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

### Users

#### Attributes
| Cinode attribute | SCIM attribute | Attribute type | Required | Read only|
|:-------------|:-------------|:-------------|:-------------:|:---------:|
| Email         | `userName` | Singular | Yes | Yes |
| FirstName | `name.givenName` | Singular | Yes | No |
| LastName | `name.familyName` | Singular | Yes | No |
| ObjectIdentifier | `externalId` | Singular | No | Yes |
| CompanyUserId | `id` | Singular | Yes | Yes | Yes |

#### GET /Users
Returns a paginated list of users. Use the `startIndex` and `count` (max 1000) query parameters to paginate long lists of users.
> Requires authentication
##### Example
```
GET /scim/v2/Users?startIndex=2&count=10 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
GET /scim/tenants/1/v2/Users?startIndex=2&count=10 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
#### GET /Users/{id}

> Requires authentication
##### Example
```
GET /scim/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
GET /scim/tenants/1/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

#### POST /Users



> Requires authentication
##### Example
```
POST /scim/v2/Users HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
POST /scim/tenants/1/v2/Users HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
#### PATCH /Users/{id}

> Requires authentication
##### Example
```
PATCH /scim/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
PATCH /scim/tenants/1/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

#### PUT /Users/{id}

> Requires authentication
##### Example
```
PUT /scim/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
PUT /scim/tenants/1/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

#### DELETE /Users/{id}
The user is not deleted, the user's status will be set to `Disconnected`. The user will be returned with the active attribute set to `false` for further requests.

> Requires authentication
##### Example
```
DELETE /scim/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
DELETE /scim/tenants/1/v2/Users/1 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```


### Groups
Groups can be linked to `Team` and `Role` in Cinode. For example when a new group is provisioned to Cinode and new Team will be created with that name and a link will be set up. If you provision members to that group they will become members in that Team. In Cinode you can create a link between a Role and a group, this will add that role to that user when the user is provisionend in that group.

#### Team
If you already have existing teams in Cinode and want them to be provisioned you can go to `Administration -> User provisioning` and set up a link. It's important that the `DisplayName` property is spelled exactly as in the IdP. It's also important that the ObjectId is the same as in the IdP. If  a group can't be found in Cinode a new Team will be created. If you update the name of a group in the IdP this will not update the name of the Team in Cinode.

#### Role
A link between a `Group` and a `Role` must be created in Cinode. Navigate to `Administration -> User provisioning` to create the link. It's important that the name and ObjectId is spelled exactly as in the IdP. If it's not set up properly a new Team with that name will be created.

#### Attributes
| Cinode attribute | SCIM attribute | Attribute type | Required | Read only|
|:-------------|:-------------|:-------------|:-------------:|:---------:|
| DisplayName |        `displayName`   | Singular  | Yes | No |
| ObjectIdentifier | `id`    | Singular | Yes | Yes |
| Members | `members` | Multi-values | Yes | No |

#### GET /Groups
Returns a paginated list of groups. Use the `startIndex` and `count` (max 1000) query parameters to paginate long lists of groups.
> Requires authentication
##### Example
```
GET /scim/v2/Groups?startIndex=2&count=10 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
GET /scim/tenants/1/v2/Groups?startIndex=2&count=10 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
#### GET /Groups/{id}
Returns a single group resource.

> Requires authentication
##### Example
```
GET /scim/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
GET /scim/tenants/1/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

#### POST /Groups
Creates a new Team in Cinode and creates a Link between them.

> Requires authentication
##### Example
```
POST /scim/v2/Groups HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
POST /scim/tenants/1/v2/Groups HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

Body
```
{
	"schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
	"id": "external_id",
	"displayName": "Group name",
	"members": [],
	"meta": {
		"resourceType": "Group"
	}
}
```

#### PATCH /Groups/{id}
Updates an existing group resource, allowing individual (or groups of) users to be added or removed from the group with a single operation.

Setting the operation attribute of a member object to `Add` will add members.
Setting the operation attribute of a member object to `Remove` will remove members.

> Requires authentication
##### Example
```
PATCH /scim/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
PATCH /scim/tenants/1/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

Body
```
{
    "schemas": [
        "urn:ietf:params:scim:api:messages: 2.0:PatchOp"
    ],
    "Operations": [
        {
            "op": "Replace",
            "path": "displayName",
            "value": "New group name"
        }
    ]
}
```

#### PUT /Groups/{id}
> Method is currently not supported.

#### DELETE /Groups/{id}
This will delete the link between an external group and a Cinode Team. The Team and it's members will remain.

> Requires authentication
##### Example
```
DELETE /scim/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```
For tenants
```
DELETE /scim/tenants/1/v2/Groups/3 HTTP/1.1
Host: api.cinode.app
Accept: application/json
Authorization: Bearer xxxxxxxxx
```

## Filter
For methods that returns a list can be filtered.
| Operator      | Description| Behavior|
| ------------- |:-------------|:-------------|
| eq            | `equal`      | The attribute and operator values must be identical for a match |
| ne            | `not equal`| The attribute and operator values are not identical |
| co            | `contains` | The entire operator value must be a substring of the attribute value for a match|
| lt            | `less than` | If the attribute value is less than the operator value, there is a match|
| le            | `less than or equal to` | If the attribute value is less than or equal to the operator value, there is a match |
| gt            | `greater than` | If the attribute value is greater than the operator value, there is a match |
| ge            | `greater than or equal to` | If the attribute value is greater than or equal to the operator value, there is match |

```
filter=userName eq firstnanme.lastname@example.net
```


## Errors 
Sometimes errors happen and we will try to respond with a descriptive error message that will help you figure out what went wrong and how to fix it.

| Error      | Description|
| ------------- |:-------------|
| "name.givenName":["Value is missing"] | Parameter GivenName is not provided     |
| "name.familyName":["Value is missing"]    | Parameter FamilyName is not prvovided |


## Azure Active Directory
You can use Azure Active Directory (AAD) as an Identity Provider (IdP). You need to add `Cinode` as an Gallery application and configure it with a `Tenant URL` and a `Secret token`, theese are described in a separate section in this document. You need to add some mappings for `Users` and `Groups`, described further down in this section. More information about `SCIM user provisioning with Azure Active Directory` can be found [here](https://docs.microsoft.com/en-gb/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)

### Mappings
You need to set up mappings in the Gallery app for it to work with our SCIM API Endpoints.

#### User

| Azure Active Directory Attribute | SCIM Attribute |  Matching precedence|
| ------------- |:-------------|:-------------|
| userPrincipalName | `userName` | 1|
| Switch([IsSoftDeleted], , "False", "True", "True", "False") | `active`||
| displayName | `displayName` | | 
| givenName | `name.givenName`| |
| surname | `name.familyName` | |
| Join(" ", [givenName], [surname]) | `name.formatted` | |
| objectId | `externalId` | |

#### Group

| Azure Active Directory Attribute | SCIM Attribute |  Matching precedence|
| ------------- |:-------------|:-------------|
| displayName | `displayName` | | 
| objectId | `id` || 
| members | `members` ||

## Limitations
* Users can not be permanently deleted from Cinode, they can only be disconnected. If you want to delete a user permanently you have to login to Cinode as an administrator and delete it manually.
* Teams can not be delete from Cinode via the SCIM API - only the link between the external group and the Team will be removed. If you want to delete a Team permanently you have to login to Cinode as an administrator and delete or move it manually.
* Attempts to provision a user with a duplicate email address (even if the existing user has been previously disconnected in Cinode) will fail. The existing user email address must be updated manually in Cinode to free up the email to be re-provisioned.