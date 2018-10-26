In FIWARE we offer some services and tools to allow you to manage authentication
and authorization in your applications and backend services. If you want to
manage identity in your application without developing your own mechanisms, you
can offer your users the possibility to log in to your app using their FIWARE
Accounts.

This is possible thanks to the [OAuth2](http://oauth.net/2/) protocol and
Keyrock, the Identity Manager component of FIWARE. In the same way that you
usually log in to some services using your Twitter or Facebook account, your
users will use their FIWARE accounts to access your service. But this is only
the first step, because you can also secure your backends using FIWARE Account.
If your service or GE has a REST API that can be accessed from Internet,
probably you want to manage the access to the resources. For instance, you can
allow the access only to the users that have a FIWARE account.

Furthermore, if you want to differentiate user permissions based on roles,
thanks to the Access Control component of FIWARE, you can define the various
roles, possibly with role hierarchies, and define specific permissions for each
role. This way you can determine which type of users can access which specific
resource in your backends.  You can also manage basic role permissions via the
Identity Management user interface and they will be pushed automatically to the
Access Control component. For more advanced role permissions, for instance
combining user roles with other user attributes, resource attributes (besides
the URL), or environment attributes (e.g. time constraints), you would use the
Access Control interface directly. In any case, you can assign roles to your
users with the Identity Management interface.

So the two components that you have to know in order to manage access control in
your services are:

**[Keyrock GE](http://catalogue.fiware.org/enablers/identity-management-keyrock)**

Identity Management covers a number of aspects involving users' access to
networks, services and applications, including secure and private authentication
from users to devices, networks and services, authorization & trust management,
user profile management, privacy-preserving disposition of personal data, Single
Sign-On (SSO) to service domains and Identity Federation towards applications.
The Identity Manager is the central component that provides a bridge between IdM
systems at connectivity-level and application-level.

Thanks to this GE you can create users and organizations in FIWARE. You can also
register new applications in order to use the OAuth2 protocol described above
and to manage roles and permissions in them.

[**Access Control GE**](http://catalogue.fiware.org/enablers/access-control-tha-implementation)

The Access Control GE provides XACML-standard-compliant authorization services.
The OASIS XACML (eXtensible Access Control Markup Language) standard [1] is the
only existing standard access control language written in XML as of writing.
According to the OASIS XACML FAQ, “it provides an extremely flexible language
for expressing access control that can use virtually any sort of information as
the basis for decisions. It is a functional superset of other familiar access
control schemes, such as permissions, ACLs, RBAC, etc. It is particularly
designed to support large-scale environments where resources are distributed and
policy administration is Federated.”

In short, this component proves to be useful when dynamic attribute-based access
control matters, i.e. when authorization depends on attributes in the context of
the access request, such as the requester (subject)'s attributes, the requested
action, the requested resource, and possibly environment attributes. The
component may be used in various ways to control access to critical resources
such as other FIWARE GEs or applications deployed by developers from use case
projects. It provides APIs to developers - or security policy authors in
general - for managing ABAC (Attribute-Based Access Control) and/or RBAC
(Role-Based Access Control) policies, and getting authorization decisions for
these policies. It may be combined with the FIWARE Security Proxy playing the
role of PEP (Policy Enforcement Point), in which case the Access Control GE
plays the role of PDP (Policy Decision Point) in XACML terms.

If you are interested in more details about how to handle access control in
FIWARE check out:

-   [How to create your identity in FIWARE](/handling-authorization-and-access-control-to-apis/how-to-create-your-identity-in-fiware/)
-   [How to implement OAuth2 in your applications](/handling-authorization-and-access-control-to-apis/how-to-implement-oauth2-in-your-applications/)
-   [How to send requests to a FIWARE GE](/handling-authorization-and-access-control-to-apis/how-to-send-requests-to-a-fiware-ge/)
-   [How to secure your backend service](/handling-authorization-and-access-control-to-apis/how-to-secure-your-backend-service/)
-   [How to manage Access Control in FIWARE](/handling-authorization-and-access-control-to-apis/how-to-manage-access-control-in-fiware/)

If you want to start experimenting and doing hands-on work, have a look at:

-   [IdM GEri](https://github.com/ging/fiware-idm/)
-   [AuthZForce GEri](https://github.com/authzforce/server)
-   [Pep-proxy GEri](https://github.com/ging/fiware-pep-proxy)
