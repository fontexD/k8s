1.
<b>First create the namespaces:</b>
kubectl apply -f namespace.yaml

2.
<b>Create the pvc for mysql:</b>
kubectl apply -f pvc.yaml

3.
<b>create zabbix user in mysql:</b>
mysql -u root -p
kubectl exec -it -n ops-services <mysql-pod-name> -- sh

CREATE USER 'zabbix'@'%'IDENTIFIED WITH mysql_native_password BY 'password';
  
