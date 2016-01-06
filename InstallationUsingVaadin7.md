#### Table of contents ####


# Introduction #

These instructions are pretty high level right now, so we'll want to nail it down to a more easily repeatable set of instructions, but we had to start somewhere!

[Yozons offers low-cost hosted commercial solutions](http://www.yozons.com/Open-eSignForms/pricing.jsp) for those who prefer to just get up-and-running. [Lower-priced hosting is available for open source users](http://open.esignforms.com/pricing.jsp). Yozons also offers private web servers for those who want their own domain name (and SSL cert).

# Details #

## Java 8 ##

We start by installing the latest version of [Java 8 SE](http://www.oracle.com/technetwork/java/javase/downloads/) available. It appears that these days you just need the JRE, but you may find the JDK useful if you need to generate keystores, monitor the JVM and the like. The version tested here was 8.0.25. The class files are still compiled targeting Java 7, but our runtime is Java 8.

Because of the sophisticated encryption we use, you'll need the Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files installed in your jre/lib/security folder. Note that encryption entails issues with the U.S. government's misguided and impractical export rules. For Windows, be careful you may have 32-bit and 64-bit JREs installed and you'll want to update both as much depends on which Eclipse uses when it starts Tomcat, of if you run Tomcat directly yourself.  You can download these files from the Java download site (often at the bottom of the page).

## Browsers ##

You can test as you see fit, but we're basically running the latest releases of Firefox, IE, Chrome, Safari and Opera. Earlier versions likely work, but with a Web 2.0 interface, a modern browser is best and fastest.

## Cygwin ##

We use [Cygwin on Windows](http://www.cygwin.com) so we have a Unix/Linux-like environment for our shell scripts. We actually use very few since we only develop on Windows and have never deployed in production on Windows (though it certainly should be doable), but the scripts for DB setup and the like are still driven by shell scripts.  Generally, the standard install works well for our needs.

We generally add 'vim' from the Editors choices if you know how to use 'vi'. We often add links, too, if you ever need a command line web browser for simple testing needs.

We like to modify the shortcut properties Options to increase the command history buffer size from 50 to 500, and include 'QuickEdit mode'. We also modify Layout to use width 220 in both locations and a screen buffer height of 5000 so we can scroll back easily. We also set the Font to 8x12.

Of course, those on Linux/Unix don't need to worry about this.

Install the 'scripts' files in the cywgin/Linux-user account's 'bin' folder. You can also use a 'scripts' folder, but then be sure to add the scripts folder to your PATH.

For Windows, you'll need the .cygwin variants (we delete the file without the extension and then rename the .cygwin variants by removing that extension).  All cygwin files typically need to a `dos2unix *` done on them so they'll run correctly. (See below for Cywin details.)  Note that script files `bashrc` and `profile` should be in the home directory, not 'bin'. You can then update `.bash_profile` to include `. ~/profile` and `.bashrc` to include `. ~/bashrc`.

If you find that scripts won't run on Linux or via Cygwin's shell, you may need to do a `dos2unix *` on the text files and scripts to ensure they are in encoded correctly. You may also want to do a `chmod +x ~/bin/*` if the scripts don't seem to have execute permission.

## Apache Tomcat ##

For testing, we generally run Tomcat from Eclipse, so it's best to download and install Tomcat next. If you plan on running Tomcat as a Windows service, there's a special installer for it. But we just download the ZIP file and really only run it via Eclipse, extracting to C:\. The version tested here was 8.0.15. Tomcat 7 may be used, but it's not recommend if you enable Vaadin server push.

On Tomcat version upgrades, we basically make the following changes on Linux:
  * `bin/startup.sh` - add the following before the last 'exec' line (only include the debug info if you remotely debug your Tomcat, and your memory values will need to match your server):
```
JAVA_OPTS="-server -Xms500m -Xmx500m -Desf.deploybase=$ESF_DEPLOYMENT_BASE"
export JAVA_OPTS
```

  * `webapps` - Move all webapps except perhaps 'manager' up a subdirectory so that they are no longer deployed. Obviously, we put the Open eSignForms webapp here. You can just move them from the prior Tomcat webapps location.
```
  mkdir ../ORIG-webapps
  mv docs examples host-manager ROOT ../ORIG-webapps
```

  * `webapps/manager/WEB-INF/web.xml` - If you use the manager webapp, add the following snippet to the `<security-constraint>` of the 'HTML Manager interface' and 'Status interface' to ensure it only works over SSL-protected connections.
```
    <user-data-constraint>
          <transport-guarantee>CONFIDENTIAL</transport-guarantee>
    </user-data-constraint>
```

  * `conf/tomcat-users.xml` - If you use the manager webapp, set up the username and password to use when remoting manager your webapps:
```
  <role rolename="manager-gui"/>
  <user username="admin" password="PUT-SECURE-PASSWORD-HERE" roles="manager-gui"/>
```

  * `conf\catalina.properties` - Not required, but may speed up startup times, add to the `tomcat.util.scan.StandardJarScanFilter.jarsToSkip` setting:
```
vaadin-push-*.jar,vaadin-server-*.jar,vaadin-sass-compiler-*.jar,vaadin-themes-*.jar,vaadin-slf4j-jdk14-*.jar,vaadin-shared-*.jar,\
json-*.jar,flute-*.jar,guava-*.jar,cssparser-*.jar,streamhtmlparser-jsilver-*.jar,atmosphere-runtime-*.jar,jsoup-*.jar,sac-*.jar,\
ButtonGroup-*.jar,confirmdialog-*.jar,dragdroplayouts-*.jar,animator-*.jar,popupbutton-*.jar,resetbuttonfortextfield-*.jar,tableexport-for-vaadin-*.jar,vaadinckeditor*.jar,\
bcprov-jdk15on-*.jar,bcpkix-jdk15on-*.jar,libphonenumber-*.jar,postgresql-*.jar,poi-*.jar,jempbox-*.jar,pdfbox-*.jar,javax.mail-*.jar,fontbox-*.jar,urlrewritefilter-*.jar
```

  * `conf/server.xml` - You can set up as you need, such as if you have an Apache HTTPD server front-end and use the APR, but for a simple stand-alone Tomcat, we make the following changes. Under the "Catalina" `<Service>` entry (of course, if you have SSL, you want to point to your keystore and its password). We also comment our the AJP Connector on port 8009, but you may need it if you put HTTP in front:
```
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000" acceptorThreadCount="2" URIEncoding="UTF-8" redirectPort="443" />

    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               connectionTimeout="20000" acceptorThreadCount="2" URIEncoding="UTF-8"
               maxThreads="100" SSLEnabled="true" scheme="https" secure="true"
               compression="on" compressableMimeType="text/html,text/xml,text/plain,application/xml,application/json,application/javascript,application/pdf"
               keystoreFile="keys/mykeys" keystorePass="PUT-KEYSTORE-PASSWORD-HERE"
               ciphers="TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,
TLS_RSA_WITH_AES_256_GCM_SHA384,
TLS_DHE_RSA_WITH_AES_256_GCM_SHA384,
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,
TLS_RSA_WITH_AES_128_GCM_SHA256,
TLS_DHE_RSA_WITH_AES_128_GCM_SHA256,
TLS_RSA_WITH_AES_256_CBC_SHA256,
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,
TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,
TLS_RSA_WITH_AES_256_CBC_SHA,
TLS_RSA_WITH_AES_128_CBC_SHA,
SSL_RSA_WITH_3DES_EDE_CBC_SHA,
SSL_RSA_WITH_RC4_128_SHA,
SSL_RSA_WITH_RC4_128_MD5"
               clientAuth="false" sslEnabledProtocols="TLSv1,TLSv1.1,TLSv1.2" />

<!--
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
-->
```

  * `conf/server.xml` - We also change the `<Host>` entry to turn off auto deploy:
```
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="false">
```

  * `conf/web.xml` - Add the following to the `<servlet>` for 'jsp' since we want our JSPs to always be checked to be compiled:
```
        <init-param>
            <param-name>development</param-name>
            <param-value>true</param-value>
        </init-param>
```

  * `keys` - We put our Tomcat SSL certificate and keys in this folder. When upgrading, be sure to copy over this folder that matches the location of specified in the SSL entry's Connector of server.xml attribute 'keystoreFile'.

### Tomcat's Java Keystore for HTTPS/SSL ###
You are free to setup Tomcat's keystore for HTTPS/SSL using any standard scheme.  But here are a few key commands you that will get this going if you are not familiar with the procedure.

  1. Create the Java keystore that Tomcat will use to control SSL. In this case, the keystore file name is 'tomcatkeys'.  You must use the alias name 'tomcat'.:
```
keytool -genkeypair -keyalg RSA -keysize 2048 -alias tomcat -keystore tomcatkeys
```
> > When prompted, you first enter the web site domain name, such as `esign.example.com` along with the other information requested. Choose a good password for the keystore, and then set the keystore file name and password in the Tomcat's `conf/server.xml`.
  1. Generate the CSR to request your SSL certificate from your favorite CA:
```
keytool -certreq -alias tomcat -keyalg RSA -file certreq.csr -keystore tomcatkeys
```
> > You can then submit the contents of `certreq.csr` when ordering your SSL certificate.
  1. Once your CA has issued your certificate, install it into the keystore:
```
keytool -import -alias tomcat -trustcacerts -file YOURCERT.crt -keystore tomcatkeys
```
  1. Restart Tomcat after you have updated Tomcat's `conf/server.xml` for the HTTPS connector's keystore and password.

## Eclipse ##

Install the latest version of http://www.eclipse.org Eclipse IDE for Java EE] developers.  The version tested here is Kepler (4.3) Windows x64. We extract all to c:\Program Files.  We set our workspace to c:\project.

Install the [Apache IvyDE plugin and Vaadin Eclipse plugin](https://vaadin.com/book/vaadin7/-/page/getting-started.eclipse.html) as instructed on their book.

We find that for debugging on your local computer, it's nice to be able to make changes without having the webapp reload, which you can often do because of the hotspot code changes Java allows.  Open the view "Servers" -- if the "tab" is not shown at the bottom of your edit area along with Problems, Console, Error, History, etc., you can use the Window->Show View->Other to pick Server->Servers.  If no servers appear, right-click New->Server; then choose Apache->Tomcat v8.0 Server, click Next and choose the location where you installed Tomcat (i.e. something like C:\apache-tomcat-8.0.15). Double-click your server (named Tomcat v8.0 Server at localhost or whatever version you are using) so you display the Overview and Modules "tabs". Click on Modules, then for your project (/open-eSignFormsVaadin for us -- see below for downloading the project code first) click the Edit button to uncheck the 'Auto reloading enabled'.

Using the same Servers configuration, we make the following changes:

Overview->Ports: Change HTTP/1.1 to use port 80 (unless you prefer 8080 for testing, but you'll need to put them in all your testing URLs).

Overview->Timeouts: Change 'Start' to be 600 to give you more time if you plan on debugging code during application initialization and don't want Eclipse to timeout the startup of your webapp.

Overview->Generation Information->Open launch configuration:
Add to Arguments tab, VM arguments: -Xmx512m -Desf.deploybase=C:\deployments

_Tip: For production deployments, you will not use Eclipse and you won't run Tomcat inside. These are just for software developers, not for those who will run/use the system._

## Building Open eSignForms Using Eclipse ##

Create the Vaadin Eclipse project by downloading the code from CVS, project open-eSignFormsVaadin.

`:pserver:cvsanon@open.esignforms.com:/home/cvs`

If you have edit/commit permission, you'll need a user account and password with SSH access to CVS with something like:

`:extssh:USERNAME@open.esignforms.com:/home/cvs`

It seems from a fresh install that you'll want to right click on the ECLIPSEPROJECTNAME project, click on Ivy->Clean All Caches and then again on Ivy->Resolve to get all of the Vaadin JARs.

(If you plan to do any VaadinCKEditor project work, not necessary for Open eSignForms itself, you'll need to install a subversion plugin -- we use Subversive SVN Team Provider from the Eclipse installation location or Eclipse Marketplace and we selected the SVN Kit 1.7.9. This related project is hosted on Google Code.)

You will need to compile the Vaadin widgets before testing. Vaadin widgets compile to GWT-based javascript and are stored in the `WebContent/VAADIN/widgetsets` folder.

Eclipse should automatically compile all of the regular Java source code into the `WebContent/WEB-INF/classes` folder. If not, right-click on the project in Eclipse and select Properties, then click on the "Java Build Path", click on the Source tab, the "Default output folder" should be something like: `ECLIPSEPROJECTNAME/WebContent/WEB-INF/classes` but this normally should be set automatically if you are using checked-in code or a source code snapshot release.

## Passwords ##

The standard JAR Open eSignForms is delivered with should include the .class and .java source files that go with it.

There are also several `*`.properties files with passwords in them. For testing, these work out of the box, but of course in a production setting, you'll want to copy these individual .properties files into the WEB-INF/classes folder so they can have better values specified.

openesignforms.properties - Sets the two boot passwords to 'test1' and 'test2' respectively.  These values must match the values given to DbSetup when a new system is deployed.

connectionpools.properties - Sets the database user and password for each deployment. The default user and password is the same for both: esignforms

log4j.properties - Be sure to fix up the location where to store the log files for your deployment.

_Remember, in production, you never want to use such passwords._

In Linux, often the IP address 127.0.0.1 is mapped to `localhost.localdomain` as well as `localhost`. On Windows, the former is generally not present, so you either need to change the .properties files that use localhost.localdomain to be just localhost, or you need to update `c:\Windows\System32\drivers\etc\hosts` so that you map the address to both names.

## Database using PostgreSQL ##

Install PostgreSQL 9 per its usual mechanisms. The version tested here was 9.2.4.

It's just a convention, but we use the DB user 'esignforms' for our admin account, and each 'deployment' (a customer system that has its own webapp and database on the server) uses a 5 character unique name that is used as that application's DB user.  The name is also the base name in the deployment folder for its database tables, log files, etc.

For Windows, the installer defaults to the user 'postgres' so you can add our DB user with commands like (for production, use good passwords, of course):
{{{psql -U postgres
CREATE USER esignforms WITH PASSWORD 'esignforms' SUPERUSER;}}}

Create the deployment folder in the home directory (/home/esignforms/deployments) or C:\deployments.  On Windows, you will want to give all users full permissions to the C:\deployments folder and subfolders when testing. We are not sure the about the exact user permissions needed, but generally you want to access those files from Windows Explorer, your cygwin user, and the PostgreSQL user.

Update the `~/profile` to point to the locations where all your stuff is. The next time you run cygwin, it should have the new good values. You can test with `java -version` and `psql template1` to see that Java and Postgresql are setup for cygwin access.

## Create the database ##

Install of the SQL code from 'database/postgresql/ddl' into the 'ddl' folder in your home folder.  We use the 5 letter deployment id for creating the database for a given webapp.

Note that the `profile` script sets ESF\_DEPLOYMENT\_BASE to the base directory where your deployment databases (PostgreSQL tablespaces) are independently stored. On Windows, we may use `C:\deployments` and on Linux something like `/home/esignforms/deployments`.  The 'templates' folder should be created in the deployment folder automatically once you run the `create_db` script.

```
If you have a previous install and need to wipe it out first, use:
  ./drop_db

To create a DB, use (if you deploy your webapp in ROOT, use ROOT as the WEBAPPNAME to rundbsetup):
  ./create_db
  Please enter the LOWERCASE name for the OpenESF database and user: test          (for testing, we just use 'test' for our deployid and DB user)
  Please enter the password to use for the OpenESF esfapp user test: test          (for testing, we just use the password 'test')
  Type 'y' to create the database.
  Type 'y' to create the tables.
  
  ./rundbsetup WEBAPPNAME
  or
  In Eclipse, create a Java Application (debug configuration) for 'DbSetup' like below and then run it:
    Project: open-eSignFormsVaadin
    Main class: com.esignforms.open.db.tools.DbSetup
    Program Arguments: open-eSignFormsVaadin
    VM Arguments: -Xmx512m -Desf.deploybase=C:\deployments
    Working directory: C:\project\.metadata\.plugins\org.eclipse.wst.server.core\tmp2\wtpwebapps\open-eSignFormsVaadin\

    (Obviously, you'll need to tweak any of the specific locations/names to match your environment.)
    
    When you run DbSetup you'll see something like:

Copyright (c) 2009-2011 Yozons, Inc.
DbSetup - Sets up the database for Open eSignForms v1.7.0

Enter setup command (initdb,addsuperuser,initsetup,setpassword,quit) [quit] : initdb
2011-07-22 00:31:49.353 UTC-PublicKeyGenerator provider = BC version 1.46; keysize: 4096
insertDeployment - Created deployment with id: b96513a5-9618-40ab-a1cd-79abf611ea32
Enter boot password 1: test1
Enter boot password 2: test2
insertBootKey - Added new boot key
Added super group: ESF/Group/Deployment/SuperAdmin
Added system admin group: System/Administrator
Added All Users pseudo-group: ESF/Group/AllUsers
Added External Users pseudo-group: ESF/Group/ExternalUsers
createInitialProperties - Updated deployment with global properties id: adeb5303-bdbe-4d46-968e-a8787db45160; deployment properties id: d4acceb5-977c-484a-b801-9666f7e6fd07
Added template library: ESF/Library/Template

Enter setup command (initdb,addsuperuser,initsetup,setpassword,quit,convert1.5) [quit] : addsuperuser
Enter super user's email address [support@yozons.com] : super@openesfdemo.com
Enter super user's first/personal name [Yozons] : Super
Enter super user's last/family name [Support] : Openesf
Initial super user password: Test
insertSuperUser - Added new super user: super@openesfdemo.com
insertUserIntoSuperGroup - Added new super user: Super Openesf <super@openesfdemo.com>; to super group: ESF/Group/Deployment/SuperAdmin

Enter setup command (initdb,addsuperuser,initsetup,setpassword,quit,convert1.5) [quit] : initsetup
Enter Commercial DB license size in MB (enter 0 for AGPL deployment) [0] :
Enter company name: Demo Company
Enter company street address: 123 Main St.
Enter company city: Kirkland
Enter company state: WA
Enter company zip: 98033
Enter company default phone number [800.555.1212] : 800-555-4321
Enter company group EsfName [CompanyRenamePlease] : DemoCo
Enter company default email address [renameyser@pleaserenamecompany.com] : democo@david.com
Enter programmer user's email address [open-esign@yozons.com] : myprogrammer@openesfdemo.com
Created company group: com.esignforms.open.user.Group@a1c6407f
Created company programming group: com.esignforms.open.user.Group@a1c6407f
Added company programming group to Library, Package, Transaction Template and Transaction Listing views
Added company groups to list/view the template library
Created sample company library: Lib/DemoCo
Created default style and version: ESF_DefaultDocumentStyle
Set default style in template library: ESF/Library/Template
Set default style in company library: Lib/DemoCo
Created standard package document: StandardPackageDisclosures
Created image: Logo
Created image: SignHereLeftArrow
Created image: PackageDocumentCompleted
Created image: PackageDocumentFixRequested
Created image: PackageDocumentRejected
Created image: PackageDocumentToDo
Created image: PackageDocumentViewOnly
Created email template and version: SetPassword
Created email template and version: ForgotPassword
Created email template and version: PasswordChanged
Created email template and version: PasswordLockout
Created email template and version: DefaultPickupNotification
Created dropdown and version: ESF_BackgroundColor
Created dropdown and version: ESF_BorderTypes
Created dropdown and version: ESF_Font
Created dropdown and version: ESF_FontColor
Created dropdown and version: ESF_FontSize
Created dropdown and version: ESF_FontStyle
Created dropdown and version: ESF_TextAlign
Created dropdown and version: ESF_Locale
Created dropdown and version: ESF_TimeZone
Created dropdown and version: ESF_USA_PostalStatePossession
Created dropdown and version: ESF_USA_PostalStates
Created dropdown and version: ESF_PartyRenotifyTimes
Created dropdown and version: ESF_TimeIntervalUnits
Created drop and version: ESF_DateFormat
Created dropdown and version: ESF_TimeFormat
Created drop and version: ESF_DecimalFormat
Created drop and version: ESF_IntegerFormat
Created drop and version: ESF_MoneyFormat
Created propertyset and version: ESF
Created propertyset and version: MyCompany
Created propertyset and version: MyCompany
Added template package and version: ESF/Package/Template
Added template package and version: Package/Template
Added ESF template transaction template: ESF/TransactionTemplate/Template
Added company template transaction template: TransactionTemplate/Template
Created programming user: Open eSignForms Programming <myprogrammer@openesfdemo.com>

Enter setup command (initdb,addsuperuser,initsetup,setpassword,quit,convert1.5) [quit] : quit
```

## Running the Application in Eclipse ##

Generally for testing in Eclipse, click Debug As->Debug On Server->Apache->Tomcat v7.0 Server. The first time you just need to point it to where you installed Tomcat above.

### Windows file and folder permissions concerns ###

Open eSignForms creates a folder called `LIBRARYGENERATED-KEEP` in the webapp's deployment folder that is used to store generated CSS and JSP files.  On Linux, this seems to work naturally with respect to ownership and file/subdirectory permissions as set from the Java code, but we've seen issues in Windows where Tomcat/Eclipse is not given permission to create files in this location.

You will see the errors in the esf.log during startup for your deployment when this is the case.  Here's an example we get from time to time:
```
ERROR (com.esignforms.open.prog.DocumentVersion) generateCode() could not create read/write/execute document version directory: C:\project\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwe
bapps\open-eSignFormsVaadin\LIBRARYGENERATED-KEEP\document\279d0d44-5e41-4b95-a2cb-6f56547b3462\d775b182-e500-4bbf-bd27-f8e9db600654
```

Unfortunately, while we manage to resolve this suitable for a developer's personal deployment, we never really know which of our various attempts at setting up Windows security is the correct way.  Surely if you expect to run on Windows, this needs more attention.  In general, we struggle with folder permissions and granting permission so that the application can create files/folders/subfolders in this directory as needed.  If you have Windows expertise and know just want to do to make this happen, please let us know and we'll update this section.

In general, we find we need to give "Full control" permission to seemingly too many group/user names using the Security tab on the folder's properties.  We seem to need to use the Advanced button, giving permissions to "This folder, subfolders and files" (sometimes with "Include inheritable permissions from this object's parent" and "Replace all child object permissions with inheritable permissions from this object" checked.  It generally seems to take 2-3 restarts with permission settings for us to get it working. Ugh!

## SMTP and IMAP ##

Under the System config->Deployment link in the application, you must configure the SMTP Return Path hostname, SMTP server and IMAP server to use. These are Internet standard services external to Open eSignForms, but must be available. Open eSignForms sends out notifications and invitation emails using a scheme that will associate bounces and replies to the original email sent.

Our basic configuration works like this:

  * Set the SMTP Return Path Hostname to the hostname where you are runnning your SMTP server. The Return-Path SMTP header, along with the Reply-To and Sender headers, are used by receiving systems to validate that an email is legitimate and not spam sent through an open relay. In general, this is the server name where Open eSignForms is installed. On our demo system, we use the value `open.esignforms.com` which results in emails sent with a return-path something like: `Return-Path: <deploy2_pxriwpqowoislybxlucq@open.esignforms.com>`  The 'deploy2' value is technically the IMAP user name (normalized to lowercase and replacing any non-alphanumeric, period, underscore or hyphen by '`_`'), but we mirror it to also be the deployment id so that a single server can run multiple deployments of our application for multiple customers. The 'pxriwpqowoislybxlucq' is a random, unique value that allows us to associate bounces and replies to the original email sent.

  * Set the SMTP server to the server that sends out emails for your deployment. Typically this is the same as the SMTP eturn Path Hostname. You can also set the SMTP Port, SMTP Auth User, SMTP Auth Password and whether SSL should be used. For typical deployments, port 25 is fine, but for some home developer systems, you may find 587 works for you to bypass ISP firewalls. For a typical deployment where relaying is allowed only from the localhost, you may not need to set the Auth User, Password or SSL options.

  * Set the IMAP Server to the server that will handle inbound emails, such as bounces and replies. This most certainly is the same value as the SMTP ReturnPath Hostname. The IMAP Port of 143 is standard. You will want to set the IMAP User and Password to be used to retrieve emails. We typically create a user account for each deployment, so in the example above, we'd have a user account 'deploy2' created. Note that this user account is only used for receiving email, and we do not allow it to be accessed from the Internet for login purposes.  We recommend using SSH with a setting that restricts user login accounts, and if possible, do as we do and prohibit 'root' login and only allow logins via public key authorization so password guessing cannot be used to hack your system.

  * In our deployments, we use Postfix for SMTP, configured to allow only the localhost server to send out messages (external systems cannot relay through it). We then set the `/etc/postfix/virtual_alias` to use values like `/^deploy2_.*@open.esignforms.com$/ deploy2`  This basically will match that return path value and assign it to the right user account that we'll access via IMAP for correlating bounces and replies. Typical settings changes we make to the `/etc/postfix/main.cf` is (the domain is your server's name):
```
mydomain = example.com
home_mailbox = Maildir/
inet_interfaces = all
mynetworks_style = host
virtual_alias_maps = regexp:/etc/postfix/virtual_alias
```

  * In our deployments, we use dovecot for our IMAP server. With CentOS 6 (wasn't needed as late as CentOS 5.6) we seem to need to add the following to `/etc/dovecot/dovecot.conf` file:
```
mail_location = maildir:~/Maildir
mail_access_groups=mail
```

## Other tools ##

We also are making use of [FindBugs](http://findbugs.sourceforge.net/). They have an Eclipse update site `http://findbugs.cs.umd.edu/eclipse` that you can use to add it to your Eclipse. We also make use of [OWASP LAPSE](https://www.owasp.org/index.php/OWASP_LAPSE_Project).

While not needed for this particular effort, we recommend PasswordSafe (sourceforge) or something similar so you can remember one great pass phrase and keep your various other passwords unique, so when your bank or the like is hacked, at least the password is not used in other sites that become vulnerable as a result.

We also like LibreOffice or OpenOffice to replace any need for Microsoft Office.

For Windows, we use WinSCP and Putty for SSH/SCP access to our Linux servers.

# Linux setup #

For Linux deployment servers, we use a layout like this in the home directory for the application. For scripts and such, you may want to run `dos2unix` on them.

  * `~/bashrc` and `profile` are here; tweak as necessary for your setup.
  * `~/bin` contains our shell scripts
  * `~/deployments` contains the `template` folder hierarchy and is used to store the PostgreSQL database per web app deployment (via PostgreSQL's tablespace directive).
  * `~/java` is where we install our Java 7 Runtime Environment, such as jre7u21, and we create a softlink of that to 'jdk1.7' which is referenced in the `profile`. This allows updates to Java by changing where the softlink points. (i.e. `ln -s jdk1.7.0_21 jdk1.7`)
  * `~/postgresql` contains the location where PostgreSQL is installed. It has folders like `bin`, `data92` (where the main db is stored, but each web app's tablespace puts those databases in the `deployments` folder), `ddl`, `logs`, `pg92` (where PostgreSQL installs to) and `postgresql-9.2.4` (were we unzipped PostgreSQL and compiled it).
  * `~/tomcat` where we unzipped Tomcat into a folder like apache-tomcat-8.0.15, and then we create a softlink from that version to `tomcat8.0` which is referenced in the `profile` script. (i.e. `ln -s apache-tomcat-8.0.15 tomcat8.0`)
  * `~/wkhtmltox-0.12.1` which contains the code to [generate PDFs from HTML](wkhtmltopdf.md). We then create a softlink of the executable to our `bin` with something like `ln -s ../wkhtmltox-0.12.1/bin/wkhtmltopdf wkhtmltopdf` (command run from the `bin` directory).

## Mapping code from the Eclipse project to Linux paths ##

  * From `ECLIPSEPROJECTNAME/database/postgresql`, we put 'ddl' in `~postgresql` and run `dos2unix` on them.
  * From `ECLIPSEPROJECTNAME/database`, we put 'deployments' in `~`.
  * From `ECLIPSEPROJECTNAME/scripts`, we put all files in `~/bin` and run `dos2unix` on them. You can put them in a 'scripts' folder, too, but just remember to add that directory to your PATH. You can delete the `*.cygwin` files. You may want to do a `chmod +x ~/bin/*` if the scripts don't seem to have execute permission.
  * From `ECLIPSEPROJECTNAME/WebContent`, we put all files in `~/tomcat/tomcat7.0/webapps/WEBAPPNAME` where WEBAPPNAME is what you are calling your deployment.

## General Linux X64 server setup information ##

As `root` do the following on your new Linux instance:
  * `useradd esignforms` (assuming you install the code under this username)
  * `passwd root`  (be sure to set good passwords)
  * `passwd esignforms`  (be sure to set good passwords)
  * `yum update`  (to ensure you are up-to-date with everything)
  * `yum install iptables ntp logwatch dos2unix gpg bind-utils jwhois telnet traceroute make gcc libgcc gcc-c++ glibc-devel readline readline-devel ncurses ncurses-devel zlib zlib-devel zip unzip bzip2 pam pam-devel postfix screen lynx dovecot rsync`
  * See the [wkhtmltopdf wiki](wkhtmltopdf.md) for other components you need, including using the 32-bit versions of some libraries.
  * Put `ALL: ALL` in `/etc/hosts.deny`
  * Put `sshd: ALL` in `/etc/hosts.allow`
  * Ensure you `/etc/hosts` file is correct with your hostname and IP address.
  * `hostname esign.example.com`
  * Set up for you timezone, i.e. `rm -f /etc/localtime` and then `ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime`
  * Set `ZONE="America/Los_Angeles"` and `UTC=true` in `/etc/sysconfig/clock`
  * Set `HOSTNAME=esign.example.com` in `/etc/sysconfig/network`
  * Ensure your `/etc/resolv.conf` is set up for the name servers you can use to resolve external host names.
  * Create Yozons-specific `/usr/local/bin/sp` script wrapper for `ps` (`chmod 755`).
  * Create Yozons-specific `/etc/esfprofile` script for offsite backups. `chmod 640` and `chown root.esignforms`
  * Change `/etc/ssh/sshd_config` to use:
    * `PermitRootLogin no`
    * `PasswordAuthentication no`
    * `AllowUsers esignforms`

As `esignforms` do:
  * `mkdir .ssh`
  * `chmod 700 .ssh`
  * `cd .ssh`
  * Install your `authorized_keys` file so you can SSH in
  * `chmod 600 authorized_keys`
  * For compiling PostgreSQL, we use this configure commands:
```
    cd ~/postgresql/postgresql-9.2.4
    ./configure --prefix=/home/esignforms/postgresql/pg92 --with-pam
    gmake
    gmake install
    cd contrib/vacuumlo
    gmake
    cp -p vacuumlo ../../../bin/
    cd ../pg_standby
    gmake
    cp -p pg_standby ../../../bin/
    cd ~/postgresql
    initdb -D $PGDATA
    yostart db
    psql template1
       ALTER USER esignforms WITH PASSWORD 'DB-ADMIN-PASSWORD-HERE';
    yostop db    
```
  * Allow for local access to all databases (for multiple deployments) by updating `data92/pg_hba.conf`:
```
    local all all md5
    host all all 127.0.0.1/32 md5
```
  * A few tweaks to `data92/postgresql.conf` for basic operation:
```
    logging_collector = on
    log_directory = '../logs'
    log_line_prefix = '%m %d '
    log_connections = on
    log_disconnections = on
```
> > Restart PG: `yostart db`

As `root` again do:
  * `service sshd restart`
  * `service ntpd start`
  * `chkconfig ntpd on`
  * Setup `/etc/sysconfig/iptables` something like:
```
# We do simple NAT here to map HTTP/HTTPS to the safer unprivileged ports Tomcat listens on.
*nat
:PREROUTING ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A PREROUTING -p tcp -m tcp --dport 443 -j REDIRECT --to-ports 8443
-A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 8080
# causes outbound connections on my public IP to also redirect like above
-A OUTPUT -p tcp -m tcp -d esign.example.com --dport 443 -j REDIRECT --to-ports 8443
-A OUTPUT -p tcp -m tcp -d esign.example.com --dport 80 -j REDIRECT --to-ports 8080
COMMIT
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:RH-Firewall-1-INPUT - [0:0]
-A INPUT -j RH-Firewall-1-INPUT
-A FORWARD -j RH-Firewall-1-INPUT
-A RH-Firewall-1-INPUT -i lo -j ACCEPT
-A RH-Firewall-1-INPUT -p icmp --icmp-type any -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 25 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp -s localhost.localdomain -d localhost.localdomain --dport 143 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT
# 9090 is the tomcat debug port
# -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 9090 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8443 -j ACCEPT
-A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
COMMIT
```
  * Make changes to [/etc/postfix/main.cf](#SMTP_and_IMAP.md) as above.
  * Set up `/etc/postfix/virtual_alias` something like with the first being how to map individual deployments (named 'demo' in this example) as well as creating forwarding rules for the esignforms, postmaster and root users:
```
/^demo_.*@esign.example.com$/ demo
/^esignforms@esign.example.com$/ support@example.com
/^postmaster@esign.example.com$/ support@example.com
/^root@esign.example.com$/ support@example.com
```
  * Add `MailTo = support@example.com` to `/etc/logwatch/conf/logwatch.conf`
  * `service postfix start`
  * `chkconfig postfix on`
  * `service dovecot start`
  * `chkconfig dovecot on`
  * Here is a sample init script for auto-starting/stopping the application on boot and shutdown (so you can use `service esignforms start` and `service esignforms stop` for example). The latest Red Hat and CentOS has programs named `start` and `stop` so our start/stop scripts are often renamed to `yostart` and `yostop` along with similar changes in their contents in the `bin` subdirectory:
```
#!/bin/bash
#
# Copyright (c) 2012 Yozons Inc.  All rights reserved worldwide.
#
# Start in runlevels 3, 4, 5, with start at the end and stop at the beginning
# chkconfig: 345 99 3
# description: Init file for Yozons Open eSignForms web applications
#
INITFILE=/etc/rc.d/init.d/esignforms
LOCK=/var/lock/subsys/esignforms

# source in the function library
. /etc/rc.d/init.d/functions

start()
{
        echo -n "Starting Open eSignForms:"
        su - esignforms -c "bin/yostart all" && success || failure
        RETVAL=$?
        [ "$RETVAL" = 0 ] && touch $LOCK
        echo
}

stop()
{
        echo -n "Stopping Open eSignForms:"
        su - esignforms -c "bin/yostop all"
        RETVAL=$?
        [ "$RETVAL" = 0 ] && rm -f $LOCK
        echo
}

case "$1" in
        start)
                start
                ;;
        stop)
                stop
                ;;
        restart)
                stop
                sleep 5
                start
                ;;
        status)
                        echo Status of ntpd:
                /usr/local/bin/sp ntpd
                        echo Status of sshd:
                /usr/local/bin/sp sshd
                        echo Status of postfix SMTP:
                /usr/local/bin/sp postfix
                        echo Status of dovecot IMAP:
                /usr/local/bin/sp dovecot
                echo
                su - esignforms -c "bin/checkall"
                RETVAL=$?
                ;;
        install)
                echo Adding esignforms to runlevel system for auto start and stop
                cp $0 $INITFILE
                chmod 755 $INITFILE
                /sbin/chkconfig --add esignforms
                ;;
        uninstall)
                echo Removing esignforms from runlevel system for auto start and stop
                /sbin/chkconfig --del esignforms
                rm -f $INITFILE
                ;;
        *)
                echo $"Usage: $0 {start|stop|restart|status|install|uninstall}"
                RETVAL=1
esac
exit $RETVAL
```

### IPv6 use ###

We have no IPv6 expertise yet, but you may consider the following changes if you do not want IPv6 connections on your server if IPv6 is otherwise configured. The application has no specific knowledge of IPv6.

  * Add to `/etc/sysctl.conf`:
```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
```
  * For a running system:
```
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1
```
  * Change `/etc/sysconfig/network`:
```
NETWORKING_IPV6=no
```

  * You may want to turn it off for Postfix, too, in /etc/postfix/main.cf:
```
inet_protocols=ipv4
```

### Other considerations ###

If your installation reports errors like the following:
```
su: PAM adding faulty module: /lib64/security/pam_fprintd.so
su: PAM unable to dlopen(/lib64/security/pam_fprintd.so): /lib64/security/pam_fprintd.so: cannot open shared object file: No such file or directory
```

You may want to run the command to suppress them:
```
authconfig --disablefingerprint --update
```

# Upgrading to new releases #

In general, all new release need the following pattern of updates from the release code to the corresponding deployment location.

  * From your deployment area, remove/clear all files in the `VAADIN` folder so you get the latest themes and GWT code.  Also, you may want to do the same for all prior JAR files in `WEB-INF/lib`.  And if you have any patched code in WEB-INF/classes (besides the standard .properties files), you should remove any .class files you have there.
  * All files from the `WebContent` folder in the code to the webapp folder where you have deployed the application.  The only exceptions in general is `WebContent/WEB-INF/web.xml`, though you may need to refer to it when it changes so you can sync yours to it.
  * Normally, the `web.xml` file does not change, but some major changes like the upgrade from Vaadin 6 to Vaadin 7 required a new web.xml file.  In this case, it's a good idea to compare the two files and be sure to synchronized them. Typically, any given deployment can use the web.xml as is, but you may have tweaked the following elements `<display-name>`; `<description>`; in production you may have set `<context-param>` `productionMode` to `true`; `<session-timeout>`; and perhaps the `<security-constraint>` if on a non-SSL protected test server.
  * Copy all files from `VAADIN-release-extras/VAADIN` to `VAADIN`. This is particularly important for production systems that will not compile the SCSS theme files on the fly.
  * Copy all files from `VAADIN-release-extras/WEB-INF` to `WEB-INF`.  This includes a few WEB-INF/lib JARs that need to be in a running system, but Vaadin doesn't want in the normal `WebContent/WEB-INF/lib` folder as of Vaadin 7 and it's use of Ivy for dependency management.
  * Assuming you removed the `WEB-INF/lib/openesignforms*.jar` for the prior version, be sure to copy over the version from the `lib` folder of the project area.
  * As always, you may need to run SQL updates and 'rundbsetup' as described in the wiki [DatabaseUpdatesForNewReleases](DatabaseUpdatesForNewReleases.md).  You can review the `scripts/createRelease` and 'scripts/installRelease' for other details.