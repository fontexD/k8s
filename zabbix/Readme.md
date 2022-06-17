Pre.

configure 

1.
<b>First create the namespaces:</b><br>
kubectl apply -f namespace.yaml

2.
<b>Create the pvc and configmap for mysql:</b><br>
kubectl apply -f pvc.yaml<br>
kubectl aplpy -f configmap.yaml<br>

3.
<b>create zabbix user in mysql:</b> <br>
kubectl exec -it -n ops-services <mysql-pod-name> -- sh <br>
mysql -u root -p

CREATE USER 'zabbix'@'%'IDENTIFIED WITH mysql_native_password BY 'password';<br>
4.<br>
<b>now start the zabbix server and agent</b><br>
but first correct the zabbx mysql pass in the zabbix.yaml same for zabix-ui.yaml<br>
kubectl apply -f zabbix.yaml<br
5.<br>
  now apply the web-ui , correct the pass if not already done, and the zabbix-server address by default its zabbixserver.zabbix.svc.cluster.local<br>
  kubectl apply -f zabbix-ui.yaml<br>
  
  now if you go to http://any_node_ip:30080 you should be able to see zabbix login.<br>
  username:Admin <br
  password: zabbix
  
