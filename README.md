# freebsd-glpi-agent-install
for  FreeBSD user tips

# Prerequisite:

* perl5.x installed
* "devel/git" installd
* "ftp/curl" installed 
* "ports-mgmt/portmaster" installed 
* "sysutils/dmidecode" installed

# GLPI-agent source
The glpi-agent settings are as follows: 

* Obtained by git from GitHub. 
https://github.com/glpi-project/glpi-agent 
* Since multiple perl modules are required, the following tasks are performed: 
* Introduction of cpan-minus  
--  Install → p5-module-install from /usr/ports 
Exapmle:
<pre>

===>>> The following actions were performed:
	Installation of converters/p5-JSON (p5-JSON-4.10)
	Installation of devel/p5-File-Remove (p5-File-Remove-1.58)
	Installation of devel/p5-Module-ScanDeps (p5-Module-ScanDeps-1.31)
	Installation of devel/p5-PAR-Dist (p5-PAR-Dist-0.51)
	Installation of textproc/p5-YAML-Tiny (p5-YAML-Tiny-1.73)
	Installation of devel/p5-Module-Install (p5-Module-Install-1.19)


</pre>
# cpan-minus install from cpan

<pre>
$ sudo curl -L https://cpanmin.us | perl - --sudo App::cpanminus 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  295k  100  295k    0     0   434k      0 --:--:-- --:--:-- --:--:--  433k
--> Working on App::cpanminus
Fetching http://www.cpan.org/authors/id/M/MI/MIYAGAWA/App-cpanminus-1.7046.tar.gz ... OK
Configuring App-cpanminus-1.7046 ... OK
Building and testing App-cpanminus-1.7046 ... OK
Successfully installed App-cpanminus-1.7046
1 distribution installed

</pre>

# fetch libraries from cpan and build.
* Run the following command directly under the glpi-agent directory obtained by git to get the necessary perl modules via cpan. 
<pre>
[/usr/local/src/glpi-agent]$ ls -la
total 275
drwxr-xr-x  17 root  wheel      30 Feb 14 11:08 .
drwxr-xr-x   3 root  wheel       3 Feb 14 11:03 ..
drwxr-xr-x   8 root  wheel      13 Feb 13 12:26 .git
drwxr-xr-x   5 root  wheel       5 Feb 13 12:26 .github
-rw-r--r--   1 root  wheel     348 Feb 13 12:26 .gitignore
-rw-r--r--   1 root  wheel    1006 Feb 13 12:26 CONTRIB.md
-rw-r--r--   1 root  wheel  155660 Feb 13 12:26 Changes
-rw-r--r--   1 root  wheel   17987 Feb 13 12:26 LICENSE
-rw-r--r--   1 root  wheel     457 Feb 13 12:26 MANIFEST.SKIP
-rw-r--r--   1 root  wheel    1839 Feb 14 11:07 META.yml
-rw-r--r--   1 root  wheel    3305 Feb 14 11:07 MYMETA.json
-rw-r--r--   1 root  wheel    2012 Feb 14 11:07 MYMETA.yml
-rw-r--r--   1 root  wheel  122488 Feb 14 11:07 Makefile
-rw-r--r--   1 root  wheel   10752 Feb 13 12:26 Makefile.PL
-rw-r--r--   1 root  wheel    6102 Feb 13 12:26 README.md
-rw-r--r--   1 root  wheel    3204 Feb 13 12:26 THANKS
drwxr-xr-x   2 root  wheel      12 Feb 13 12:26 bin
drwxr-xr-x   8 root  wheel       8 Feb 13 12:27 blib
drwxr-xr-x   6 root  wheel       6 Feb 13 12:26 contrib
drwxr-xr-x   6 root  wheel      22 Feb 13 12:26 debian
drwxr-xr-x   2 root  wheel      10 Feb 13 12:26 etc
drwxr-xr-x   4 root  wheel       4 Feb 14 11:07 inc
drwxr-xr-x   3 root  wheel       4 Feb 13 12:26 lib
-rw-r--r--   1 root  wheel       0 Feb 14 11:07 pm_to_blib
drwxr-xr-x  19 root  wheel      19 Feb 13 12:26 resources
drwxr-xr-x   3 root  wheel       7 Feb 13 12:26 share
drwxr-xr-x   3 root  wheel       4 Feb 13 12:26 snap
drwxr-xr-x   6 root  wheel      17 Feb 13 12:26 t
drwxr-xr-x   2 root  wheel      13 Feb 13 12:26 tools
drwxr-xr-x   5 root  wheel       6 Feb 14 11:06 var

