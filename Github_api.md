
 **Github 的api示例**
|     info    |     url       |
| ------------- |:-------------|
|current_user_url|"https://api.github.com/user"|
|current_user_authorizations_html_url|"https://github.com/settings/connections/applications{/client_id}"|
|authorizations_url|"https://api.github.com/authorizations"|
|code_search_url|"https://api.github.com/search/code?q={query}{&page,per_page,sort,order}"|
|emails_url|"https://api.github.com/user/emails"|
|emojis_url|"https://api.github.com/emojis"|
|events_url|"https://api.github.com/events"|
|feeds_url|"https://api.github.com/feeds"|
|followers_url|"https://api.github.com/user/followers"|
|following_url|"https://api.github.com/user/following{/target}"|
|gists_url|"https://api.github.com/gists{/gist_id}"|
|hub_url|"https://api.github.com/hub"|
|issue_search_url|"https://api.github.com/search/issues?q={query}{&page,per_page,sort,order}"|
|issues_url|"https://api.github.com/issues"|
|keys_url|"https://api.github.com/user/keys"|
|notifications_url|"https://api.github.com/notifications"|
|organization_repositories_url|"https://api.github.com/orgs/{org}/repos{?type,page,per_page,sort}"|
|organization_url|"https://api.github.com/orgs/{org}"|
|public_gists_url|"https://api.github.com/gists/public"|
|rate_limit_url|"https://api.github.com/rate_limit"|
|repository_url|"https://api.github.com/repos/{owner}/{repo}"|
|repository_search_url|"https://api.github.com/search/repositories?q={query}{&page,per_page,sort,order}"|
|current_user_repositories_url|"https://api.github.com/user/repos{?type,page,per_page,sort}"|
|starred_url|"https://api.github.com/user/starred{/owner}{/repo}"|
|starred_gists_url|"https://api.github.com/gists/starred"|
|team_url|"https://api.github.com/teams"|
|user_url|"https://api.github.com/users/{user}"|
|user_organizations_url|"https://api.github.com/user/orgs"|
|user_repositories_url|"https://api.github.com/users/{user}/repos{?type,page,per_page,sort}"|
|user_search_url|"https://api.github.com/search/users?q={query}{&page,per_page,sort,order}"|

|     Response Headers    |            |
| -------------: |:-------------|
|Access-Control-Allow-Origin|*|
|Access-Control-Expose-Headers|ETag, Link, X-GitHub-OTP, X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes, X-Poll-Interval|
|Cache-Control|public, max-age=60, s-maxage=60|
|Content-Encoding|gzip|
|Content-Type|application/json; charset=utf-8|
|Date|Sun, 06 Nov 2016 02:44:34 GMT|
|Etag|W/"aa8dddb16e1a555eaad6a8fea13effb1"|
|Server|GitHub.com|
|Strict-Transport-Security|max-age=31536000; includeSubdomains; preload|
|Transfer-Encoding|chunked|
|Vary|Accept, Accept-Encoding|
|X-Content-Type-Options|nosniff|
|X-GitHub-Media-Type|github.v3|
|X-GitHub-Request-Id|AC6A5816:62AB:15626B0:581E9911|
|X-RateLimit-Limit|60|
|X-RateLimit-Remaining|59|
|X-RateLimit-Reset|1478403874|
|X-Served-By|2c18a09f3ac5e4dd1e004af7c5a94769|
|X-XSS-Protection|1; mode=block|
|content-security-policy|default-src 'none'|
|status|200 OK|
|x-frame-options|deny|

|     Request Headers    |            |
| -------------: |:-------------|
|Accept|text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8|
|Accept-Encoding|gzip, deflate, br|
|Accept-Language|en-US,en;q=0.5|
|Connection|keep-alive|
|Cookie|logged_in=yes; _ga=GA1.2.675931747.1477132178; _octo=GH1.1.2109163812.1477132178; dotcom_user=talliujobs|
|Host|api.github.com|
|Referer|http://zhangmhao.github.io/2014/09/12/Restful-API/|
|Upgrade-Insecure-Requests|1|
|User-Agent|Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:51.0) Gecko/20100101 Firefox/51.0|
