### 🔍 Complete Guide to Google & GitHub Dorks Operators

## 🌐 Google Dorks Operators

```mermaid
graph TD;
    A["Google Dorks Operators"]-->B["Content Operators"]
    A-->C["URL Operators"]
    A-->D["File Operators"]
    A-->E["Technical Operators"]
    A-->F["Metadata Operators"]
```

### Basic Search Operators

| Operator | Description        | Example                               |
| -------- | ------------------ | ------------------------------------- |
| **" "**  | Exact match search | `"confidential employee information"` |
| **OR**   | Match either term  | `password OR credentials`             |
| **AND**  | Match both terms   | `admin AND password`                  |
| **-**    | Exclude term       | `security -jobs`                      |
| **( )**  | Group terms        | `(username OR user) AND password`     |
| *****    | Wildcard           | `admin*.php`                          |

### Content Operators

| Operator         | Description               | Example                              |
| ---------------- | ------------------------- | ------------------------------------ |
| **intext:**      | Text appears in body      | `intext:"internal use only"`         |
| **intitle:**     | Text appears in title     | `intitle:"index of"`                 |
| **allintitle:**  | All terms appear in title | `allintitle: admin login`            |
| **inanchor:**    | Text in link anchor       | `inanchor:"never shared publicly"`   |
| **allinanchor:** | All terms in link anchor  | `allinanchor: confidential login`    |
| **allintext:**   | All terms in page text    | `allintext: username password email` |
### URL & Domain Operators

| Operator      | Description              | Example                      |
| ------------- | ------------------------ | ---------------------------- |
| **site:**     | Limit to specific domain | `site:example.com password`  |
| **-site:**    | Exclude specific domain  | `-site:example.com password` |
| **inurl:**    | Text appears in URL      | `inurl:admin`                |
| **allinurl:** | All terms appear in URL  | `allinurl: admin config`     |
| **link:**     | Pages that link to URL   | `link:example.com`           |
| **domain:**   | Pages in specific domain | `domain:gov confidential`    |
### File Operators

| Operator      | Description                       | Example                       |
| ------------- | --------------------------------- | ----------------------------- |
| **filetype:** | Specific file types               | `filetype:pdf "confidential"` |
| **ext:**      | File extension (same as filetype) | `ext:sql password`            |
| **cache:**    | Google's cached version           | `cache:example.com/admin`     |
### Technical Operators

| Operator     | Description                 | Example                |
| ------------ | --------------------------- | ---------------------- |
| **info:**    | Information about a page    | `info:example.com`     |
| **related:** | Related websites            | `related:example.com`  |
| **ip:**      | Sites hosted on specific IP | `ip:8.8.8.8`           |
| **define:**  | Definition of a term        | `define:vulnerability` |
| **stocks:**  | Stock information           | `stocks:GOOG`          |
| **map:**     | Map of a location           | `map:silicon valley`   |
### Date & Number Operators

| Operator       | Description                        | Example                                     |
| -------------- | ---------------------------------- | ------------------------------------------- |
| **before:**    | Pages indexed before date          | `before:2020-06-01 confidential`            |
| **after:**     | Pages indexed after date           | `after:2020-06-01 vulnerability`            |
| **daterange:** | Pages from date range              | `daterange:2457388-2457491 security breach` |
| **AROUND(n)**  | Terms within n words of each other | `password AROUND(5) reset`                  |
| **..**         | Number range                       | `salary $50000..$70000`                     |
### Advanced Google Dorks Examples

```bash
intitle:"Index of" intext:credentials.txt
filetype:log username password email
inurl:admin intitle:login
site:example.com filetype:pdf "confidential" "internal only"
intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
```

## 🐙 GitHub Dorks Operators

```mermaid
graph TD;
    A["GitHub Dorks Operators"]-->B["Code Search"]
    A-->C["Repository Search"]
    A-->D["User Search"]
    A-->E["Issue Search"]
    A-->F["Commit Search"]
```

### Code Search Operators

| Operator       | Description                       | Example                         |
| -------------- | --------------------------------- | ------------------------------- |
| **language:**  | Code in specific language         | `language:python password`      |
| **filename:**  | Files with specific name          | `filename:config.yml aws`       |
| **extension:** | Files with specific extension     | `extension:properties password` |
| **path:**      | Files in specific path            | `path:admin password`           |
| **content:**   | Files containing specific content | `content:"API_KEY"`             |
| **size:**      | Files of specific size (in bytes) | `size:>10000 password`          |
### Repository Search Operators

