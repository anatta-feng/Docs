> https://developer.atlassian.com/cloud/jira/platform/rest/v2/#api-api-2-issue-properties-propertyKey-delete

# Get issue

GET /rest/api/2/issue/{issueIdOrKey}

Returns the details for an issue.

The issue is identified by its ID or key, however, if the identifier doesn't match an issue, a case-insensitive search and check for moved issues is performed. If a matching issue is found its details are returned, a 302 or other redirect is **not** returned. The issue key returned in the response is the key of the issue found.

# 过滤指定人未解决的项目列表

https://jira.cvte.com/rest/api/2/search?jql=assignee=fengxuchao+AND+resolution=Unresolved+ORDER+BY+updated+ASC

