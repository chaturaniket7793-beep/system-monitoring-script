#!/bin/bash

#  Name: Aniket Vasant chatur
#  Date: 08-07-2026
#  modified date:-08-07-2026
#  Description:System Monitoring and Alerting Script
#  usage: To automate task .

# monitor disk usage and display warnings beyond a defined threshold

exceed=80
logfile="logfile"

echo " =============== Monitoring disk usage ================== "

usage=$( df / | awk 'NR==2 print{$5}' tr -d '%' )
        echo "Disk Usage: {DISK_USAGE}%"


if ["$usage" -ge "$exceed" ];then
        echo " Disk usage is above utilization "
else
        echo " Disk usage is normal "
fi

echo "$(date): ALERT - Disk utilization is ${usage}%" >> "$logfile"


# monitor memory usage and alert when usage is high

usage=$(free -l | awk '/Mem:/ {printf "%.0f", $3/$2*100}')

if [ "$usage" -ge "$exceed" ]; then
    echo "Memory usage is high: ${usage}%"

else
    echo "Memory usage is normal: ${usage}%"
fi

echo "$(date): ALERT - Memory utilization is ${usage}%" >> "$logfile"

# Display top cpu or memory-consuming processes

echo "===== CPU-consuming Processes ====="
ps -eo pid,user,%cpu,comm --sort=-%cpu | head -6


echo "======= memory-consuming Processes ===== "
ps -eo pid,user,%mem,comm --sort=-%mem | head -6


echo "$(date): ALERT - cpu_memory consumtion is ${usage}%" >> "$logfile"
