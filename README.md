**Example of ansible ad-hoc command-**

_Commands run on host server:_

ubuntu@ip-172-31-24-64:~/ansible$ ansible -i inventory 172.31.18.202 -m shell -a "mkdir ansible_target"
172.31.18.202 | CHANGED | rc=0 >>

ubuntu@ip-172-31-24-64:~/ansible$ ansible -i inventory 172.31.18.202 -m shell -a "mkdir ansible_test"
172.31.18.202 | CHANGED | rc=0 >>

_Folders created on target server:_

<img width="1792" height="617" alt="Screenshot 2025-08-07 at 10 40 00 PM" src="https://github.com/user-attachments/assets/f9be85ec-1be5-4dde-95d7-2b7f48d26d5d" />
