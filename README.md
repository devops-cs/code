###The below python code use to pull the data from website.

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
