# MonitoringLinuxServer
a shell script that monitors all running processes in Linux, checks for processes that are either in a zombie state or consume a significant amount of CPU time, and sends an email notification if any issues are found:
how can use:

Replace "YourMail@youemail.com" with the actual email address of the recipient.

In this script, the is_process_zombie function checks if a process with a given PID is in a zombie state. It looks for the "Z" flag in the process status file (/proc/$pid/status). If a process is a zombie, an email is sent with the appropriate subject and body.

The is_process_high_cpu function checks if a process with a given PID exceeds the CPU threshold specified. It retrieves the CPU usage percentage for the process using ps command and compares it with the threshold. If the CPU usage is higher than the threshold, an email is sent with the appropriate subject and body.

The main script first defines the CPU threshold (e.g., 90% in this example). It then retrieves all running process PIDs using ps command. For each PID, it checks if the process is a zombie or has high CPU usage, and sends an email notification if any issues are found.

You can save this script in a file, make it executable with chmod +x script_name.sh, and run it periodically using a cron job or any other scheduling mechanism to monitor the processes and receive email notifications for any issues.
