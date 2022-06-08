заметки по тесту оператора от percona


minikube start
minikube tunnel

cd /home/orbit/git-work/percona-postgresql-operator/ && kubectl config set-context $(kubectl config current-context) --namespace=pgo

узнавать пароли можно так:
kubectl get secrets pg-nsi-users -o jsonpath='{.data}' | jq

смотрим нужный юзернейм и тогда например:
kubectl get secrets pg-nsi-users -o 'go-template={{index .data "pguser"}}' | base64 -d

удалить "кластер PG" --
kubectl delete -f deploy/pg-restnsi.yaml 

удалить отработавшие pod-ы 
kubectl delete jobs --field-selector status.successful=1 


## развертывание кластера vi c хранил в s3

kubectl create namespace pgo
kubectl config set-context $(kubectl config current-context) --namespace=pgo
kubectl apply -f deploy/operator.yaml 
kubectl create configmap vi-custom-config --from-file=deploy/vi-conf/postgres-ha.yaml 

kubectl apply -f deploy/backup/vi-s3-backrest-repo-config-secret.yaml 
kubectl apply -f deploy/cr-vi_s3_alt8_pg12.yaml 

## замечания -- в этом тестовом варианте

    - без хранилища -- не развернется до конца -- будет ошибка stanza
    - на хостах с PG нет в env PGBACKREST_REPO1_S3_VERIFY_TLS=n по этому нужно
      руками добавлять --repo1-storage-verify-tls=n для info например
    - при этом Patroni switchover -- делает бэкап нормально -- только в s3
    - команда archive_command -- для posix и s3 -- в PG но складывает вроде
      только в s3
    - после развертывания выполнился таск backrest-backup-vi -- хотел делать
      both -- но по факту в локальной репе нет ни чего
    - после удаления PGHOME на реплике -- Pantoni восстановил из бэкапа

