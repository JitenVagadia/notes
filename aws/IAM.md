# Identity Access Management
* global, not region specific
* root account created by default, all powerful
* users are people, group can have people but not groups, people can belong to multiple groups
* roles - identities meant to be assumed temporarily
  * Assign permissions to AWS services with IAM Roles

* policies - defined permissions, attached to users/roles/groups
    * JSON, versioned, list of statements, statement has effect (allow,deny), action, resource, principal (to which this policy applies), condition (optional)
    * least privilege
    * explicit deny wins
      * user in 2 groups, 1 allows something, 1 denies that thing -> user will be denied that thing
    * can be identity based (attached to user/group/role) / resource based (attached to resource itself)
* IAM policy simulator to test policies
* IAM Creds report - account level, list of all users and their creds (how do they access, not what they can access)
* IAM Access Advisor - user level, all their permissions, last access of them
* Shared respo model for IAM
  * AWS: infra, config & vuln analysis, compliance validation
  * Us: management & monitoring, MFA enablement, key rotation