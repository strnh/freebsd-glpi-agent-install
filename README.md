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
--  Install → p5-module-instlall from /usr/ports 
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

