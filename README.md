# MonitoringLinuxServer
#--------------------------------------------------------------------------#

# START BODY:

#--------------------------------------------------------------------------#

#!/bin/bash

# Define recipient email address
recipient="YourMail@YoeMail.com"

# Function to send an email
send_email() {
  local subject="$1"
  local body="$2"

  echo "$body" | mail -s "$subject" "$recipient"
}

# Function to check if a process is a zombie
is_process_zombie() {
  local pid="$1"
  grep -q "Z" "/proc/$pid/status" 2>/dev/null
}

# Function to check if a process exceeds CPU threshold
is_process_high_cpu() {
  local pid="$1"
  local cpu_threshold="$2"
  local cpu_usage=$(ps -p "$pid" -o %cpu | tail -n 1)
  awk -v threshold="$cpu_threshold" -v usage="$cpu_usage" 'BEGIN {if (usage > threshold) exit 0; else exit 1}'
}

# Main script

# CPU threshold in percentage
cpu_threshold=90

# Get all running process PIDs
pids=$(ps -eo pid=)

# Loop through each process
for pid in $pids; do
  # Check if the process is a zombie
  if is_process_zombie "$pid"; then
    subject="Zombie Process Alert"
    body="Process with PID $pid is in a zombie state."
    send_email "$subject" "$body"
  fi

  # Check if the process exceeds CPU threshold
  if is_process_high_cpu "$pid" "$cpu_threshold"; then
    process_name=$(ps -p "$pid" -o comm=)
    subject="High CPU Usage Alert"
    body="Process '$process_name' with PID $pid is consuming high CPU time."
    send_email "$subject" "$body"
  fi
done