| Operator           | Description                    | Example                        |
| ------------------ | ------------------------------ | ------------------------------ |
| **repo:**          | Specific repository            | `repo:username/repo password`  |
| **in:name**        | Term in repository name        | `in:name password`             |
| **in:description** | Term in repository description | `in:description credentials`   |
| **in:readme**      | Term in README file            | `in:readme API_KEY`            |
| **fork:**          | Include/exclude forks          | `fork:true security`           |
| **mirror:**        | Include/exclude mirrors        | `mirror:true password`         |
| **archived:**      | Include/exclude archived repos | `archived:false password`      |
| **stars:**         | Repos with specific stars      | `stars:>100 auth`              |
| **topics:**        | Repos with specific topics     | `topics:security password`     |
| **created:**       | Created on specific date       | `created:>2020-01-01 password` |
| **pushed:**        | Last pushed on specific date   | `pushed:>2020-01-01 password`  |
### User Search Operators

| Operator       | Description                     | Example                        |
| -------------- | ------------------------------- | ------------------------------ |
| **user:**      | Code from specific user         | `user:username password`       |
| **org:**       | Code from specific organization | `org:microsoft password`       |
| **followers:** | Users with specific followers   | `followers:>1000 security`     |
| **location:**  | Users in specific location      | `location:london security`     |
| **created:**   | User accounts created on date   | `created:>2020-01-01 security` |
### Issue & Pull Request Search Operators

| Operator       | Description                       | Example                      |
| -------------- | --------------------------------- | ---------------------------- |
| **type:**      | Issues or pull requests           | `type:issue password`        |
| **is:open**    | Open issues/PRs                   | `is:open security`           |
| **is:closed**  | Closed issues/PRs                 | `is:closed password`         |
| **is:merged**  | Merged pull requests              | `is:merged password`         |
| **is:pr**      | Pull requests                     | `is:pr password`             |
| **is:issue**   | Issues                            | `is:issue password`          |
| **author:**    | Issues/PRs by author              | `author:username password`   |
| **assignee:**  | Issues/PRs assigned to user       | `assignee:username password` |
| **mentions:**  | Issues/PRs mentioning user        | `mentions:username password` |
| **comments:**  | Issues/PRs with specific comments | `comments:>10 password`      |
| **label:**     | Issues/PRs with specific label    | `label:bug password`         |
| **milestone:** | Issues/PRs in specific milestone  | `milestone:"v1.0" password`  |
### Commit Search Operators

| Operator            | Description                   | Example                               |
| ------------------- | ----------------------------- | ------------------------------------- |
| **author:**         | Commits by author             | `author:username password`            |
| **committer:**      | Commits by committer          | `committer:username password`         |
| **author-date:**    | Commits authored on date      | `author-date:>2020-01-01 password`    |
| **committer-date:** | Commits committed on date     | `committer-date:>2020-01-01 password` |
| **merge:**          | Include/exclude merge commits | `merge:false password`                |
| **hash:**           | Commits with specific hash    | `hash:a7d9b0c password`               |
| **parent:**         | Commits with specific parent  | `parent:a7d9b0c password`             |
| **tree:**           | Commits with specific tree    | `tree:a7d9b0c password`               |
### Advanced GitHub Dorks Examples

```bash
filename:config.yml password
"API_KEY" extension:js
"aws_access_key" language:python
"BEGIN RSA PRIVATE KEY" extension:pem
filename:.env language:text
"JDBC:mysql" extension:properties
"Authorization: Bearer" extension:js
"-----BEGIN OPENSSH PRIVATE KEY-----" extension:txt
"AKIA" extension:json
"password" in:file language:php
```

## 🔐 Security Implications

```mermaid
graph TD;
    A["Security Implications"]-->B["Exposed Credentials"]
    A-->C["Configuration Files"]
    A-->D["API Keys"]
    A-->E["Database Connection Strings"]
    A-->F["Private Keys"]
    A-->G["Internal Documentation"]
```

### Defensive Measures

1. **Regular Audits**: Regularly search for your own domain/organization using these dorks
2. **Git Hooks**: Implement pre-commit hooks to prevent sensitive data from being committed
3. **Secret Scanning**: Use tools like GitGuardian, Gitleaks, or GitHub's secret scanning
4. **Environment Variables**: Use environment variables instead of hardcoding secrets
5. **Gitignore**: Properly configure .gitignore to exclude sensitive files
6. **Rotate Credentials**: Regularly rotate any exposed credentials
7. **Education**: Train developers on secure coding practices