#### The below python code use to pull the data from website (stackflow.py).

	import subprocess
	import mechanize
	import urlparse
	from bs4 import BeautifulSoup
	import urllib2 
	import cookielib
	cj = cookielib.CookieJar()
	br = mechanize.Browser()
	br.set_cookiejar(cj)
	br.open("https://******/login")

	br.select_form(nr=0)
	br.form['name'] = 'username'
	br.form['password'] = 'password'
	br.submit()
	#print =br.response().read()
	with open('contrat.txt') as f:
		for line in f:

			response = br.open(urlparse.urljoin('https://targetpage', line))
			r = response.read()
			text_file = open('output.txt', 'w')
			text_file.write(r)
			text_file.close()
			test = (subprocess.check_output("cat output.txt  | grep string1", shell=True))
			urls = re.findall(r'href=[\'"]?([^\'" >]+)', test) # we are finding all links in test
			con =  ', '.join(urls).split("/")[4] #4th link on page
			response1 = br.open(urlparse.urljoin('https://another url here', con)) #concatenation
			w =  response1.read()
			text_file = open('output1.txt', 'w')
			text_file.write(w)
			text_file.close()
			test1 = (subprocess.check_output("cat output1.txt | grep -w -A1 -e emp_name -e code_name -e address -e id | grep -v -e '<th>' -e 'label' -e 'input'", shell=True))
			print line
			print test1



#### The below code will align the report, 

	python stackflow.py > 02_06.txt

	cat 02_06.txt  | sed 's/<\/td>//g' | sed 's/<td>//g' | sed 's/^[ \t]*//;s/[ \t]*$//' | grep -v -e ^- -e ^$ | grep -v -B1 ^econ | grep -v  ^- | sed "s/$/,/g"| awk 'ORS=NR%5?FS:RS'

	1)sed 's/<\/td>//g' - will remove the <\/td> tag
	2)sed 's/<td>//g' - will remove the <td> tag
	3)sed 's/^[ \t]*//;s/[ \t]*$//' - replaces leading whitespace &  trailing whitespace
	4)grep -v -e ^- -e ^$ - Remove line start with - and  blanks lines
	5)grep -v -B1 ^econ - remove one line before the matching pattern.
	6)grep -v  ^- - Remove line start with -
	7)sed "s/$/,/g" - Add comma at end of every lines.
	8)awk 'ORS=NR%5?FS:RS' - Concatenate 5 words per line.


#### The below python script is use to auto login on jump server from local system and form tunnel with storage box without any addition package on any servers
##### Install paramiko module on local machine.
##### Run the below script from local machine.

	import paramiko
	from sshtunnel import SSHTunnelForwarder
	with SSHTunnelForwarder(
	   ('servername', 22),		#jump server address
	    ssh_username='ajay.prajapati',
	    ssh_pkey=paramiko.RSAKey.from_private_key_file("/root/.ssh/id_rsa"),
	    #ssh_private_key_password="*******",
	   remote_bind_address=("*.*.*.*", 22), #storage box ip address 
	   local_bind_address=('127.0.0.1', 10023)
	) as tunnel:
	   client = paramiko.SSHClient()
		#client.load_system_host_keys()
	   client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
	   client.connect(hostname='127.0.0.1', username="root", password="****", port=10023, look_for_keys=False)
		# do some operations with client session
	   stdin, stdout, stderr = (client.exec_command('./datapull.sh')) # it will run the datapull.sh which is on storage box.
	   print stdout.readlines()
	   print stderr.readlines()
	   client.close()
	   
#### The command used to upload the file on wesite- This example to upload file on confluence.
	cat confluence.py import subprocess subprocess.check_output("curl -v -S -X POST -H 'X-Atlassian-Token: no-check' -F 	file='@filename.txt' 'https:********* username=******&os_password=*********'", shell=True)`

#### The below command will print the MAC and IP address for provided interface - it ran on Centos/Redhat - 7
	ip addr show enp0s8 | grep -w  -e  inet -e ether | awk '{ print $2; }' | sed 's/\/.*$//'
	
#### Output-
		**:00:27:**:d7:**
		10.0.0.**

#### Upgrade the python from 2.7.5 to 3.5.2

	1) wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz
	2) tar -xvzf Python-3.5.2.tgz
	3) cd Python-3.5.2
	4) ./configure --prefix=/usr/local
	5) make altinstall
	6) alias python=/usr/bin/python3 and enter the same entry in  ~/.bashrc file to make it permanent
	
	File - .bashrc is below

	# User specific aliases and functions

	alias python=/usr/local/bin/python3.5
	alias rm='rm -i'
	alias cp='cp -i'
	alias mv='mv -i'

	# Source global definitions
	if [ -f /etc/bashrc ]; then
        . /etc/bashrc
	fi
	
	7) logout from current bash and relogin
	
	8) verify with below command:
	[root@main3 ~]# python --version
	Python 3.5.2

#### How to find package name by using command:
	[root@****]# rpm -qf $(which top)
	output - procps-ng-3.3.10-10.el7.x86_64

#### For loop example 
		for i in `cat serverlist.txt | awk '{ print $2 }'` ; do ssh@$i; done
	bash: ssh@10.0.0.1: command not found...
	bash: ssh@10.0.0.2: command not found...


		]# for i in `seq 1 10` ; do echo file$i; done
		file1
		file2
		file3
		file4
		file5
		file6
		file7
		file8
		file9
		file10
