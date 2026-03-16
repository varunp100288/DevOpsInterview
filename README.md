DEVOPS INTERVIEW QUESTIONS



# Top 200 DevOps Interview Questions and Answers

This README contains **200 DevOps interview questions with answers, detailed explanations, and useful commands**.
It focuses on **Linux, Bash, Docker, Jenkins, Kubernetes, and AWS services (EC2, ECR, EKS, IAM, CloudWatch)**.
It intentionally **excludes scenario-based questions** and progresses from **beginner to advanced**, suitable for **4+ years of DevOps experience**.

## How to Use This File

- Read category by category.
- Revise the commands and try them in a lab.
- Focus on understanding the concept first, then the command, then the real-world DevOps relevance.

## Contents

- **Linux Questions (1-39)**
- **Bash Questions (40-64)**
- **Docker Questions (65-94)**
- **Jenkins Questions (95-119)**
- **Kubernetes Questions (120-154)**
- **AWS Questions (155-200)**


---

## Linux Questions (1-39)


### 1. What is Linux and why is it important in DevOps?

**Answer:** Linux is an open-source operating system widely used for servers, cloud workloads, containers, and automation platforms. In DevOps, most CI/CD servers, container runtimes, Kubernetes nodes, and cloud VMs run on Linux. A DevOps engineer must understand Linux because deployments, troubleshooting, permissions, services, networking, and logs are usually managed there.

**Useful commands:**
```bash
uname -a
cat /etc/os-release
uptime
```


### 2. What is the difference between the Linux kernel and a Linux distribution?

**Answer:** The kernel is the core component that manages CPU, memory, devices, and processes. A distribution packages the kernel together with user-space tools, package managers, libraries, and defaults. Ubuntu, RHEL, Rocky Linux, and Debian are distributions built around the Linux kernel.

**Useful commands:**
```bash
uname -r
cat /etc/os-release
```


### 3. What are the main Linux filesystem directories and their purpose?

**Answer:** /etc stores configuration, /var stores changing data such as logs, /home stores user data, /usr stores binaries and libraries, /bin and /sbin provide essential commands, /tmp is for temporary files, /opt is for optional software, and /proc exposes process and kernel information. In interviews, connect directories with real operations like editing configs in /etc or checking logs in /var/log.

**Useful commands:**
```bash
ls /
ls /etc
ls /var/log
```


### 4. How do you check CPU, memory, disk, and uptime on a Linux server?

**Answer:** A DevOps engineer should quickly inspect system health using standard commands. CPU and load can be checked with top or uptime, memory with free, disk with df, and detailed block devices with lsblk. These are first-line troubleshooting commands during outages or performance checks.

**Useful commands:**
```bash
uptime
top
free -h
df -h
lsblk
```


### 5. What is the difference between a process and a thread?

**Answer:** A process is an independent running program with its own memory space, while a thread is a lightweight execution unit within a process. Threads share the same process memory, which makes them faster for communication but also more sensitive to concurrency problems. In Linux troubleshooting, you usually inspect the process first and then thread behavior if the application is multithreaded.

**Useful commands:**
```bash
ps -ef
top -H -p <PID>
```


### 6. How do you list running processes in Linux?

**Answer:** The most common command is ps -ef for a full snapshot and top or htop for live monitoring. You can filter processes with grep, or sort by resource usage. This is frequently asked because identifying the right PID is required before debugging, restarting, or tracing services.

**Useful commands:**
```bash
ps -ef
ps aux | grep nginx
top
```


### 7. How do you stop, kill, and manage a process?

**Answer:** Use kill with a signal to ask a process to stop. SIGTERM is graceful and should be tried first, while SIGKILL forcefully stops the process and should be used only when necessary. pkill and killall can target by name, but they must be used carefully to avoid killing the wrong processes.

**Useful commands:**
```bash
kill -15 <PID>
kill -9 <PID>
pkill nginx
killall java
```


### 8. What is the difference between foreground and background processes?

**Answer:** A foreground process is attached to the terminal and blocks it until completion. A background process runs without occupying the terminal, allowing you to continue other work. Jobs, bg, fg, nohup, and process supervisors like systemd are used to control them.

**Useful commands:**
```bash
sleep 300 &
jobs
fg %1
nohup ./app.sh > app.log 2>&1 &
```


### 9. What is systemd and why is it important?

**Answer:** systemd is the init and service manager on most modern Linux distributions. It is used to start, stop, enable, disable, and inspect services and boot targets. In DevOps, most application runtimes, reverse proxies, monitoring agents, and CI runners are managed through systemd.

**Useful commands:**
```bash
systemctl status nginx
systemctl start nginx
systemctl enable docker
journalctl -u nginx
```


### 10. How do you check the status of a service?

**Answer:** Use systemctl status for the current service state and journalctl for logs. status shows whether the service is active, failed, or restarting, while logs reveal why. This combination is a standard production troubleshooting flow.

**Useful commands:**
```bash
systemctl status ssh
journalctl -u ssh -n 50 --no-pager
```


### 11. How do file permissions work in Linux?

**Answer:** Linux permissions are expressed as read, write, and execute for owner, group, and others. They protect files, directories, scripts, and keys from unauthorized access. A DevOps engineer must understand them to manage SSH keys, deployment artifacts, and service configs safely.

**Useful commands:**
```bash
ls -l
chmod 644 file.txt
chmod 755 script.sh
```


### 12. What is the difference between chmod, chown, and chgrp?

**Answer:** chmod changes permissions, chown changes ownership, and chgrp changes the group. These are often used together after deployments or when fixing application access issues. Misuse can break web servers, CI agents, or mounted volumes.

**Useful commands:**
```bash
chmod 600 ~/.ssh/id_rsa
sudo chown ubuntu:ubuntu app.log
sudo chgrp docker /var/run/docker.sock
```


### 13. What do permission modes like 777, 755, and 644 mean?

**Answer:** Each digit represents owner, group, and others. 7 means read, write, execute; 5 means read and execute; 4 means read only. 755 is common for executable scripts and directories, 644 for normal files, and 777 is generally unsafe because it grants everyone full access.

**Useful commands:**
```bash
chmod 755 deploy.sh
chmod 644 config.yml
```


### 14. What is a hard link vs a soft link?

**Answer:** A hard link points directly to the same inode, so it still works even if the original filename is removed. A soft link is a shortcut that points to a path and breaks if the target file is deleted or moved. Soft links are more common in real systems for configs and versioned releases.

**Useful commands:**
```bash
ln file.txt hardlink.txt
ln -s /opt/app/current/config.yml config-link.yml
ls -li
```


### 15. What is an inode?

**Answer:** An inode stores metadata about a file such as ownership, permissions, timestamps, and disk block pointers. The filename itself is stored separately in the directory entry. Understanding inodes helps explain hard links, deleted-but-still-open files, and inode exhaustion.

**Useful commands:**
```bash
ls -i
df -i
```


### 16. How do you find files in Linux?

**Answer:** Use find for flexible path-based searches and locate for fast indexed searches. find is preferred in administration because it can filter by name, size, time, owner, and execute actions. Interviewers ask this because file discovery is a daily operations task.

**Useful commands:**
```bash
find /var/log -name "*.log"
find / -type f -size +100M 2>/dev/null
locate nginx.conf
```


### 17. How do you search inside files?

**Answer:** Use grep for text pattern matching, often with flags like -i for case-insensitive, -r for recursive, and -n for line numbers. grep is heavily used in logs, configs, manifests, and CI outputs. It is one of the most practical Linux commands in DevOps work.

**Useful commands:**
```bash
grep -i error /var/log/syslog
grep -rn "image:" .
grep -E "WARN|ERROR" app.log
```


### 18. How do you monitor log files in real time?

**Answer:** tail -f follows a file as new lines are appended. It is useful during deployments, service restarts, or debugging application startup. For systemd-managed services, journalctl -f gives real-time logs directly from the journal.

**Useful commands:**
```bash
tail -f /var/log/nginx/access.log
journalctl -u docker -f
```


### 19. What is the difference between df and du?

**Answer:** df reports filesystem-level free and used disk space, while du reports directory or file-level usage. Use df when a disk seems full, and du when you need to identify what is consuming space. Both are often used together during incident response.

**Useful commands:**
```bash
df -h
du -sh /var/log
du -sh * | sort -h
```


### 20. How do you archive and compress files in Linux?

**Answer:** tar is used to bundle files, while gzip, bzip2, and xz compress them. Backups, log shipping, artifacts, and release bundles often use tar.gz. A DevOps engineer should know both creation and extraction commands.

**Useful commands:**
```bash
tar -czf backup.tar.gz /etc/nginx
tar -xzf backup.tar.gz
gzip app.log
```


### 21. What is the difference between apt, yum, and dnf?

