LDAP CAS Overlay
============================

Practicing configuring CAS maven war overlay which is enabled with LDAP authenticator.
The original template can be found in [Jasig/cas-overlay-template](https://github.com/Jasig/cas-overlay-template)

The configuration is referencing [LDAP Authentication](https://github.com/Jasig/cas/blob/4.2.x/cas-server-documentation/installation/LDAP-Authentication.md)
from CAS repo. It's currently configured to use LDAP direct bind. The `deployerConfigContext.xml`
is modified from the default config, obtained by building the
[Jasig/cas-overlay-template](https://github.com/Jasig/cas-overlay-template) and copying from the
generated `target/cas/WEB-INF/deployerConfigContext.xml`.

The documentation for setup and configure OpenLdap server can be found in 
[this link](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-openldap-and-phpldapadmin-on-an-ubuntu-14-04-server).
Following the instructions will set up the user DN like this:
```
cn={uid},ou=users,dc=tinymake,dc=com
``` 
The above DN is currently hardcoded in `deployerConfigContext.xml` 
due to the format field is not translating the config vars. This should be fixed and moved into
`etc/cas/cas.properties`.

## Versions
```xml
<cas.version>4.2.1</cas.version>
```

## Requirements
* JDK 1.7+

## Configuration

The `etc` directory contains the configuration files that need to be copied to `/etc/cas`.

Current files are:

* `etc/cas/cas.properties`
* `src/main/webapp/WEB-INF/deployerConfigContext.xml`

## Build

```bash
mvnw clean package
```

or

```bash
mvnw.bat clean package
```

## Deployment

### Embedded Jetty

* Create a Java keystore at `/etc/cas/jetty/thekeystore` with the password `changeit`.
* Import your CAS server certificate inside this keystore.

```bash
mvnw jetty:run-forked
```

CAS will be available at:

* `http://cas.server.name:8080/cas`
* `https://cas.server.name:8443/cas`

### External
Deploy resultant `target/cas.war` to a Servlet container of choice.
