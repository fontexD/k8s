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
4.
<b>now start the zabbix server and agent</b><br>
kubectl apply -f zabbix.yaml
