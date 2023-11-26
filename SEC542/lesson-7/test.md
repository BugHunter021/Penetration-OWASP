# Enumerate Application Admin

**Enumerate Infrastructure and Application Admin Interfaces**

> [Detail on OWASP website](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces)
> 

<aside>
💡 Administrator interfaces may be present in the application or on the application server to allow certain users to perform privileged activities on the site. Tests should be undertaken to reveal if and how this privileged functionality can be accessed by an unauthorized or standard user.
An application may require an administrator interface to enable a privileged user to access functionality that may make changes to how the site functions. Such changes may include:

- user account provisioning
- site design and layout
- data manipulation
- configuration changes
</aside>

### Tools need to this test:

- Browser
- Nikto
- DirSearch
- ffuf
- DirBuster
- Burpsuite/ZAP

### CWE use for this test:

- CWE-284
- CWE-419

---

---

### **Test Objectives**

- Identify hidden administrator interfaces and functionality.
- Directory and file enumeration, comments and links in source (/admin, /administrator, /backoffice, /backend, etc), alternative server port (Tomcat/8080)

---

## How to Test

The following section describes vectors that may be used to test for 
the presence of administrative interfaces. These techniques may also be 
used to test for related issues including privilege escalation, and are 
described elsewhere in this guide (for example, [Testing for bypassing authorization schema](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema) and [Testing for Insecure Direct Object References](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References)) in greater detail.

- Directory and file enumeration
- Comments and links in source code
- Publicly available information
- Alternative server port : Administration interfaces may be seen on a different port on the host than the main application. For example, Apache Tomcat’s Administration interface can often be seen on port 8080.
- Parameter tampering: A GET or POST parameter, or a cookie may be required to enable the administrator functionality. Clues to this include the presence of
hidden fields such as:

```bash
<input type="hidden" name="admin" value="no">
```

or in a cookie:

```bash
Cookie: session_cookie; useradmin=0
```

Each web framework may have its own default admin pages or paths, as in the following examples:

**PHP:**

```bash
/phpinfo
/phpmyadmin/
/phpMyAdmin/
/mysqladmin/
/MySQLadmin
/MySQLAdmin
/login.php
/logon.php
/xmlrpc.php
/dbadmin
```

**WordPress:**

```bash
wp-admin/
wp-admin/about.php
wp-admin/admin-ajax.php
wp-admin/admin-db.php
wp-admin/admin-footer.php
wp-admin/admin-functions.php
wp-admin/admin-header.php
```

**Joomla:**

```bash
/administrator/index.php
/administrator/index.php?option=com_login
/administrator/index.php?option=com_content
/administrator/index.php?option=com_users
/administrator/index.php?option=com_menus
/administrator/index.php?option=com_installer
/administrator/index.php?option=com_config
```

**Tomcat:**

```bash
/manager/html
/host-manager/html
/manager/text
/tomcat-users.xml
```

**Apache:**

```bash
/index.html
/httpd.conf
/apache2.conf
/server-status
```

**Ngnix:**

```bash
/index.html
/index.htm
/index.php
/nginx_status
/index.php
/nginx.conf
/html/error
```

- **References**:
    - [FuzzDB can be used to do brute force browsing admin login path](https://github.com/fuzzdb-project/fuzzdb/blob/master/discovery/predictable-filepaths/login-file-locations/Logins.txt)
