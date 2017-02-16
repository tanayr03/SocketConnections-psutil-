import psutil

connections = psutil.net_connections(kind='tcp')
non_empty_connections = []
grouped_list = []
list_of_pids = []
for process in connections:

    pid = process[6]
    laddr = process[3]
    raddr = process[4]

    if not('::' in laddr[0]) and len(raddr)>0:
        non_empty_connections.append(process)
        list_of_pids.append(pid)

newList = []
for i in sorted(list_of_pids, key=list_of_pids.count, reverse=True):
    if i not in newList:
        newList.append(i)

for pids in newList:

    for processes in non_empty_connections:
        if pids == processes[6]:
            grouped_list.append(processes)

print "pid,","laddr,","raddr,","status,"
for process_details in grouped_list:
    print process_details[6],",",process_details[3],",",process_details[4],",",process_details[5]