**Answer:** These are package managers used by different distributions. apt is common on Debian and Ubuntu, while yum and dnf are used on RHEL-based systems. They install, update, and remove packages, and are critical in server provisioning and AMI/container image builds.

**Useful commands:**
```bash
sudo apt update && sudo apt install nginx -y
sudo yum install git -y
sudo dnf install jq -y
```


### 22. How do you check open ports on a Linux server?

**Answer:** You can use ss, netstat on older systems, lsof, or nmap from another host. During troubleshooting, you check whether a service is listening and on which interface. This is essential for debugging connectivity issues.

**Useful commands:**
```bash
ss -tulnp
sudo lsof -i -P -n | grep LISTEN
nmap -p 22,80,443 <IP>
```


### 23. How do you test network connectivity from Linux?

**Answer:** Use ping for reachability, curl for HTTP(S), nc or telnet for TCP port testing, and traceroute for path analysis. In DevOps work, you often need to verify DNS resolution, routing, firewall rules, or service exposure.

**Useful commands:**
```bash
ping -c 4 google.com
curl -I https://example.com
nc -zv 127.0.0.1 8080
traceroute 8.8.8.8
```


### 24. How do you check DNS resolution in Linux?

**Answer:** Use dig, nslookup, or host to resolve names and inspect DNS responses. This helps when applications fail because of incorrect records, private hosted zones, or resolver issues. Always verify that the returned IP is the expected one.

**Useful commands:**
```bash
dig example.com
nslookup example.com
host example.com
```


### 25. How do you schedule jobs in Linux?

**Answer:** cron is the classic scheduler for recurring jobs, while at is used for one-time scheduling. Common examples include backups, cleanup scripts, certificate checks, and report generation. On modern systems, systemd timers can also be used.

**Useful commands:**
```bash
crontab -e
crontab -l
echo "date >> /tmp/time.log" | at now + 1 minute
```


### 26. What is a crontab entry format?

**Answer:** The five main fields are minute, hour, day of month, month, and day of week, followed by the command. For example, 0 2 * * * means run daily at 2 AM. A DevOps engineer should also remember to redirect output to logs for observability.

**Useful commands:**
```bash
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
```


### 27. How do you view environment variables?

**Answer:** Use printenv, env, or echo with a specific variable. Environment variables are heavily used in application configuration, CI/CD, and containers. Always verify them before assuming an application got the correct config.

**Useful commands:**
```bash
printenv
env | sort
echo $PATH
```


### 28. What is the PATH variable?

**Answer:** PATH is a colon-separated list of directories the shell searches when you run a command without a full path. Incorrect PATH values cause 'command not found' errors or can make the wrong binary run. This matters in CI pipelines and multi-version environments.

**Useful commands:**
```bash
echo $PATH
which kubectl
type -a python
```


### 29. How do you switch users or run commands as another user?

**Answer:** Use su for switching shells and sudo for privileged execution. sudo is preferred because it is auditable and can be scoped precisely. In production, least privilege access is a security best practice.

**Useful commands:**
```bash
sudo -i
sudo -u jenkins whoami
su - ubuntu
```


### 30. What is sudo and why is it preferred over direct root login?

**Answer:** sudo allows controlled privilege escalation with logging and policy enforcement. It reduces the need to share the root password and supports least privilege. Many security baselines disable direct root SSH access and rely on sudo instead.

**Useful commands:**
```bash
sudo visudo
sudo systemctl restart nginx
```


### 31. How do you check memory usage and swap?

**Answer:** Use free -h for a quick overview, vmstat for system activity, and top for live inspection. High swap usage may indicate memory pressure, poor sizing, or leaks. Always correlate memory symptoms with application logs and OOM events.

**Useful commands:**
```bash
free -h
vmstat 1 5
top
```


### 32. How do you identify high CPU usage?

**Answer:** Use top, htop, ps sorted by CPU, or pidstat for more detail. High CPU may come from an application bug, infinite loop, high load, or compression/GC-heavy activity. The next step is to identify the process and correlate with logs or recent changes.

**Useful commands:**
```bash
top
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head
pidstat 1
```


### 33. How do you identify large files consuming disk?

**Answer:** Use du with sorting and find by size. Logs, core dumps, container layers, and backup archives are common reasons disks fill up. Do not delete blindly; confirm the file purpose first.

**Useful commands:**
```bash
du -ah /var | sort -h | tail -20
find / -type f -size +500M 2>/dev/null
```


### 34. What is the /proc filesystem?

**Answer:** /proc is a virtual filesystem that exposes kernel and process information. It does not store regular files on disk; instead it presents live system state. It is useful for inspecting CPU info, memory info, mounts, and process details.

**Useful commands:**
```bash
cat /proc/cpuinfo | head
cat /proc/meminfo | head
cat /proc/<PID>/status
```


### 35. What is the difference between load average and CPU usage?

**Answer:** CPU usage shows how busy CPUs are right now, while load average reflects the number of runnable or uninterruptible tasks over time. A system can show high load even when CPU is not fully utilized, for example due to disk I/O waits. Interviewers like this because it tests real understanding, not just command memorization.

**Useful commands:**
```bash
uptime
top
```


### 36. What are signals in Linux?

**Answer:** Signals are software interrupts sent to processes to control behavior, such as termination, reload, or interrupt. SIGTERM asks a process to exit gracefully, SIGKILL forces termination, SIGHUP is often used to reload configs, and SIGINT is what Ctrl+C sends. Knowing common signals helps in safe operations.

**Useful commands:**
```bash
kill -l
kill -HUP <PID>
kill -TERM <PID>
```


### 37. How do you securely manage SSH private keys on Linux?

**Answer:** Store keys with restrictive permissions, usually 600 for the private key and 700 for the .ssh directory. Never commit keys into repositories or put them into world-readable locations. Use an SSH agent, secret manager, or ephemeral credentials in CI where possible.

**Useful commands:**
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
ssh -i ~/.ssh/id_rsa ubuntu@server
```


### 38. How do you check recent login history and user activity?

**Answer:** Use last for login history, who or w for current sessions, and lastlog for the latest login per account. These commands help in auditing, security checks, and basic incident investigation.

**Useful commands:**
```bash
last
who
w
lastlog | head
```


### 39. What is log rotation and why is it needed?

**Answer:** Log rotation prevents logs from growing indefinitely and consuming disk space. Tools like logrotate rotate, compress, and retain logs according to policy. This is essential for long-running services and servers with limited disk capacity.

**Useful commands:**
```bash
cat /etc/logrotate.conf
ls /etc/logrotate.d
logrotate -d /etc/logrotate.conf
```


---

## Bash Questions (40-64)


### 40. What is Bash?

**Answer:** Bash is a command shell and scripting language commonly used for automation on Linux systems. DevOps engineers use it for provisioning, deployments, CI pipeline steps, health checks, and utility scripts. It is popular because it is available almost everywhere and integrates naturally with system commands.

**Useful commands:**
```bash
bash --version
```


### 41. What is the difference between a shell and a terminal?

**Answer:** A shell is the command interpreter such as bash or zsh, while a terminal is the interface program that lets you interact with the shell. You can run different shells inside the same terminal application. This distinction matters when debugging startup files or scripting behavior.

**Useful commands:**
```bash
echo $SHELL
ps -p $$
```


### 42. How do you make a Bash script executable?

**Answer:** Add a shebang line, save the file, and set execute permission with chmod. The shebang tells the OS which interpreter to use. Without execute permission, the file can still be run by explicitly passing it to bash.

**Useful commands:**
```bash
cat > script.sh <<'EOF'
#!/bin/bash
echo Hello
EOF
chmod +x script.sh
./script.sh
```


### 43. What is a shebang and why is it used?

**Answer:** The shebang is the first line of a script, such as #!/bin/bash. It tells the system which interpreter should execute the script. This makes script behavior predictable across environments.

**Useful commands:**
```bash
#!/bin/bash
#!/usr/bin/env bash
```


### 44. How do you define and use variables in Bash?

**Answer:** Variables are assigned without spaces around the equals sign and referenced with a leading dollar sign. Quoting is important when values contain spaces or special characters. Environment variables can be exported for child processes.

**Useful commands:**
```bash
name="devops"
echo "$name"
export ENV=prod
```


### 45. What is the difference between local and environment variables?

**Answer:** A local shell variable exists only in the current shell, while an exported environment variable is inherited by child processes. This matters when a script or application cannot see a variable you set interactively.

**Useful commands:**
```bash
foo=bar
export foo
env | grep foo
```


### 46. What is the difference between single quotes and double quotes in Bash?

**Answer:** Single quotes preserve the literal value of characters, while double quotes allow variable expansion and command substitution. Correct quoting prevents bugs related to spaces, glob expansion, and unexpected substitutions.

**Useful commands:**
```bash
name="world"
echo 'hello $name'
echo "hello $name"
```


### 47. How do you take input in a Bash script?

**Answer:** Use read to capture user input into variables. For non-interactive automation, scripts more often accept positional parameters or environment variables. Interactive input is useful for quick utilities but less common in CI/CD jobs.

**Useful commands:**
```bash
read -p "Enter name: " name
echo "Hello $name"
```


### 48. What are positional parameters in Bash?

**Answer:** These are arguments passed to the script and accessed as $1, $2, and so on. $# gives the count, $@ expands to all arguments, and $0 is the script name. They are heavily used in reusable deployment scripts.

**Useful commands:**
```bash
echo "Script: $0"
echo "First: $1"
echo "Count: $#"
echo "All: $@"
```


### 49. What is the difference between $* and $@?

**Answer:** Both represent all positional parameters, but quoted "$@" preserves argument boundaries while "$*" joins them into a single string. In real scripts, "$@" is usually the correct and safer choice when forwarding arguments.

**Useful commands:**
```bash
printf '<%s>
' "$@"
```


### 50. How do you write if, elif, and else conditions in Bash?

**Answer:** Bash supports conditional execution using test expressions inside square brackets or double brackets. Double brackets are generally safer and more expressive. Conditions are used in deployment logic, validation, and branching based on environment values.

**Useful commands:**
```bash
if [[ -f /etc/passwd ]]; then
  echo "exists"
