### Google Dorks Lab Report

## Exercise 1: Basic Google Dork Operators

### a) Finding pages on a specific domain

To return pages located on the domain [www.tek-up.de](http://www.tek-up.de) using the "site" operator:

```bash
site:tek-up.de
```

This query returns all pages indexed by Google that are hosted on the tek-up.de domain.

### b) Finding PDF files on a domain

To return PDF files located on the domain:

```bash
site:tek-up.de filetype:pdf
```

This combines the site operator with the filetype operator to specifically target PDF documents.

### c) Checking for Excel files

To verify the existence of Excel files on the domain:

```bash
site:tek-up.de filetype:xlsx OR site:tek-up.de filetype:xls
```

This searches for both modern Excel files (.xlsx) and legacy Excel files (.xls).

### d) Finding subdomains

To return all subdomains of tek-up.de:

```bash
site:*.tek-up.de -site:www.tek-up.de
```

This finds all subdomains while excluding the main www subdomain.

## Exercise 2: Advanced Google Dork Techniques

### a) Difference between "inurl" and "allinurl"

- **inurl:** Searches for pages that contain the specified term in the URL. Other terms can appear anywhere in the page.
Example: `inurl:admin site:tek-up.de` finds pages with "admin" in the URL on tek-up.de domain.
- **allinurl:** Requires that ALL terms specified must appear in the URL.
Example: `allinurl:admin login` finds pages where both "admin" and "login" appear in the URL.


### b) Finding websites linking to tek-up.de

```bash
link:tek-up.de
```

Note: The "link:" operator has been deprecated by Google, so a better alternative is:

```bash
"tek-up.de" -site:tek-up.de
```

### c) Finding emails with passwords on Pastebin

```bash
site:pastebin.com intext:"email" intext:"password" OR site:pastebin.com intext:"@gmail.com" intext:"password"
```

This query searches for pages on Pastebin containing both email addresses and passwords.

### d) Finding WordPress login pages

```bash
intitle:"Log In" inurl:wp-login.php
```

This query targets the standard WordPress login page by looking for the login title and the specific login URL path.

### e) Finding cached versions of websites

To view the cached version of a website, you can use:

```bash
cache:website.com
```

For example: `cache:tek-up.de`

### f) Role of the "related" operator

The query `related:www.mosaiquefm.net` returns websites that Google considers similar to mosaiquefm.net. This operator helps find sites that are topically related or in the same category (in this case, likely other Tunisian news/media websites).

### g) Finding SMTP servers

```bash
intitle:"Welcome to" intext:"SMTP" intext:"port"
```

This query looks for pages that might expose SMTP server information, often found in welcome or configuration pages.

### h) Analysis of "intitle: 'live view' intitle:axis" query

This query targets Axis network cameras with exposed web interfaces. The results show:

1. **Security Implications**: These are live camera feeds that are publicly accessible without proper authentication.
2. **Types of Exposures**: The results show various locations including:
3. Office spaces
4. Public areas
5. Building entrances
6. Parking lots
7. **Technical Details**:
8. Most cameras use default configurations
9. Many show the Axis camera model in the title
10. Some include IP addresses in the URL
11. **Privacy Concerns**: These exposed cameras could potentially violate privacy laws depending on their location and what they're recording.
12. **Recommendations**:
13. Camera administrators should implement proper authentication
14. Restrict access through IP filtering
15. Use VPNs for remote viewing
16. Change default credentials
17. Keep firmware updated