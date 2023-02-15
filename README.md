# freebsd-glpi-agent-install
for  FreeBSD user 

＃ GLPI Settings 
The glpi-agent settings are as follows: 

* Obtained by git from GitHub. 
https://github.com/glpi-project/glpi-agent 
* Since multiple perl modules are required, the following tasks are performed: 
* Introduction of cpan-minus → p5-module-instlall from /usr/ports
* 
* Run the following command directly under the glpi-agent directory obtained by git to get the necessary perl modules via cpan. 
<pre>
[/usr/local/src/glpi-agent ] root # >>> cpanm --installdeps.
</pre>
