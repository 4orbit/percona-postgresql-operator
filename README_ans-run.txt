нужно запускать из корня развернутой репы percona pgo v1.2

    ansible-playbook -v run-db_cluster-deploy.yml  -e "target=nsi,vi,pdb"
    ansible-playbook -v run-db_cluster-deploy.yml  -e "target=pgo_cluster"

если -v тогда debug значимых переменных

Делает:
1) проверяет и создает, если нет неймспейс pgo
2) деплоит pgo operator и ждет успешного завершения
3) из шаблонов в ans-templates и переменных в плейбуке
генерит кастом конфигмап и манифест деплоя для каждого кластера из
перечисленных
4) пременяет кастом конфигмапы и манифесты для каждого кластера,
которые распологаются в deploy/ans-name-conf/
5) теперь с настройками и для kerberos -- они в кастом конфигмапе

TODO

нет про диски уточнения


kubectl -n pgo exec nsi-6fc7c96785-ms89g -- env | grep PATRONI_POSTGRESQL_DATA_DIR
PATRONI_POSTGRESQL_DATA_DIR=/pgdata/nsi

kubectl -n pgo exec nsi-repl1-c575c9c5c-56hnz -- env | grep PATRONI_POSTGRESQL_DATA_DIR
PATRONI_POSTGRESQL_DATA_DIR=/pgdata/nsi-repl1

kubectl -n pgo exec nsi-repl1-c575c9c5c-56hnz -- sh -c 'echo $PATRONI_POSTGRESQL_DATA_DIR'
/pgdata/nsi-repl1


полезные команды

for i in {01..29}; do kubectl -n pgo delete pv crunchy-pv$i ; done
for f in deploy/ans-*/cr*; do kubectl -n pgo delete -f $f ; done


kubectl -n pgo get svc | rg LoadBalancer

for f in deploy/ans-*/lb*; do kubectl -n pgo delete -f $f ; done
for f in deploy/ans-*/lb*; do kubectl -n pgo apply -f $f ; done

