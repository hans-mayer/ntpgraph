# ntpgraph

### make NTP statistic files visible as graph 

for UNIX like systems 

ksh scripts using gawk, gnuplot and gnuplot-x11 

* ntp_shps
* ntp_shdiff
* ntptconv
* ntp_shavail

Ubuntu and Debian prerequisites:  apt-get install ksh gawk gnuplot gnuplot-x11

## ntp_shps 

### usage 

    # ntp_shps
     
    show NTP peerstats or loopstats values as graph  - v 2017 01 22 
      author: ntpgraph@ma.yer.at 

    usage: /uni/bin/ntp_shps [ -L ] -s|-i|-o|-d|-r|-j | -O|-D|-E|-S|-P [ -p value ] [ -a ] [ -A ] [ -t value ] [ -m min max ] [ -c value ] [ -l ] [ -w value ] [ -x range ][ -y range ] [ -F n ] [ -L ] [ -f IMG ] [ -Y string ] [ IP ] DATESPEC 
       DATESPEC is MMDD in year 2017 or YYYYMMDD or . (today) or - (yesterday) 
       DATESPEC may also be path to peerstats/loopstats file
       IP address  - only for peerstats graph, not for loopstats 
       -s          - success rate 
       -p value    - poll interval used for calculation of success rate, default 64 
       -c          - y-axis is number and not percent , usefull for success rate 
       -i          - interval between updates 
       -a          - print average line 
       -A          - save average result in file /tmp/ntp_shps_average / only with -a option usefull 
       -l          - straight line instead of smooth csplines 
       -f IMG      - output to file - IMG can be jpeg, png, ... 
       -x range    - low:high, example 1:10.75 , default autorange -0.5:24.8 
       -y range    - low:high, example -0.1:0.1 , default autorange 
       -t value    - timestemps per hour - default 1 
       -w value    - line width 
       -m min max  - minimum and maximum values for calculation, only for next 4 options below 
       -o          - show offset from peerstats file - column 5 
       -r          - show roundtrip delay from peerstats file - column 6 
       -d          - show dispersion from peerstats file - column 7 
       -j          - show rms jitter from peerstats file - column 8 
       -F n        - fit function, n polynomial ( 1 or 2 )  
       -b          - label at bottom - only for fit function 
       -Z          - debug 
       -Y string   - y-axis format, example 8.6f 
       -L          - use loopstats file instead of peerstats file 
       -O          - show offset from loopstats file - column 3 
       -D          - show drift compensation from loopstats file - column 4 
       -E          - show estimated error from loopstats file - column 5 
       -S          - show stability from loopstats file - column 6 
       -P          - show polling interval from loopstats file - column 7 



### examples 

#### offset by a GPS disciplined NTP server 

###### ntp_shps -a -o -f png 127.127.28.1 0711

![](img/plot_7026.png)

#### success rate for a DCF receiver 

of course it can't be more than 100%.
but there two reasons why the graph shows more than 100 % 
* 1) the reference clock is updated each 64 seconds.  
therefore an exact count of possible updates within one hour is hard to calculate
* 2) the smooth function generates an overshot. 
adding the option -l gives sometimes better results 

###### ntp_shps -a -s -f png 127.127.8.0 20170112

![](img/plot_24978.png)


#### interval between updates for a DCF receiver 

if all data-grams are received all intervals are 64 seconds 
this gives an indication how well the receiver performs 

###### ntp_shps -a -i -f png -y -50:300 127.127.8.0 0112

![](img/plot_24942.png)


#### round-trip delay between a remote peer and the local server 

the local NTP server is connected with ADSL to the Internet 

###### ntp_shps -a -r -y 0:0.03 -f png some.ip.addr 0723 

![](img/plot_7266.png)

#### using the FIT function to interpolate the measured values 

The fit option must have an additional value of 1 or 2 

The functions is for <br />
<pre>
1: line(x) = y0 + m*x   
2: line(x) = y0 + m*x + n*x^2
</pre>

###### ntp_shps -o -b -x 10:18 -F 1 -f png  127.127.22.0 .

![](img/plot_11845.png) 

Now you get an additional ( green ) line with function: line(x) = y0 + m*x 

On error output one can directly read the value: m = -1.18075e-05

With debug option -D the fit log file "/tmp/fit.log.$$" will not be deleted. 


## ntp_shdiff 

### usage 

    # ntp_shdiff
     
    show time difference for 2 NTP server as graph - v 2017 01 22 
      author: ntpgraph@ma.yer.at 

    usage: /uni/bin/ntp_shdiff [ -a ] [ -f ] [ -l ] [ -m value ] [ -c low high ] [ -t value ] [ -w value ] [ -y range ] [ -F n ] [ -L ] IP1 IP2 DATESPEC 
       DATESPEC is MMDD in year 2017 or YYYYMMDD or . (today) or - (yesterday) 
       DATESPEC can also be path to peerstats file
       -x range      - low:high, example 1:10 , default autorange -0.5:24.8 , time in hours 
       -y range      - low:high, example -0.1:0.1 , default autorange 
       -a            - print average line 
       -l            - straight line instead of smooth csplines 
       -f IMG        - output to file in current working directory - IMG can be jpeg, png, ... 
       -t number     - values per hour for average calculation, default is 1  
       -m value      - maximum time difference - default 1.1 second 
       -c low high   - lower and upper limit to calculate - default 1.1 second 
       -F n          - fit function, n polynomial ( 1 or 2 ) 
       -L            - label at bottom - only for fit function 
       -w value      - line width 
       -D            - debug 

### example

#### time difference between two NTP server 

###### ntp_shdiff -a -f png 127.127.28.1 some.ip.addr 0724

![](img/plot_7381.png)


## ntptconv 

#### ntp time convert 

make the time stamp in various statistic files human readable 

example 

without ntptconv 

    $ cat /var/log/ntpstats/peerstats.20150725 | grep 0.001142971
    57228 86324.503 192.168.241.190 9024 0.005973466 0.001142971 0.000946181 0.000013892

with ntptconv

    $ cat /var/log/ntpstats/peerstats.20150725 | grep 0.001142971 | ntptconv
    57228 23:58:44 192.168.241.190 9024 0.005973466 0.001142971 0.000946181 0.000013892


## ntp_shavail 

### usage

    # ntp_shavail

    show NTP available peers as graph  - v 2016 02 06
      author: ntpgraph@ma.yer.at
    
    usage: ntp_shavail [ -D ] [ -f IMG ] DATE
       date is MMDD in year 2016 or YYYYMMDD or . or - ( . is today, - is yesterday  )
       -f IMG      - output to file - IMG can be jpeg, png, ...
       -D          - debug

ntp_shavail will show all available NTP server for a given day. On the Y axis one can see all server. For example server #3 ( 192.168.241.190 ) called "blitz". All it's dots are on the base line which is 3.0 - the .0 says "reject". An other example for server #4. It was most of the time a candidate (+) on line 4.4 and sometime a peer (*) on y-value 4.6 <br />
The symbols ( x - + # o ) have the same meaning as "ntpq" shows.

ntp_shavail -f png .

![](img/plot_19046.png)


