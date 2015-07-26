# ntpgraph

### make NTP statistic files visible 

## ntp_shps 

### usage 

    # ntp_shps
     
    show NTP peerstats values as graph  - v 2015 07 14
    usage: /opt/iiasa/bin/ntp_shps -s|-i|-o|-d|-r|-j [ -p value ] [ -t value ] [ -m min max ] [ -c value ] [ -l ] [ -y range ] [ -f IMG ] IP DATE
       date is MMDD in year 2015 or YYYYMMDD or . ( . is today )
       -s          - success rate
       -i          - interval between updates
       -a          - print average line
       -l          - straight line instead of smooth csplines
       -f IMG      - output to file - IMG can be jpeg, png, ...
       -y range    - low:high, example -0.1:0.1 , default autorange
       -c value    - y-axis is number and not percent
       -t value    - timestemps per hour - default 1
       -p value    - poll interval used for calculation
       -m min max  - minimum and maximum values for calculation, only for options below
       -o          - show offset - column 5
       -r          - show roundtrip delay - column 6
       -d          - show dispersion - column 7
       -j          - show rms jitter - column 8
       -D          - debug

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

###### ntp_shps -a -s -f png 127.127.8.0 0725

![](img/plot_22516.png)


#### interval between updates for a DCF receiver 

if all datagrams are received all intervals are 64 seconds 
this gives an indication how well the receiver performs 

###### ntp_shps -a -i -f png -y -50:300 127.127.8.0 0723

![](img/plot_22693.png)


#### roundtrip delay between a remote peer and the local server 

the local NTP server is connected with ADSL to the Internet 

###### ntp_shps -a -r -y 0:0.03 -f png 178.189.127.148 0723 

![](img/plot_7266.png)


## ntp_shdiff 

### usage 

    # ntp_shdiff
     
    show time difference for 2 NTP server - v 2015 07 15
    usage: /usr/local/bin/ntp_shdiff [ -a ] [ -f ] [ -l ] [ -m value ] [ -t value ] [ -y range ] IP1 IP2 date
       date is MMDD in year 2015 or YYYYMMDD or . ( . is today )
       -y range   - low:high, example -0.1:0.1 , default autorange
       -a         - print average line
       -l         - straight line instead of smooth csplines
       -f IMG     - output to file in current working directory - IMG can be jpeg, png, ...
       -t number  - values per hour
       -m value   - maximum time difference - default 1.1
       -D         - debug

### example

#### time difference between two NTP server 

###### ntp_shdiff -a -f png 127.127.28.1 178.189.127.148 0724

![](img/plot_7381.png)


## ntptconv 

#### ntp time convert 

make the time stamp in varios statistic files human readable 

example 

    $ cat /var/log/ntpstats/peerstats.20150725 | grep 0.001142971
    57228 86324.503 192.168.241.190 9024 0.005973466 0.001142971 0.000946181 0.000013892
    $ cat /var/log/ntpstats/peerstats.20150725 | grep 0.001142971 | ntptconv
    57228 23:58:44 192.168.241.190 9024 0.005973466 0.001142971 0.000946181 0.000013892


