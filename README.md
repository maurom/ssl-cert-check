# SSL Certification Expiration Checker:

ssl-cert-check is a Bourne shell script that can be used to report on expiring SSL certificates. The script was designed to be run from cron and can e-mail warnings or log alerts through nagios.  

# Usage:
<pre>
$ ./ssl-cert-check
Usage: ./ssl-cert-check [OPTIONS] [-e email address] [-E sender email address] [-x days] [-T seconds]
       { [ -s common_name ] && [ -p port] } || { [ -f domains file ] } || { [ -c cert file ] } || { [ -d cert dir ] }

  -a                : Send a warning message through E-mail
  -b                : Will not print header
  -C                : Print the subject's common name of the certificate
  -c cert file      : Print the expiration date for the PEM or PKCS12 formatted certificate in cert file
  -d cert directory : Print the expiration date for the PEM or PKCS12 formatted certificates in cert directory
  -e E-mail address : E-mail address to send expiration notices
  -E E-mail sender  : E-mail address of the sender
  -f domains file   : File with a list of FQDNs and ports
  -F                : Add a status summary footer
  -h                : Print this screen
  -i                : Print the issuer of the certificate
  -k password       : PKCS12 file password
  -n                : Run as a Nagios plugin
  -N                : Run as a Nagios plugin and output one line summary (implies -n, requires -f or -d)
  -p port           : Port to connect to (interactive mode)
  -q                : Don't print anything on the console
  -s commmon name   : Server to connect to (interactive mode)
  -S                : Print validation information
  -t type           : Specify the certificate type
  -T seconds        : Timeout request after N seconds
  -V                : Print version information
  -x days           : Certificate expiration interval (eg. if cert_date < days)


</pre>

# Examples:

Print the expiration times for one or more certificates listed in ssldomains:

<pre>
$ ssl-cert-check -i -C -F -f ssldomains
Host                                Subject                   Issuer            Status   Expires     Days
----------------------------------- ------------------------- ----------------- -------- ----------- ----
www.spotch.com:443                  www.spotch.systems        Let's Encrypt     Valid    Aug 23 2021   33
os.educ.gob.ar:imaps                *.educ.gob.ar             Sectigo Limited   Expired  Jul 13 2021   -8
correo.ada.gba.gov.ar:smtps         correo.ada.gba.gov.ar     Let's Encrypt     Valid    Sep  4 2021   45
gmail.google.com:443                *.google.com              Google Trust Serv Valid    Sep 14 2021   55
www.debian.org:443                  www.debian.org            Let's Encrypt     Valid    Sep 26 2021   67
www.prefetch.com:443                Unable to resolve the DNS name              Unknown    0    0    0

6 endpoint(s) checked, 4 valid certificate(s), 1 expired, 0 expiring since on Jul 13 2021
</pre>

Check all certificates with file pattern "/etc/haproxy/ssl/\*.pem"

<pre>
$ ssl-cert-check -d "/etc/haproxy/ssl/*.pem"
Host                                            Status       Expires      Days
----------------------------------------------- ------------ ------------ ----
FILE:/etc/haproxy/ssl/example1.org.pem      Valid        Jan 6 2017   78                                 
FILE:/etc/haproxy/ssl/example2.org.pem      Valid        Jan 1 2017   73                                 
FILE:/etc/haproxy/ssl/example3.org.pem      Valid        Jan 6 2017   78                                 
</pre>

Send an e-mail to admin@prefetch.net if a domain listed in ssldomains will expire in the next 60-days:

<pre>
$ ssl-cert-check -a -f ssldomains -q -x 60 -e admin@prefetch.net
</pre>

# Additional Documentation

Documentation And Examples: http://prefetch.net/articles/checkcertificate.html