[/usr/local/src/glpi-agent ] root # >>> cpanm --installdeps.
--> Working on .
Configuring GLPI-Agent-1.5-dev ... OK
==> Found dependencies: Test::Deep, DateTime, UNIVERSAL::require, File::Which, IPC::Run, Text::Template, IO::Capture::Stderr, Test::Exception, File::Copy::Recursive, Net::IP, LWP::Protocol::https, HTTP::Server::Simple::Authen, Test::NoWarnings, Parallel::ForkManager, HTTP::Server::Simple, Test::MockObject, Cpanel::JSON::XS, HTTP::Proxy, Test::Compile, LWP::UserAgent, Data::UUID, XML::LibXML, Test::MockModule
--> Working on Test::Deep
Fetching http://www.cpan.org/authors/id/R/RJ/RJBS/Test-Deep-1.204.tar.gz ... OK
Configuring Test-Deep-1.204 ... OK
Building and testing Test-Deep-1.204 ... OK
Successfully installed Test-Deep-1.204

-- <snip> --

</pre>

* Fail.. 
<pre>
! Installing the dependencies failed: Module 'XML::LibXML' is not installed, Module 'HTTP::Proxy' is not installed
! Bailing out the installation for GLPI-Agent-1.5-dev.
</pre>

# deploy and build textproc/p5-XML-LibXML and www/p5-HTTP-PROXY from ports 
* PERL module XML::LibXML depend on textproc/libxml2. if CPAN install fail cause by "libxml2" then install from /usr/ports/textproc/libxml2
* TimeOut: HTTP::Proxy install with cpanminus 
<pre>
Fetching http://www.cpan.org/authors/id/H/HA/HAARG/Test-Needs-0.002010.tar.gz ... OK
Configuring Test-Needs-0.002010 ... OK
Building and testing Test-Needs-0.002010 ... OK
Successfully installed Test-Needs-0.002010
Building and testing HTTP-Daemon-6.14 ... OK
Successfully installed HTTP-Daemon-6.14
Building and testing HTTP-Proxy-0.304 ... 
--- snip  ---
! Installing the dependencies failed: Module 'XML::LibXML' is not installed, Module 'HTTP::Proxy' is not installed
! Bailing out the installation for GLPI-Agent-1.5-dev.

</pre>
 
Countermeasure:
<pre>
$ sudo portmaster -Gdy textproc/p5-XML-LibXML www/p5-HTTP-Proxy
</pre>

# check dependency 
<pre>

$ sudo cpanm --installdeps .                                                
--> Working on .
Configuring GLPI-Agent-1.5-dev ... OK
<== Installed dependencies for .. Finishing.

</pre>

# for Debian 

<pre>
# apt-get install libhttp-proxy-perl libxml-simple-perl
</pre>
# for Raspian 

if not installed Module::Install then use CPAN installer.

<pre>
# cpan --install Module::Install
Loading internal logger. Log::Log4perl recommended for better logging

CPAN.pm requires configuration, but most of it can be done automatically.
If you answer 'no' below, you will enter an interactive dialog for each
configuration option instead.

Would you like to configure as much as possible automatically? [yes] Y
Fetching with LWP:http://www.cpan.org/authors/01mailrc.txt.gz
Reading '/root/.cpan/sources/authors/01mailrc.txt.gz'
............................................................................DONE
Fetching with LWP:
http://www.cpan.org/modules/02packages.details.txt.gz
Reading '/root/.cpan/sources/modules/02packages.details.txt.gz'
  Database was generated on Fri, 02 Jun 2023 01:53:58 GMT
Warning: Your system date is 17 days behind this index file!
  System time:          Mon May 15 14:04:57 2023
  Timestamp index file: Fri, 02 Jun 2023 01:53:58 GMT
  Please fix your system time, problems with the make command expected.
..............
  New CPAN.pm version (v2.36) available.
  [Currently running version is v2.27]
  You might want to try
    install CPAN
    reload cpan
  to both upgrade CPAN.pm and run the new version without leaving
  the current session.

... snip ...



</pre>

# Problem lack of XML/LibXML and XML-SAX
## lack of LibXML 
Error Exaused by lack of XML::LibXML module and etc.
<pre>
# glpi-inventory 
Can't locate XML/LibXML.pm in @INC (you may need to install the XML::LibXML module) (@INC contains: /usr/local/share/glpi-agent/lib /usr/local/lib/perl5/site_perl/mach/5.32 /usr/local/lib/perl5/site_perl /usr/local/lib/perl5/5.32/mach /usr/local/lib/perl5/5.32) at /usr/local/share/glpi-agent/lib/GLPI/Agent/XML.pm line 6.
BEGIN failed--compilation aborted at /usr/local/share/glpi-agent/lib/GLPI/Agent/XML.pm line 6.
Compilation failed in require at /usr/local/share/glpi-agent/lib/GLPI/Agent/Inventory.pm line 13.
BEGIN failed--compilation aborted at /usr/local/share/glpi-agent/lib/GLPI/Agent/Inventory.pm line 13.
Compilation failed in require at /usr/local/share/glpi-agent/lib/GLPI/Agent/Task/Inventory.pm line 13.
BEGIN failed--compilation aborted at /usr/local/share/glpi-agent/lib/GLPI/Agent/Task/Inventory.pm line 13.
Compilation failed in require at /usr/local/bin/glpi-inventory line 14.
BEGIN failed--compilation aborted at /usr/local/bin/glpi-inventory line 14.
</pre>

