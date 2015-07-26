# ntpgraph

### make NTP statistic files visible 

## ntp_shps examples 

### usage 

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

![](plot_7026.png)
![](plot_22516.png)
![](plot_22693.png)
![](plot_7266.png)
![](plot_7381.png)


