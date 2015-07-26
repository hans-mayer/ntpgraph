# ntpgraph

### make NTP statistic files visible 

## ntp_shps examples 

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

#### offset for a GPS disciplined NTP server 

ntp_shps -a -o -f png 127.127.28.1 0711

![](img/plot_7026.png)
![](img/plot_22516.png)
![](img/plot_22693.png)
![](img/plot_7266.png)

## ntp_shdiff 

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

#### time difference between two NTP server 

ntp_shdiff -a -f png 127.127.28.1 178.189.127.148 0724

![](img/plot_7381.png)