<pre>
# cd /usr/ports/*/p5-XML-LibXML
# portmaster -Gdty --no-confirm
..
// snip //
..
Installing p5-XML-LibXML-2.0208_1,1...

===>>> The following actions were performed:
	Installation of textproc/libxml2 (libxml2-2.10.4)
	Installation of textproc/p5-XML-LibXML (p5-XML-LibXML-2.0208_1,1)


</pre>
## XML-SAX 
require .. ''glpi-inventory''

<pre>
p5-XML-NamespaceSupport-1.12   Simple generic namespace support class
p5-XML-SAX-1.02                Simple API for XML
p5-XML-SAX-Base-1.09           Base class SAX Drivers and Filters
</pre>

If lack these perl-packges no inventory send to server.

# daemon mode
stack by lack of Clone.pm ..

<pre>
/usr/local/bin/glpi-agent --daemon    
[info] GLPI Agent starting
[error] Failed to load HTTP server: Can't locate Clone.pm in @INC (you may need to install the Clone module) (@INC contains: /usr/local/share/glpi-agent/lib /usr/local/lib/perl5/site_perl/mach/5.36 /usr/local/lib/perl5/site_perl /usr/local/lib/perl5/5.36/mach /usr/local/lib/perl5/5.36) at /usr/local/lib/perl5/site_perl/HTTP/Headers.pm line 8.
BEGIN failed--compilation aborted at /usr/local/lib/perl5/site_perl/HTTP/Headers.pm line 8.
Compilation failed in require at /usr/local/lib/perl5/site_perl/HTTP/Message.pm line 8.
Compilation failed in require at /usr/local/lib/perl5/5.36/parent.pm line 16.
BEGIN failed--compilation aborted at /usr/local/lib/perl5/site_perl/HTTP/Request.pm line 8.
Compilation failed in require at /usr/local/lib/perl5/site_perl/HTTP/Daemon.pm line 85.
BEGIN failed--compilation aborted at /usr/local/lib/perl5/site_perl/HTTP/Daemon.pm line 85.
Compilation failed in require at /usr/local/share/glpi-agent/lib/GLPI/Agent/HTTP/Server.pm line 9.
BEGIN failed--compilation aborted at /usr/local/share/glpi-agent/lib/GLPI/Agent/HTTP/Server.pm line 9.
Compilation failed in require at /usr/local/share/glpi-agent/lib/GLPI/Agent/Daemon.pm line 708.
[info] target server0: next run: Fri Jun  2 11:44:16 2023 - https://glpi.shorijo.cmk.or.jp/
^C[info] GLPI Agent exiting (81225)

</pre>

... What?? 

<pre>
# cd /usr/ports/*/p5-Clone
# portmaster -Gdty --no-confirm                                                              

===>>> Currently installed version: p5-Clone-0.46
===>>> Port directory: /usr/ports/devel/p5-Clone

===>>> Gathering distinfo list for installed ports

===>>> Launching 'make checksum' for devel/p5-Clone in background
===>>> Gathering dependency list for devel/p5-Clone from ports
===>>> Initial dependency check complete for devel/p5-Clone


===>>> Starting build for devel/p5-Clone <<<===

===>>> All dependencies are up to date

===>  Cleaning for p5-Clone-0.46
===>>> Waiting on fetch & checksum for devel/p5-Clone <<<===
===>  License ART10 GPLv1+ accepted by the user
===>   p5-Clone-0.46 depends on file: /usr/local/sbin/pkg - found
=> Clone-0.46.tar.gz doesn't seem to exist in /usr/ports/distfiles//.
=> Attempting to fetch https://cpan.metacpan.org/modules/by-module/Clone/Clone-0.46.tar.gz


