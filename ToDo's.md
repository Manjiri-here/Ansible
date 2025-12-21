ToDo's

1) Write ansible playbook

2) Create a dynamic inventroy file and note down the steps to create it. Also check how can we create dynamic inventory to get resources created on cloud (AWS)

3) Create ansible vault.

4) Practice how can we copy pulic key to multiple machines at once automatically.

5) How to copy files and directory recursively to host in asible. (Using copy module, and couple of methods more. Practice it)

6) Test this scenario:
   Setup in ansible galaxy out of which there are few decommissioned servers . How will you exclude or identify the decommissioned ones?  
By using dynamic inventory. As dynamic inventory gathers the IP during the execution of playbook it automatically picks the servers which are running and not the deleted ones. We can use the status flag as state= running to only filter the running servers.



