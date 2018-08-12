# rsamba
Custom build of Samba for R.

A security focused release of Samba, meant for simple file sharing - custom for R.

Tested security configuration:
-- User namespace
-- Non root user within namespace
-- All capabilities dropped
-- No new privileges enabled
-- Apparmor enabled
-- Sensible memory, PID, and file descriptor ulimits and cgroup restrictions

Command to launch:

sudo docker run -dt --init --name sambashare --restart always -p 445:4045/tcp -v /<base_dir>:/samba -m 3G --memory-reservation 1G --cpus=1.5 --memory-swappiness=5 --pids-limit 196 --ulimit nofile=10060:12560 --ulimit nproc=164:196 --security-opt no-new-privileges --cap-drop ALL iotos/samba:latest

to change a password (from host, default is 'test', requires samba-client package):
smbpasswd -U samba

to mount to a folder (requires cifs-utils):
sudo mount -t cifs //localhost/samba /tmpfs -o uid=id -u,gid=id -g,username=samba