===>  License ART10 GPLv1+ accepted by the user
===>   p5-Clone-0.46 depends on file: /usr/local/sbin/pkg - found
===> Fetching all distfiles required by p5-Clone-0.46 for building
===>  Extracting for p5-Clone-0.46
=> SHA256 Checksum OK for Clone-0.46.tar.gz.
===>  Patching for p5-Clone-0.46
===>   p5-Clone-0.46 depends on package: perl5>=5.36<5.37 - found
===>  Configuring for p5-Clone-0.46
Checking if your kit is complete...
Looks good
Warning: prerequisite B::COW 0.004 not found.
Generating a Unix-style Makefile
Writing Makefile for Clone
Writing MYMETA.yml and MYMETA.json
===>  Building for p5-Clone-0.46
--- blib/lib/.exists ---
--- blib/arch/.exists ---
--- blib/lib/auto/Clone/.exists ---
--- blib/arch/auto/Clone/.exists ---
--- blib/bin/.exists ---
--- blib/script/.exists ---
--- blib/man1/.exists ---
--- blib/man3/.exists ---
--- config ---
--- subdirs ---
--- dynamic ---
--- blibdirs ---
--- Clone.c ---
--- Clone.bs ---
--- pm_to_blib ---
--- config ---
--- Clone.c ---
"/usr/local/bin/perl" "/usr/local/lib/perl5/5.36/ExtUtils/xsubpp"  -typemap '/usr/local/lib/perl5/5.36/ExtUtils/typemap'  Clone.xs > Clone.xsc
--- Clone.bs ---
Running Mkbootstrap for Clone ()
chmod 644 "Clone.bs"
--- blib/arch/auto/Clone/Clone.bs ---
"/usr/local/bin/perl" -MExtUtils::Command::MM -e 'cp_nonempty' -- Clone.bs blib/arch/auto/Clone/Clone.bs 644
--- pm_to_blib ---
cp Clone.pm blib/lib/Clone.pm
AutoSplitting blib/lib/Clone.pm (blib/lib/auto/Clone)
--- Clone.c ---
mv Clone.xsc Clone.c
--- Clone.o ---
cc -c    -O2 -pipe  -fstack-protector-strong -fno-strict-aliasing -O2 -pipe -fstack-protector-strong -fno-strict-aliasing    -DVERSION=\"0.46\"  -DXS_VERSION=\"0.46\" -DPIC -fPIC "-I/usr/local/lib/perl5/5.36/mach/CORE"   Clone.c
--- blib/arch/auto/Clone/Clone.so ---
rm -f blib/arch/auto/Clone/Clone.so
cc  -shared  -L/usr/local/lib/perl5/5.36/mach/CORE -lperl -L/usr/local/lib -fstack-protector-strong  Clone.o  -o blib/arch/auto/Clone/Clone.so        
chmod 755 blib/arch/auto/Clone/Clone.so
--- dynamic ---
--- linkext ---
--- pure_all ---
--- manifypods ---
Manifying 1 pod document
--- all ---
===>>> Building the port required 0 seconds
===>  Staging for p5-Clone-0.46
===>   Generating temporary packing list
"/usr/local/bin/perl" -MExtUtils::Command::MM -e 'cp_nonempty' -- Clone.bs blib/arch/auto/Clone/Clone.bs 644
Manifying 1 pod document
Files found in blib/arch: installing files in blib/lib into architecture dependent library tree
Installing /usr/ports/devel/p5-Clone/work/stage/usr/local/lib/perl5/site_perl/mach/5.36/auto/Clone/Clone.so
Installing /usr/ports/devel/p5-Clone/work/stage/usr/local/lib/perl5/site_perl/mach/5.36/Clone.pm
Installing /usr/ports/devel/p5-Clone/work/stage/usr/local/lib/perl5/site_perl/mach/5.36/auto/Clone/autosplit.ix
Installing /usr/ports/devel/p5-Clone/work/stage/usr/local/lib/perl5/site_perl/man/man3/Clone.3
/usr/bin/strip /usr/ports/devel/p5-Clone/work/stage/usr/local/lib/perl5/site_perl/mach/5.36/auto/Clone/Clone.so
====> Compressing man pages (compress-man)

===>>> Creating a backup package for old version p5-Clone-0.46
Creating package for p5-Clone-0.46
Updating database digests format: 100%
Checking integrity... done (0 conflicting)
Deinstallation has been requested for the following 1 packages (of 0 packages in the universe):

Installed packages to be REMOVED:
	p5-Clone: 0.46

Number of packages to be removed: 1
[1/1] Deinstalling p5-Clone-0.46...
[1/1] Deleting files for p5-Clone-0.46: 100%

===>  Installing for p5-Clone-0.46
===>  Checking if p5-Clone is already installed
===>   Registering installation for p5-Clone-0.46 as automatic
Installing p5-Clone-0.46...

===>>> Re-installation of p5-Clone-0.46 complete

</pre>