elif [[ -d /tmp ]]; then
  echo "dir"
else
  echo "none"
fi
```


### 51. How do you use loops in Bash?

**Answer:** for loops are used when iterating over a list, while while loops are used when repeating until a condition changes or reading line by line. Loops are common in automation over hosts, files, namespaces, or URLs.

**Useful commands:**
```bash
for i in 1 2 3; do echo "$i"; done

count=1
while [[ $count -le 3 ]]; do
  echo "$count"
  ((count++))
done
```


### 52. What is command substitution?

**Answer:** Command substitution captures the output of a command and assigns or embeds it into another command. The modern form uses $(...), which is clearer and easier to nest than backticks.

**Useful commands:**
```bash
today=$(date +%F)
echo "Today is $today"
```


### 53. How do you perform arithmetic in Bash?

**Answer:** You can use double parentheses for integer arithmetic or the let command. Bash arithmetic is suitable for counters and simple calculations, but not floating-point math.

**Useful commands:**
```bash
a=5
b=3
echo $((a+b))
((a++))
echo "$a"
```


### 54. What is the difference between exit code 0 and non-zero in Bash?

**Answer:** An exit code of 0 means success and any non-zero value indicates failure. CI/CD pipelines and shell conditionals rely on exit codes to decide whether to continue or stop. Proper scripts return meaningful exit codes on validation failures.

**Useful commands:**
```bash
grep root /etc/passwd
echo $?
grep nosuchuser /etc/passwd
echo $?
```


### 55. What does set -euo pipefail mean?

**Answer:** set -e exits on command failure, -u treats unset variables as errors, and pipefail makes a pipeline fail if any command in it fails. Together they make scripts safer and reduce silent failures. This is a strong Bash best practice for DevOps scripts.

**Useful commands:**
```bash
set -euo pipefail
```


### 56. How do you redirect stdout and stderr?

**Answer:** Use > to redirect stdout, 2> for stderr, and &> or > file 2>&1 for both. Redirection is critical in cron jobs, scripts, and CI to capture logs. Appending uses >>.

**Useful commands:**
```bash
command > out.log
command 2> err.log
command > all.log 2>&1
command >> app.log 2>&1
```


### 57. What is a pipe in Bash?

**Answer:** A pipe sends the stdout of one command as the stdin of another. Pipes let you combine simple tools into powerful workflows, which is a core shell philosophy and common in debugging and reporting.

**Useful commands:**
```bash
ps -ef | grep nginx | awk '{print $2}'
```


### 58. How do you use grep, awk, and sed together?

**Answer:** grep filters lines, awk extracts or transforms columns and fields, and sed performs stream editing or substitutions. These tools are foundational for text processing in logs, manifests, config files, and command outputs.

**Useful commands:**
```bash
kubectl get pods -A | grep CrashLoopBackOff
df -h | awk 'NR==1 || /\/$/'
sed -n '1,5p' file.txt
```


### 59. What is a function in Bash?

**Answer:** A function groups reusable logic in a script. It improves readability, reduces duplication, and makes scripts easier to maintain. Production scripts often wrap logging, validation, retries, or cleanup logic into functions.

**Useful commands:**
```bash
hello() {
  echo "Hello, $1"
}
hello DevOps
```


### 60. How do you parse a file line by line in Bash?

**Answer:** A while read loop is the usual pattern. Use IFS= and -r to preserve spacing and backslashes correctly. This is common when processing host lists, URLs, or manifest fragments.

**Useful commands:**
```bash
while IFS= read -r line; do
  echo "$line"
done < hosts.txt
```


### 61. What is xargs used for?

**Answer:** xargs builds command arguments from stdin. It is useful for batching operations across command results, such as deleting files, inspecting containers, or parallel execution with care.

**Useful commands:**
```bash
find . -name "*.log" | xargs ls -l
cat pods.txt | xargs -n1 kubectl get pod
```


### 62. How do you debug a Bash script?

**Answer:** Run the script with bash -x or use set -x inside it to trace commands as they execute. You can also echo variable values and add strict modes. Debugging shell scripts is important because quoting and expansion errors can be subtle.

**Useful commands:**
```bash
bash -x deploy.sh
set -x
```


### 63. How do you check whether a file or directory exists in Bash?

**Answer:** Use test expressions like -f for files, -d for directories, and -e for either. Input validation using these checks prevents deployment scripts from failing later in less clear ways.

**Useful commands:**
```bash
if [[ -f config.yml ]]; then echo "file"; fi
if [[ -d /opt/app ]]; then echo "dir"; fi
```


### 64. How do you handle arguments with spaces safely in Bash?

**Answer:** Always quote variables and prefer arrays when storing multiple arguments. Unquoted variables can split unexpectedly and break paths, URLs, or labels. This is one of the most common Bash mistakes.

**Useful commands:**
```bash
file="my file.txt"
cp "$file" /tmp/
args=("kubectl" "get" "pods" "-A")
"${args[@]}"
```


---

## Docker Questions (65-94)


### 65. What is Docker?

**Answer:** Docker is a container platform that packages an application and its dependencies into an image and runs it as an isolated container. Containers are lightweight compared with VMs because they share the host kernel. In DevOps, Docker standardizes builds, testing, and deployments across environments.

**Useful commands:**
```bash
docker version
docker info
```


### 66. What is the difference between a container and a virtual machine?

**Answer:** A VM virtualizes hardware and runs a full guest OS, while a container isolates processes at the OS level and shares the host kernel. Containers start faster and use fewer resources, making them ideal for microservices and CI workloads. VMs provide stronger isolation and can run different kernels.

**Useful commands:**
```bash
docker ps
docker run -d nginx
```


### 67. What is a Docker image?

**Answer:** A Docker image is a read-only template that contains the application code, runtime, libraries, and metadata needed to run a container. Containers are created from images. Images are typically built from Dockerfiles and stored in registries like Docker Hub or Amazon ECR.

**Useful commands:**
```bash
docker images
docker inspect nginx:latest
```


### 68. What is a Dockerfile?

**Answer:** A Dockerfile is a text file that defines how an image is built. It contains instructions such as FROM, COPY, RUN, WORKDIR, ENV, EXPOSE, and CMD. A good Dockerfile should be deterministic, small, and secure.

**Useful commands:**
```bash
cat Dockerfile
docker build -t myapp:1.0 .
```


### 69. What is the difference between CMD and ENTRYPOINT?

**Answer:** CMD provides default arguments or a default command that can be overridden. ENTRYPOINT defines the main executable and is harder to replace unintentionally. They are often combined so ENTRYPOINT sets the program and CMD sets default parameters.

**Useful commands:**
```bash
docker run myimage
docker run myimage --help
docker inspect myimage | grep -i -E 'Cmd|Entrypoint'
```


### 70. What is the difference between COPY and ADD?

**Answer:** COPY simply copies files from the build context into the image and is preferred for clarity. ADD can also extract local tar archives and fetch remote URLs, but its extra behavior can be surprising. In most production Dockerfiles, COPY is recommended unless ADD is specifically needed.

**Useful commands:**
```bash
COPY . /app
ADD archive.tar.gz /app/
```


### 71. What is the purpose of the FROM instruction?

**Answer:** FROM sets the base image for the build. It defines the starting filesystem and runtime environment. Choosing a minimal and trusted base image reduces size, attack surface, and build time.

**Useful commands:**
```bash
FROM ubuntu:24.04
FROM node:20-alpine
```


### 72. What is a multi-stage Docker build?

**Answer:** A multi-stage build uses multiple FROM statements so you can compile in one stage and copy only the final artifacts into a small runtime stage. This reduces image size and removes unnecessary build tools from production images. It is a best practice for Java, Go, Node, and frontend builds.

**Useful commands:**
```bash
# build stage
FROM node:20 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build

