# freebsd-glpi-agent-install
for  FreeBSD user 

＃ GLPI Settings 
The glpi-agent settings are as follows: 

* Obtained by git from GitHub. 
https://github.com/glpi-project/glpi-agent 
* Since multiple perl modules are required, the following tasks are performed: 
* Introduction of cpan-minus → p5-module-instlall from /usr/ports 
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
* Run the following command directly under the glpi-agent directory obtained by git to get the necessary perl modules via cpan. 
<pre>
[/usr/local/src/glpi-agent ] root # >>> cpanm --installdeps.
</pre>
