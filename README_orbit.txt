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
