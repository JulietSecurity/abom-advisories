# ABOM Advisory format

## Format

The ABOM Advisory file is a JSON document.
Each entry contains the elements in the table below.
All keys are required unless noted.

| Key | Type | Description
| --- | ---- | -----------
| id | string | Unique ID in the form "ABOM-_YYYY_-_NNNN_"
| title | string | Brief title for the advisory
| cve | string | (optional) Associated CVE entry
| cvss | float64 | (optional) CVSS score for the CVE
| published | string | Date advisory was published to ABOM Advisories in the form "_YYYY_-_MM_-_DD_"
| updated |  string | Date advisory was updated in ABOM Advisories in the form "_YYYY_-_MM_-_DD_"
| status | string | "active" or "withdrawn"
| description | string | Description of the method and consequences of the incident prompting the advisory
| references | []string | (optional) List of URLs with more information
| affected_actions | []AffectedAction | One or more Actions affected (see [AffectedAction entries](#affectedaction-entries))
| indicators | Indicators | (see [Indicators entries](#indicators-entries))
| recommended_actions | []string  | (optional) Steps to mitigate or remediate

### AffectedAction entries

`AffectedAction` entries describe the GitHub Actions associated with an advisory.

| Key | Type | Description
| --- | ---- | -----------
| uses | string | The name of the action in the form "_org_/_repo_"
| affected_refs | AffectedRefs | (see [AffectedRefs entries](#affectedrefs-entries))
| affected_period.from | string | Date of first-known impact in ISO 8601 format
| affected_period.to | string | Date of final remediation in ISO 8601 format
| tool_names | []string | (optional) List of command line tool names associated with the action

#### AffectedRefs entries

AffectedRefs describes which refs of an action are affected.
All keys are optional.

| Key | Type | Description
| --- | ---- | -----------
| tags | []string | Known unsafe Git tags
| tags_range | string | Range(s) of affected Git tags
| safe_tags | []string | Git tags known to be safe
| safe_shas | []string | Git commit hashes known to be safe

### Indicators entries

Indicators contains indicators of compromise for an advisory.
All keys are optional.

| Key | Type | Description
| --- | ---- | -----------
| docker_images | []string | List of container images associated with an advisory
| repos_to_check | []string | Repositories whose presence indicates compromise
| notes | string | Additional notes on the indicators

## FAQ

* What year should the `id` use?
    * Use the year that the advisory is added for `id`, not the year that the compromise was introduced or discovered.
* What should `update` be for a new entry?
    * Use the value of `published` for new entries.
* What do the values of `status` mean?
    * `active`: The advisory is valid and will be used in checks by [abom](https://github.com/JulietSecurity/abom) and other tools that consume this database.
    * `withdrawn`: The advisory is not used in checks by tools that consume this database.
