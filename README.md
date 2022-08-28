# read-file-log-and-noti-telegram

info: 
- id group telegram: -group_chat_id
- id bot telegram=: botX
- path file log: /path/run.log
- os: ubuntu.

step 1: install crontab
```
apt-get install cron
```

step 2: create script read file log
- create file
```
cd  /path

nano read_log_and_noti.sh
```
- add and save:
```
timeout 300s tail -F /path/run.log |
grep --line-buffered 'ERROR' |
while read ; do curl -s -X POST https://api.telegram.org/botX:hash_code/sendMessage -d chat_id=-group_chat_id -d text="SERVER have ERROR!"; done
```
```
chmod +x read_log_and_noti.sh
```
- user crontab
```
crontab -e
```

- add:
```
*/5 * * * * /path/read_log_and_noti.sh
```
every 5 minutes run file read_log_and_noti.sh