# runtime stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
```


### 73. How do you build an image?

**Answer:** You build an image with docker build and optionally tag it with a repository name and version. The build context should be kept minimal because everything in it may be sent to the Docker daemon.

**Useful commands:**
```bash
docker build -t myorg/myapp:1.0 .
docker build -f Dockerfile.prod -t myapp:prod .
```


### 74. How do you run a container?

**Answer:** Use docker run with options for detached mode, ports, environment variables, volumes, and container name. Understanding docker run shows whether you truly understand the runtime side of containers, not just image creation.

**Useful commands:**
```bash
docker run -d --name web -p 8080:80 nginx
docker run --rm alpine:3.20 echo hello
```


### 75. What is the difference between detached and interactive mode?

**Answer:** Detached mode runs the container in the background, while interactive mode attaches your terminal so you can interact with the process. Interactive mode is common for debugging and detached mode is common for services.

**Useful commands:**
```bash
docker run -it ubuntu bash
docker run -d nginx
```


### 76. How do you inspect logs of a container?

**Answer:** Use docker logs to read stdout and stderr from the main container process. Add -f to follow in real time. If logs are empty, the app may be logging elsewhere or failing before output.

**Useful commands:**
```bash
docker logs web
docker logs -f web
```


### 77. How do you execute a command inside a running container?

**Answer:** Use docker exec, usually with -it for an interactive shell. This is useful for inspecting files, environment variables, network tools, or process state inside the container.

**Useful commands:**
```bash
docker exec -it web sh
docker exec -it web env
```


### 78. What is the difference between expose and publish ports?

**Answer:** EXPOSE in a Dockerfile is only metadata indicating the intended container port. Publishing with -p or --publish actually maps a host port to a container port so external traffic can reach it. Many beginners confuse these two.

**Useful commands:**
```bash
docker run -d -p 8080:80 nginx
```


### 79. What is a Docker volume?

**Answer:** A volume is persistent storage managed by Docker and mounted into containers. Volumes outlive container deletion and are used for databases, uploads, and stateful data. Bind mounts are host paths, while named volumes are managed abstractions.

**Useful commands:**
```bash
docker volume create appdata
docker run -d -v appdata:/var/lib/mysql mysql:8
```


### 80. What is the difference between a bind mount and a named volume?

**Answer:** A bind mount maps a specific host path into the container, while a named volume is created and managed by Docker. Bind mounts are useful in local development; named volumes are cleaner and more portable for data persistence.

**Useful commands:**
```bash
docker run -v /host/path:/app/data myimage
docker run -v myvolume:/app/data myimage
```


### 81. How do Docker networks work?

**Answer:** Docker networks provide isolated communication between containers. The default bridge network allows local container connectivity, while user-defined bridge networks provide better DNS-based service discovery. Networking is central when connecting app, database, and proxy containers.

**Useful commands:**
```bash
docker network ls
docker network create app-net
docker run -d --network app-net --name db postgres
```


### 82. What is the difference between bridge, host, and none network modes?

**Answer:** bridge is the default isolated network mode, host shares the host's network stack, and none disables networking entirely. bridge is most common, host can improve performance or simplify low-level networking, and none is used in special restricted cases.

**Useful commands:**
```bash
docker run --network bridge nginx
docker run --network host nginx
docker run --network none alpine
```


### 83. How do you remove stopped containers, unused images, and unused volumes?

**Answer:** Docker accumulates old resources over time, especially on CI agents and dev boxes. prune commands are useful for cleanup, but they should be used carefully because they can remove resources still needed by other workflows.

**Useful commands:**
```bash
docker container prune -f
docker image prune -a -f
docker volume prune -f
docker system prune -a -f
```


### 84. What is a .dockerignore file?

**Answer:** .dockerignore excludes files from the build context. It reduces build time, avoids sending secrets or unnecessary files, and can improve cache behavior. Typical entries include .git, node_modules, logs, and local artifacts.

**Useful commands:**
```bash
cat > .dockerignore <<'EOF'
.git
node_modules
*.log
.env
EOF
```


### 85. How does Docker image layer caching work?

**Answer:** Each Dockerfile instruction creates a layer that can be reused if nothing relevant changed. Ordering instructions well, such as copying dependency files before source files, improves cache reuse and speeds builds. Poor ordering causes full rebuilds unnecessarily.

**Useful commands:**
```bash
# Better cache pattern for Node
COPY package*.json ./
RUN npm ci
COPY . .
```


### 86. Why should containers run as a non-root user?

**Answer:** Running as root increases the impact of a container compromise. A non-root user reduces privilege and is a widely accepted container security best practice. Many scanners and Kubernetes policies check for this.

**Useful commands:**
```bash
RUN useradd -m appuser
USER appuser
```


### 87. How do you pass environment variables into a container?

**Answer:** You can pass variables at runtime with -e or --env-file, or define defaults in the image with ENV. In production, secrets should come from secure stores or orchestrator-managed secret injection, not hard-coded image values.

**Useful commands:**
```bash
docker run -e APP_ENV=prod myimage
docker run --env-file .env myimage
```


### 88. What is the difference between image tags and digests?

**Answer:** A tag is a mutable human-friendly label like latest or 1.2.0, while a digest is an immutable content-based identifier. For reproducible deployments, digests are safer because tags can be moved to a different image later.

**Useful commands:**
```bash
docker pull nginx:1.27
docker inspect --format='{{index .RepoDigests 0}}' nginx:1.27
```


### 89. How do you push an image to a registry?

**Answer:** First tag the image with the registry/repository name, then authenticate and push. This is a standard CI/CD step before deployment. On AWS, the destination is usually Amazon ECR.

**Useful commands:**
```bash
docker login
docker tag myapp:1.0 myrepo/myapp:1.0
docker push myrepo/myapp:1.0
```


### 90. What is Docker Compose?

**Answer:** Docker Compose defines multi-container applications in a declarative YAML file. It is commonly used for local development and testing stacks such as app, database, cache, and reverse proxy together. In interviews, explain that Compose is not a replacement for Kubernetes in large-scale production.

**Useful commands:**
```bash
docker compose up -d
docker compose ps
docker compose logs -f
```


### 91. What is the difference between Docker Compose and Kubernetes?

**Answer:** Compose is simpler and mainly aimed at local or small deployments, while Kubernetes is a full orchestration platform for scheduling, scaling, service discovery, self-healing, and rolling updates. Compose is great for development; Kubernetes is designed for clustered production operations.

**Useful commands:**
```bash
docker compose up -d
kubectl get pods -A
```


### 92. How do you check resource usage of running containers?

**Answer:** Use docker stats for live CPU, memory, network, and block I/O metrics. This is useful for identifying noisy containers, leaks, or sizing issues before moving workloads into orchestrators.

**Useful commands:**
```bash
docker stats
docker stats web db
```


### 93. What happens when the main process in a container exits?

**Answer:** The container stops because the container lifecycle is tied to the main PID 1 process. This is why a container should run the actual application in the foreground rather than starting a background daemon and exiting.

**Useful commands:**
```bash
docker run --rm alpine sh -c 'echo done'
docker ps -a
```


### 94. Why is it recommended to use the exec form of CMD or ENTRYPOINT?

**Answer:** The exec form uses JSON array syntax and avoids an extra shell process. It handles signals more correctly, which is important for graceful shutdown in containers and orchestrators. The shell form can interfere with signal propagation.

**Useful commands:**
```bash
CMD ["nginx", "-g", "daemon off;"]
# instead of
# CMD nginx -g 'daemon off;'
```


---

## Jenkins Questions (95-119)


### 95. What is Jenkins?

**Answer:** Jenkins is an open-source automation server used to build, test, and deploy software. It supports pipelines as code, plugins, distributed builds, and integrations with SCM, artifact repositories, cloud providers, and chat tools. In DevOps, Jenkins commonly orchestrates CI/CD workflows.

**Useful commands:**
```bash
jenkins --version
```


### 96. What is the difference between CI and CD?

**Answer:** Continuous Integration means frequently merging code and automatically validating it through builds and tests. Continuous Delivery means keeping code in a releasable state, while Continuous Deployment goes further and releases automatically to production after checks pass. Jenkins can support all three depending on the pipeline design.

**Useful commands:**
```bash
# Example pipeline stages
Build -> Test -> Package -> Deploy
```


### 97. What is a Jenkins pipeline?

**Answer:** A pipeline is a codified sequence of automation stages defined in a Jenkinsfile. It makes the CI/CD workflow version-controlled, repeatable, and reviewable. Pipelines can be declarative or scripted.

**Useful commands:**
```bash
pipeline {
  agent any
  stages {
    stage('Build') { steps { sh 'echo build' } }
  }
}
```


### 98. What is the difference between declarative and scripted pipeline?

**Answer:** Declarative pipeline is more structured and opinionated, making it easier to read and standardize. Scripted pipeline is more flexible because it is closer to raw Groovy, but it can become harder to maintain. Most teams prefer declarative for standard CI/CD workflows.

**Useful commands:**
```bash
pipeline { agent any stages { stage('Test'){ steps { sh 'make test' } } } }
```


### 99. What is a Jenkinsfile?

**Answer:** A Jenkinsfile is a text file stored in source control that defines the Jenkins pipeline. Keeping the pipeline with the application code ensures changes to build and deploy logic are reviewed and versioned together.

**Useful commands:**
```bash
cat Jenkinsfile
```


### 100. What is an agent in Jenkins?

**Answer:** An agent is a worker node that executes pipeline jobs. The controller coordinates work, while agents provide compute environments with the needed tools. This improves scalability and isolation.

**Useful commands:**
```bash
node('docker') {
  sh 'docker version'
}
```


### 101. Why do teams use multiple Jenkins agents?

**Answer:** Multiple agents let teams parallelize builds, separate workloads by tooling, isolate risky jobs, and scale compute as needed. For example, one agent may have Docker, another Java/Maven, and another kubectl or Terraform.

**Useful commands:**
```bash
pipeline {
  agent none
  stages {
    stage('Build') { agent { label 'maven' } steps { sh 'mvn -v' } }
    stage('Deploy') { agent { label 'k8s' } steps { sh 'kubectl version --client' } }
  }
}
```


### 102. What are Jenkins stages, steps, and post actions?

**Answer:** Stages are major phases like Build, Test, and Deploy. Steps are the individual actions inside a stage, such as sh, git, or archiveArtifacts. post actions define behavior after success, failure, or always, often used for cleanup or notifications.

**Useful commands:**
```bash
post {
  always { echo 'cleanup' }
  failure { echo 'notify failure' }
}
```


### 103. How do you trigger Jenkins jobs?

**Answer:** Jobs can be triggered manually, by SCM webhooks, polling, schedules, upstream jobs, or APIs. Webhooks are preferred over polling because they are more immediate and efficient.

**Useful commands:**
```bash
triggers {
  cron('H 2 * * *')
}
```


### 104. What is the purpose of webhooks in Jenkins?

**Answer:** Webhooks notify Jenkins immediately when events happen in GitHub, GitLab, or Bitbucket, such as a push or pull request. This enables near real-time CI without constant polling.

**Useful commands:**
```bash
# Git provider sends POST request to Jenkins webhook endpoint
```


### 105. How do you store secrets in Jenkins?

**Answer:** Secrets should be stored in Jenkins Credentials, not hard-coded in Jenkinsfiles or repositories. Pipelines can inject them securely using credentials binding. For stronger security, many teams integrate external secret managers.

**Useful commands:**
```bash
withCredentials([string(credentialsId: 'token-id', variable: 'TOKEN')]) {
  sh 'echo "$TOKEN" | wc -c'
}
```


### 106. What is credentials binding in Jenkins?

**Answer:** It is a mechanism that exposes stored credentials to pipeline steps as environment variables or files during execution. It reduces accidental exposure and centralizes secret handling.

**Useful commands:**
```bash
withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
  sh 'echo "$USER"'
}
```


### 107. How do you archive artifacts in Jenkins?

**Answer:** archiveArtifacts stores build outputs so they can be downloaded later or passed along in the pipeline history. This is useful for JAR files, reports, logs, or packaged bundles.

**Useful commands:**
```bash
archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
```


### 108. What is fingerprinting in Jenkins?

**Answer:** Fingerprinting tracks artifact usage across jobs and builds by calculating hashes. It helps with traceability, showing which downstream builds consumed which upstream artifacts.

**Useful commands:**
```bash
archiveArtifacts artifacts: 'build/*.zip', fingerprint: true
```


### 109. How do you run parallel stages in Jenkins?

**Answer:** Parallel stages let multiple tasks run at the same time, reducing total pipeline duration. Common examples are running unit tests, linting, and security scans in parallel.

**Useful commands:**
```bash
stage('Parallel Checks') {
  parallel {
    stage('Unit Tests') { steps { sh 'make test' } }
    stage('Lint') { steps { sh 'make lint' } }
  }
}
```


### 110. What is the purpose of the agent any directive?

**Answer:** agent any tells Jenkins to run the pipeline or stage on any available agent. It is convenient for general jobs, but specialized workloads should target labels or containerized agents with the required tools.

**Useful commands:**
```bash
pipeline {
  agent any
  stages { stage('Hello'){ steps { sh 'hostname' } } }
}
```


### 111. How do you restrict a stage to a specific agent label?

**Answer:** Use an agent block with a label inside the stage or top-level pipeline. This ensures the stage runs only where its tools or permissions are available.

**Useful commands:**
```bash
stage('Docker Build') {
  agent { label 'docker' }
  steps { sh 'docker build -t app .' }
}
```


### 112. What is the difference between freestyle jobs and pipelines?

**Answer:** Freestyle jobs are configured mainly through the Jenkins UI and are harder to version and review. Pipelines are defined as code, more reusable, and better suited for modern CI/CD practices. For production teams, pipelines are usually preferred.

**Useful commands:**
```bash
# Freestyle: UI configuration
# Pipeline: Jenkinsfile in repo
```


### 113. How do you implement input approval in a pipeline?

**Answer:** The input step pauses the pipeline and waits for human approval before continuing. It is commonly used before production deployment or destructive actions.

**Useful commands:**
```bash
stage('Approve Prod') {
  steps {
    input message: 'Deploy to production?'
  }
}
```


### 114. What is the purpose of post { always {} } in Jenkins?

**Answer:** It ensures cleanup or reporting runs regardless of success or failure. This is useful for collecting logs, publishing reports, or removing temporary resources.

**Useful commands:**
```bash
post {
  always {
    junit 'reports/*.xml'
  }
}
```


### 115. How do you parameterize a Jenkins job?

**Answer:** Parameters let one pipeline handle different environments, versions, flags, or branches. This improves reusability and reduces duplicate jobs.

**Useful commands:**
```bash
parameters {
  choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Target environment')
  string(name: 'TAG', defaultValue: 'latest', description: 'Image tag')
}
```


### 116. What is Blue Ocean in Jenkins?

**Answer:** Blue Ocean is a modern Jenkins UI focused on visualizing pipelines and improving user experience. It simplifies viewing stages, logs, and pipeline status, though some teams still use the classic UI extensively.

**Useful commands:**
```bash
# Installed as Jenkins plugin
```


### 117. How do you clean the workspace in Jenkins?

**Answer:** Workspaces can accumulate stale files that affect builds. Cleaning before or after builds helps reproducibility, especially on long-lived agents.

**Useful commands:**
```bash
cleanWs()
deleteDir()
```


### 118. What are shared libraries in Jenkins?

**Answer:** Shared libraries let you centralize reusable pipeline code such as standard build, test, notification, or deployment functions. They reduce duplication and standardize practices across many repositories.

**Useful commands:**
```bash
@Library('my-shared-lib') _
deployApp(env: 'staging')
```


### 119. How do you make Jenkins highly available or more production-ready?

**Answer:** A common approach is to keep the controller stable, externalize backups, use agents for workloads, store configs as code, and integrate with external databases or cloud storage where appropriate. Jenkins itself is not typically run as an active-active control plane the way Kubernetes is; resilience comes from backups, restore procedures, and reproducible configuration. Production readiness also requires RBAC, credential hygiene, monitoring, and plugin governance.

**Useful commands:**
```bash
# Practices
# Backup JENKINS_HOME
# Use Configuration as Code
# Use ephemeral agents
# Monitor disk, memory, queue, and executor usage
```


---

## Kubernetes Questions (120-154)


### 120. What is Kubernetes?

**Answer:** Kubernetes is a container orchestration platform that automates deployment, scaling, networking, service discovery, rolling updates, and self-healing of containerized applications. It is the standard platform for managing containers in production across cloud and on-prem environments.

**Useful commands:**
```bash
kubectl version --client
kubectl cluster-info
```


### 121. What is a pod?

**Answer:** A pod is the smallest deployable unit in Kubernetes. It usually contains one main container, though multiple tightly coupled containers can share the same pod network namespace and volumes. Pods are ephemeral and usually created indirectly through higher-level controllers.

**Useful commands:**
```bash
kubectl get pods -A
kubectl describe pod <pod-name>
```


### 122. What is a deployment?

**Answer:** A Deployment manages a ReplicaSet and provides declarative updates for pods. It maintains the desired number of replicas and supports rolling updates and rollbacks. Most stateless applications are run using Deployments.

**Useful commands:**
```bash
kubectl get deploy
kubectl rollout status deployment/myapp
```


### 123. What is a ReplicaSet?

**Answer:** A ReplicaSet ensures a specified number of identical pod replicas are running. In day-to-day work, you usually manage ReplicaSets indirectly through Deployments rather than creating them manually.

**Useful commands:**
```bash
kubectl get rs
kubectl describe rs <name>
```


### 124. What is the difference between a deployment and a StatefulSet?

**Answer:** Deployments are for stateless applications where pod identity does not matter. StatefulSets are for stateful workloads needing stable pod names, ordered rollout, and persistent storage, such as databases or clustered systems.

**Useful commands:**
```bash
kubectl get statefulsets
kubectl get deploy
```


### 125. What is a DaemonSet?

**Answer:** A DaemonSet ensures one pod runs on each selected node. It is commonly used for log collectors, monitoring agents, CNI components, and security agents.

**Useful commands:**
```bash
kubectl get daemonsets -A
```


### 126. What is a Service in Kubernetes?

**Answer:** A Service provides a stable virtual IP and DNS name for accessing a set of pods selected by labels. Since pod IPs change, Services are the abstraction that keeps communication stable.

**Useful commands:**
```bash
kubectl get svc -A
kubectl describe svc myservice
```


### 127. What is the difference between ClusterIP, NodePort, and LoadBalancer?

**Answer:** ClusterIP exposes the service only inside the cluster, NodePort exposes it on a port of every node, and LoadBalancer requests an external cloud load balancer. ClusterIP is the default and most common internal mode.

**Useful commands:**
```bash
kubectl get svc
kubectl expose deployment myapp --port=80 --target-port=8080
```


### 128. What is an Ingress?

**Answer:** Ingress provides HTTP and HTTPS routing into the cluster, typically based on hostnames and paths. It works together with an Ingress controller such as NGINX Ingress or AWS Load Balancer Controller.

**Useful commands:**
```bash
kubectl get ingress -A
kubectl describe ingress mying
```


### 129. What is a namespace?

**Answer:** A namespace is a logical partition inside a cluster used to organize resources, apply quotas, and separate environments or teams. Dev, staging, and prod are commonly separated using namespaces.

**Useful commands:**
```bash
kubectl get ns
kubectl config set-context --current --namespace=dev
```


### 130. What is a ConfigMap?

**Answer:** A ConfigMap stores non-sensitive configuration data such as app settings, feature flags, or config files. Pods can consume it as environment variables or mounted files.

**Useful commands:**
```bash
kubectl create configmap app-config --from-literal=ENV=prod
kubectl get configmap app-config -o yaml
```


### 131. What is a Secret in Kubernetes?

**Answer:** A Secret stores sensitive data such as passwords, tokens, or certificates. It is still only base64-encoded in etcd by default, so production clusters should add encryption at rest and strong access controls.

**Useful commands:**
```bash
kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=pass
kubectl get secret db-secret -o yaml
```


### 132. How do liveness and readiness probes differ?

**Answer:** Liveness probes determine whether a container should be restarted, while readiness probes determine whether a pod is ready to receive traffic. A pod can be alive but not ready during startup or dependency checks.

**Useful commands:**
```bash
kubectl describe pod <pod-name>
```


### 133. What is a persistent volume (PV)?

**Answer:** A PersistentVolume is a cluster storage resource. It abstracts the underlying storage implementation, such as EBS, NFS, or a cloud file service. It is consumed through PersistentVolumeClaims.

**Useful commands:**
```bash
kubectl get pv
```


### 134. What is a persistent volume claim (PVC)?

**Answer:** A PVC is a request for storage by a workload. The application references the PVC, and Kubernetes binds it to a suitable PV dynamically or statically depending on the storage class and configuration.

**Useful commands:**
```bash
kubectl get pvc -A
kubectl describe pvc myclaim
```


### 135. What is a StorageClass?

**Answer:** A StorageClass defines how dynamic storage should be provisioned, for example which provisioner to use, volume type, reclaim policy, or performance options. On AWS, a StorageClass may provision gp3 EBS volumes automatically.

**Useful commands:**
```bash
kubectl get storageclass
```


### 136. What is the difference between requests and limits?

**Answer:** Requests define the minimum CPU and memory guaranteed for scheduling, while limits cap the maximum the container can use. Poor sizing can cause OOM kills, CPU throttling, or wasted capacity.

**Useful commands:**
```bash
kubectl top pod
kubectl describe pod <pod-name>
```


### 137. What is an OOMKilled event?

**Answer:** It means the container exceeded its memory limit and the kernel terminated it. This often indicates bad sizing, memory leaks, or spikes in workload. Fixes include raising memory limits, optimizing the app, or changing traffic patterns.

**Useful commands:**
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name> --previous
```


