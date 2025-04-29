Practice of:
Run a Replicated Stateful Application

https://kubernetes.io/docs/tasks/run-application/run-replicated-stateful-application/#sending-client-traffic


# Useful kubectl commands (default namespace):

kc cluster-info

kc get nodes -o wide

kc apply -f mysql-configmap.yaml -f mysql-services.yaml -f mysql-statefulset.yaml

kc get configmap

kc get svc

kc get statefulsets

kc get pv

kc get pvc


kc describe statefulsets mysql

kc describe pod mysql-0

kc logs mysql-0 -c <init-container-name>

kc get events --watch

kc scale statefulset mysql  --replicas=5

kc get pods -l app=mysql --watch


# Mysql shell

## connect to shell of mysql inside pod 0

kc exec -it mysql-0 -- mysql -uroot -p

## check is it primary or replica

mysql> SHOW VARIABLES LIKE 'read_only';

## databases list and tables

mysql> SHOW DATABASES;

mysql> USE your_custom_db;

mysql> SHOW TABLES;

## run client pod performing select to service mysql

kubectl run mysql-client --image=mysql:5.7 -i -t --rm --restart=Never --\
  mysql -h mysql-read -e "SELECT * FROM test.messages"

## check that connections accept all servers of the cluster

kubectl run mysql-client-loop --image=mysql:5.7 -i -t --rm --restart=Never --\
  bash -ic "while sleep 1; do mysql -h mysql-read -e 'SELECT @@server_id,NOW()'; done"