### 138. What is CrashLoopBackOff?

**Answer:** It indicates a container repeatedly starts and crashes, so Kubernetes backs off before restarting again. Common causes include bad environment variables, missing secrets, failed dependencies, or incorrect commands.

**Useful commands:**
```bash
kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name> --previous
```


### 139. How do you debug a failing pod?

**Answer:** Start with kubectl describe for events, then inspect logs, environment variables, image, probes, and resource settings. If the container runs long enough, use kubectl exec to inspect from inside. Debugging is systematic: events, logs, config, network, and dependencies.

**Useful commands:**
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- sh
```


### 140. What is the difference between kubectl apply and kubectl create?

**Answer:** create creates a new resource and fails if it already exists, while apply is declarative and updates the resource to match the manifest. apply is more suitable for GitOps and repeated environment reconciliation.

**Useful commands:**
```bash
kubectl create -f deployment.yaml
kubectl apply -f deployment.yaml
```


### 141. What is a rolling update?

**Answer:** A rolling update replaces pods gradually so service remains available during deployment. Kubernetes controls how many pods can be unavailable or created above the desired count using strategy settings.

**Useful commands:**
```bash
kubectl rollout status deployment/myapp
kubectl rollout history deployment/myapp
```


### 142. How do you roll back a deployment?

**Answer:** If a new version fails, use kubectl rollout undo to return to the previous ReplicaSet. You can also inspect rollout history to choose a specific revision.

**Useful commands:**
```bash
kubectl rollout undo deployment/myapp
kubectl rollout history deployment/myapp
```


### 143. What is a label and selector?

**Answer:** Labels are key-value metadata attached to resources, and selectors are queries used to match resources by labels. Services, Deployments, and many controllers depend on correct label-selector matching.

**Useful commands:**
```bash
kubectl get pods --show-labels
kubectl get pods -l app=myapp
```


### 144. What is taint and toleration?

**Answer:** Taints repel pods from nodes unless the pods have matching tolerations. They are used to reserve nodes for special workloads such as GPUs, system components, or isolated jobs.

**Useful commands:**
```bash
kubectl describe node <node-name> | grep -i Taint -A2
```


### 145. What is node affinity?

**Answer:** Node affinity lets you influence or require scheduling onto nodes with specific labels. It is more expressive than the older nodeSelector and supports preferred and required rules.

**Useful commands:**
```bash
kubectl get nodes --show-labels
```


### 146. What is a Horizontal Pod Autoscaler (HPA)?

**Answer:** HPA automatically adjusts the number of pod replicas based on metrics such as CPU, memory, or custom application metrics. It helps match capacity with demand without manual scaling.

**Useful commands:**
```bash
kubectl get hpa
kubectl autoscale deployment myapp --cpu-percent=70 --min=2 --max=10
```


### 147. What is a Job and a CronJob?

**Answer:** A Job runs a task to completion, such as a migration or report, while a CronJob runs Jobs on a schedule. They are used for batch workloads rather than long-running services.

**Useful commands:**
```bash
kubectl get jobs
kubectl get cronjobs
```


### 148. What is the purpose of kubectl describe?

**Answer:** describe gives a detailed human-readable view of a resource including events, conditions, container states, and related information. It is one of the fastest ways to troubleshoot problems.

**Useful commands:**
```bash
kubectl describe pod <pod-name>
kubectl describe node <node-name>
```


### 149. What is the difference between kubectl logs and kubectl exec?

**Answer:** kubectl logs shows application output from the container stdout/stderr streams, while kubectl exec runs a command inside the container. logs is the first step; exec is for deeper inspection when needed.

**Useful commands:**
```bash
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- sh
```


### 150. What is CoreDNS in Kubernetes?

**Answer:** CoreDNS is the cluster DNS service that resolves service names like myservice.mynamespace.svc.cluster.local. If service discovery breaks, many applications will fail even though pods are running.

**Useful commands:**
```bash
kubectl get pods -n kube-system | grep coredns
```


### 151. What is kubelet?

**Answer:** kubelet is the agent that runs on each node and ensures containers described by pod specs are running. It communicates with the API server and the container runtime. If kubelet is unhealthy, node and pod problems follow.

**Useful commands:**
```bash
systemctl status kubelet
journalctl -u kubelet -n 50 --no-pager
```


### 152. What is etcd?

**Answer:** etcd is the distributed key-value store that holds the cluster state. It is critical to cluster health and must be backed up in self-managed clusters. Losing etcd means losing the control plane state.

**Useful commands:**
```bash
kubectl get componentstatuses 2>/dev/null || true
```


### 153. What is the control plane in Kubernetes?

**Answer:** The control plane includes components such as the API server, scheduler, controller manager, and etcd. It decides desired state, scheduling, and reconciliation, while worker nodes run the actual pods.

**Useful commands:**
```bash
kubectl cluster-info
```


### 154. How do you inspect cluster resources usage?

**Answer:** You can use metrics-server-backed commands like kubectl top for nodes and pods. This is useful for capacity reviews and identifying hot workloads.

**Useful commands:**
```bash
kubectl top nodes
kubectl top pods -A
```


---

## AWS Questions (155-200)


### 155. What is AWS and why is it important for DevOps?

**Answer:** AWS is a cloud platform that provides on-demand infrastructure and managed services. For DevOps engineers, it offers compute, storage, networking, IAM, observability, and managed container platforms that integrate well into CI/CD workflows.

**Useful commands:**
```bash
aws --version
aws configure list
```


### 156. What is EC2?

**Answer:** Amazon EC2 is a virtual server service that lets you launch compute instances on demand. It is used for Jenkins controllers, bastion hosts, legacy applications, self-managed Kubernetes nodes, and general Linux administration tasks.

**Useful commands:**
```bash
aws ec2 describe-instances --region ap-south-1
```


### 157. What is an AMI?

**Answer:** An Amazon Machine Image is a template used to launch EC2 instances. It includes the operating system, and may include packages or application preconfiguration. Custom AMIs speed up provisioning and standardize environments.

**Useful commands:**
```bash
aws ec2 describe-images --owners self amazon --region ap-south-1
```


### 158. What is the difference between an instance type and an AMI?

**Answer:** An AMI defines the software image, while the instance type defines the hardware capacity such as CPU, memory, and networking characteristics. Both are chosen when launching an EC2 instance.

**Useful commands:**
```bash
aws ec2 run-instances --image-id ami-xxxx --instance-type t3.micro --region ap-south-1
```


### 159. What is an EBS volume?

**Answer:** Amazon EBS is block storage for EC2 instances. It is commonly used for root disks, application data, and self-managed database or build storage. Volumes persist independently of the instance if configured accordingly.

**Useful commands:**
```bash
aws ec2 describe-volumes --region ap-south-1
```


### 160. What is the difference between EBS and instance store?

**Answer:** EBS is persistent network-attached block storage, while instance store is ephemeral storage physically attached to the host. Instance store is faster in some cases but data is lost when the instance stops or terminates.

**Useful commands:**
```bash
aws ec2 describe-instance-types --instance-types i3.large --region ap-south-1
```


### 161. What is a security group?

**Answer:** A security group is a virtual firewall attached to ENIs and instances. It controls inbound and outbound traffic using allow rules only. Correct security group design is essential for exposing apps safely.

**Useful commands:**
```bash
aws ec2 describe-security-groups --region ap-south-1
```


### 162. What is the difference between a security group and a network ACL?

**Answer:** Security groups are stateful and applied at the instance ENI level, while network ACLs are stateless and applied at the subnet level. Security groups are used far more often for day-to-day access control, while NACLs provide broader subnet filtering.

**Useful commands:**
```bash
aws ec2 describe-network-acls --region ap-south-1
```


### 163. What is a VPC?

**Answer:** A VPC is a logically isolated virtual network in AWS. It contains subnets, route tables, gateways, security groups, and other networking constructs. Most DevOps deployments start with VPC design because network placement controls connectivity.

**Useful commands:**
```bash
aws ec2 describe-vpcs --region ap-south-1
```


### 164. What is a subnet?

**Answer:** A subnet is a range of IP addresses inside a VPC. Public subnets can route to an internet gateway, while private subnets do not expose instances directly to the internet. Workloads are typically segmented by subnet role such as public, app, and database.

**Useful commands:**
```bash
aws ec2 describe-subnets --region ap-south-1
```


### 165. What is an internet gateway?

**Answer:** An internet gateway enables internet connectivity for resources in public subnets that have suitable route table entries and public IPs. Without it, even a public subnet cannot reach the internet.

**Useful commands:**
```bash
aws ec2 describe-internet-gateways --region ap-south-1
```


### 166. What is a NAT gateway?

**Answer:** A NAT gateway allows instances in private subnets to reach the internet for outbound connections, such as package installation or API access, without exposing them directly to inbound internet traffic.

**Useful commands:**
```bash
aws ec2 describe-nat-gateways --region ap-south-1
```


### 167. What is an Elastic IP?

**Answer:** An Elastic IP is a static public IPv4 address that you can associate with AWS resources. It is useful when you need a stable public address, though many architectures prefer load balancers or DNS instead of directly exposing instances.

**Useful commands:**
```bash
aws ec2 describe-addresses --region ap-south-1
```


### 168. How do you connect to an EC2 Linux instance securely?

**Answer:** Most commonly by SSH using a key pair, controlled security group rules, and often a bastion host or Session Manager. Production access should avoid open SSH to the world and should be logged and restricted.

**Useful commands:**
```bash
ssh -i mykey.pem ubuntu@<public-ip>
```


### 169. What is IAM?

**Answer:** AWS Identity and Access Management controls authentication and authorization across AWS services. It defines who can do what on which resources. DevOps engineers use IAM constantly for users, roles, policies, and service permissions.

**Useful commands:**
```bash
aws iam get-user
aws sts get-caller-identity
```


### 170. What is the difference between an IAM user, group, role, and policy?

**Answer:** A user represents a person or application identity, a group is a collection of users, a role is an assumable identity often used by services or federated principals, and a policy is the permission document attached to identities or resources. Roles are preferred for workloads because they avoid long-lived credentials.

**Useful commands:**
```bash
aws iam list-users
aws iam list-roles
aws iam list-policies --scope Local
```


### 171. What is the principle of least privilege in IAM?

**Answer:** It means granting only the minimum permissions required to perform a task, nothing more. This reduces blast radius and is a core AWS security best practice for both humans and workloads.

**Useful commands:**
```bash
aws iam simulate-principal-policy --policy-source-arn <role-arn> --action-names s3:ListBucket
```


### 172. Why are IAM roles preferred over access keys on EC2?

**Answer:** Roles provide temporary credentials automatically rotated by AWS, while static access keys are long-lived and harder to secure. On EC2, attaching a role through an instance profile is the standard pattern.

**Useful commands:**
```bash
aws ec2 describe-instances --query 'Reservations[].Instances[].IamInstanceProfile' --region ap-south-1
```


### 173. What is ECR?

**Answer:** Amazon Elastic Container Registry is AWS's managed Docker and OCI image registry. It stores container images securely and integrates well with EKS, ECS, and CI/CD pipelines.

**Useful commands:**
```bash
aws ecr describe-repositories --region ap-south-1
```


### 174. How do you authenticate Docker to ECR?

**Answer:** You retrieve a login password using the AWS CLI and pass it to docker login. This is a standard CI/CD step before pushing or pulling images.

**Useful commands:**
```bash
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-south-1.amazonaws.com
```


### 175. How do you create an ECR repository?

**Answer:** Use the AWS CLI or console to create the repository, then tag and push images into it. Repositories can also have lifecycle policies and image scanning settings.

**Useful commands:**
```bash
aws ecr create-repository --repository-name myapp --region ap-south-1
```


### 176. How do you push an image to ECR?

**Answer:** Authenticate to ECR, tag the local image with the repository URI, and then push. This is usually automated in Jenkins or GitHub Actions after a successful build.

**Useful commands:**
```bash
docker tag myapp:1.0 <account-id>.dkr.ecr.ap-south-1.amazonaws.com/myapp:1.0
docker push <account-id>.dkr.ecr.ap-south-1.amazonaws.com/myapp:1.0
```


### 177. What is EKS?

**Answer:** Amazon Elastic Kubernetes Service is AWS's managed Kubernetes offering. AWS manages the control plane, while you manage worker nodes or Fargate profiles depending on your architecture. EKS is widely used for production container orchestration on AWS.

**Useful commands:**
```bash
aws eks list-clusters --region ap-south-1
```


### 178. What is the difference between EKS control plane and worker nodes?

**Answer:** The EKS control plane is managed by AWS and runs Kubernetes control plane components. Worker nodes run your application pods and are managed by you via managed node groups, self-managed nodes, or Fargate.

**Useful commands:**
```bash
kubectl get nodes
aws eks describe-cluster --name mycluster --region ap-south-1
```


### 179. What is a managed node group in EKS?

**Answer:** A managed node group is an AWS-managed group of EC2 worker nodes for an EKS cluster. AWS handles lifecycle aspects such as provisioning, updates, and integration with auto scaling more easily than self-managed nodes.

**Useful commands:**
```bash
aws eks list-nodegroups --cluster-name mycluster --region ap-south-1
```


### 180. How do you configure kubectl to access an EKS cluster?

**Answer:** Use aws eks update-kubeconfig to write cluster access details into your kubeconfig. Access is then governed by both AWS IAM authentication and Kubernetes RBAC configuration.

**Useful commands:**
```bash
aws eks update-kubeconfig --name mycluster --region ap-south-1
kubectl get nodes
```


### 181. What is the aws-auth mechanism or EKS access mapping concept?

**Answer:** EKS needs a mapping between AWS IAM identities and Kubernetes access. Historically this was done through the aws-auth ConfigMap; newer access entry mechanisms are also available. The key interview point is that AWS IAM authentication alone is not enough unless the identity is also authorized in Kubernetes/EKS access configuration.

**Useful commands:**
```bash
kubectl get configmap aws-auth -n kube-system -o yaml
```


### 182. How do pods in EKS pull images from ECR?

**Answer:** The nodes or pod identities must have IAM permissions to access ECR. With node roles this often works automatically for EKS worker nodes; with IRSA or other pod identity patterns, specific service accounts can receive scoped permissions.

**Useful commands:**
```bash
kubectl describe pod <pod-name>
aws iam get-role --role-name <node-role-name>
```


### 183. What is CloudWatch?

**Answer:** Amazon CloudWatch is AWS's observability platform for metrics, logs, alarms, dashboards, and events. DevOps engineers use it to monitor infrastructure, applications, EKS nodes, EC2 instances, and automated responses.

**Useful commands:**
```bash
aws cloudwatch list-metrics --region ap-south-1
aws logs describe-log-groups --region ap-south-1
```


### 184. What is the difference between CloudWatch metrics and CloudWatch Logs?

**Answer:** Metrics are numerical time-series data such as CPUUtilization or request count, while Logs stores raw log events from applications and services. Metrics are best for alerting and dashboards; logs are best for detailed troubleshooting and search.

**Useful commands:**
```bash
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --start-time 2026-03-15T00:00:00Z --end-time 2026-03-16T00:00:00Z --period 300 --statistics Average --dimensions Name=InstanceId,Value=i-1234567890abcdef0 --region ap-south-1
```


### 185. What is a CloudWatch alarm?

**Answer:** A CloudWatch alarm watches a metric or expression and changes state when a threshold is crossed. It can notify via SNS, trigger automation, or support scaling decisions.

**Useful commands:**
```bash
aws cloudwatch describe-alarms --region ap-south-1
```


### 186. How do you send EC2 logs to CloudWatch Logs?

**Answer:** You usually install and configure the CloudWatch agent on the instance, define which files or metrics to collect, and attach an IAM role allowing log delivery. Centralizing logs makes troubleshooting easier than logging only to local files.

**Useful commands:**
```bash
sudo systemctl status amazon-cloudwatch-agent
aws logs describe-log-streams --log-group-name /ec2/app --region ap-south-1
```


### 187. What is an Auto Scaling Group (ASG)?

**Answer:** An ASG maintains a desired number of EC2 instances and can scale out or in based on policies or schedules. It improves availability and elasticity for instance-based workloads.

**Useful commands:**
```bash
aws autoscaling describe-auto-scaling-groups --region ap-south-1
```


### 188. What is the difference between scaling vertically and horizontally in AWS?

**Answer:** Vertical scaling increases the size of a single instance, while horizontal scaling adds more instances or pods. Horizontal scaling is generally preferred for resilient production systems because it avoids a single large point of failure.

**Useful commands:**
```bash
# Vertical: change t3.micro to t3.large
# Horizontal: increase desired capacity in ASG or replicas in Kubernetes
```


### 189. What is an Application Load Balancer (ALB)?

**Answer:** An ALB distributes HTTP and HTTPS traffic across targets such as EC2 instances, IPs, or containers. It supports path- and host-based routing, TLS termination, and health checks, making it ideal for web applications and microservices.

**Useful commands:**
```bash
aws elbv2 describe-load-balancers --region ap-south-1
```


### 190. What is the difference between ALB and NLB?

**Answer:** ALB works at Layer 7 and understands HTTP/HTTPS, while NLB works at Layer 4 and handles TCP/UDP with high performance and static IP support. Choose based on protocol and routing needs.

**Useful commands:**
```bash
aws elbv2 describe-load-balancers --region ap-south-1
```


### 191. What is an S3 bucket and why should a DevOps engineer know it?

**Answer:** Amazon S3 is object storage used for artifacts, backups, state files, logs, static content, and more. Even if the role focuses on EKS and EC2, S3 appears frequently in CI/CD and infrastructure workflows.

**Useful commands:**
```bash
aws s3 ls
aws s3 cp build.zip s3://my-bucket/
```


### 192. How is S3 commonly used in CI/CD?

**Answer:** It is often used to store build artifacts, static frontend bundles, backup files, and Terraform state. Using S3 decouples artifact storage from the CI server's local disk.

**Useful commands:**
```bash
aws s3 cp target/app.jar s3://my-artifacts-bucket/releases/
```


### 193. What is Systems Manager Session Manager?

**Answer:** Session Manager lets you access EC2 instances without direct SSH exposure by using the SSM agent and IAM permissions. It improves security and auditability and is increasingly preferred over open SSH.

**Useful commands:**
```bash
aws ssm start-session --target i-1234567890abcdef0 --region ap-south-1
```


### 194. What is the shared responsibility model in AWS?

**Answer:** AWS is responsible for security of the cloud, while customers are responsible for security in the cloud, including IAM, data, OS patching on EC2, network rules, and application configuration. The exact boundary depends on the managed service.

**Useful commands:**
```bash
# Conceptual model, not a single command
```


### 195. How do you check who you are authenticated as in AWS CLI?

**Answer:** Use sts get-caller-identity to verify the current account, user, or role. This simple command prevents many mistakes in multi-account environments.

**Useful commands:**
```bash
aws sts get-caller-identity
```


### 196. What is IRSA in EKS?

**Answer:** IRSA, IAM Roles for Service Accounts, lets Kubernetes service accounts assume IAM roles without sharing node-wide permissions. This gives pods least-privilege AWS access and is a major best practice on EKS.

**Useful commands:**
```bash
kubectl get sa -A
aws iam list-open-id-connect-providers
```


### 197. Why is CloudWatch important for EKS and EC2 operations?

**Answer:** It gives centralized visibility into infrastructure health, application logs, alarms, and trends. Without observability, build and deployment automation becomes risky because failures are harder to detect and diagnose.

**Useful commands:**
```bash
aws cloudwatch list-dashboards --region ap-south-1
aws logs tail /aws/containerinsights/mycluster/application --follow --region ap-south-1
```


### 198. What is the purpose of ECR lifecycle policies?

**Answer:** Lifecycle policies automatically expire old or unneeded images based on rules such as age or count. This reduces storage cost and keeps registries cleaner without manual cleanup.

**Useful commands:**
```bash
aws ecr get-lifecycle-policy --repository-name myapp --region ap-south-1
```


### 199. How do you design secure access for a Jenkins server deploying to AWS?

**Answer:** The standard pattern is to avoid static root credentials, assign least-privilege IAM roles or federated access, restrict network exposure, store secrets in a credentials manager, and log all actions. Jenkins should push images to ECR, interact with EKS or EC2 using scoped permissions, and ideally run agents separately from the controller.

**Useful commands:**
```bash
aws sts get-caller-identity
aws iam list-attached-role-policies --role-name JenkinsRole
```


### 200. What is the difference between authentication and authorization in AWS IAM?

**Answer:** Authentication verifies who you are, such as through login, access keys, or assumed role credentials. Authorization decides what actions that identity may perform based on IAM policies, resource policies, and conditions. Both are required for secure AWS access.

**Useful commands:**
```bash
aws sts get-caller-identity
aws iam simulate-principal-policy --policy-source-arn <arn> --action-names eks:DescribeCluster
```


---

## Final Revision Tips

1. Be able to explain every answer in simple language first.
2. Memorize only the commands you understand.
3. Practice Linux, Docker, Jenkins, Kubernetes, and AWS hands-on.
4. For 4+ years experience, interviewers expect both concept clarity and operational reasoning.
5. When answering, connect the concept to real CI/CD, deployment, troubleshooting, security, or scalability use cases.
