# kubernetes


## Sem2. Kubernetes

В этом занятии мы научимся работать с Kubernetes и поднимем с помощью него REST 
сервис с прошлого [курса](https://github.com/EvgeniiMunin/ctr_project).

Для деплоя мы попрактикуемся со следующими манифестами
- поднимем один под и пробросим порты для доступа клиента к нему
- поднимем `ReplicaSet` и запустить сервис на нескольких подах распределенно
- поднимем под с ограничениями на ресурсы с помощью `requests`, `limits`
- поднимем `Service` с балансировщиком нагрузки с клиента на поды

### Установка `kubectl` и подключение к кластеру
Подробная инструкция к подключению к k8s кластеру доступна в документации [VKCloud](https://cloud.vk.com/docs/base/k8s/connect/kubectl#9959-tabpanel-0)

```bash
# install k8s cli
brew install kubernetes-cli
kubectl version

# install and activate client-keystone-auth
curl -sSL \\n  https://hub.mcs.mail.ru/repository/client-keystone-auth/latest/linux/client-install.sh \\n| bash
source "/Users/evgeniimunin/.zshrc"

# export and check env vars
source openrc.sh
echo $OS_PASSWORD
echo $OS_PROJECT_ID

# define kubeconfig
sudo chmod 0600 /path/to/kubeconfig.yaml
export KUBECONFIG=/path/to/kubeconfig.yaml
echo $KUBECONFIG

# check cluster connection
kubectl cluster-info
```

### Основные команды

```bash
# create and switch namespace
kubectl create namespace ctr-app
kubectl config set-context --current --namespace=ctr-app

# apply manifest config
kubectl apply -f kubernetes/service.yaml

# forward ports from pod running on remote node to localhost
kubectl port-forward pod/ctr-app-kube 8000:8000

# get info about k8s cluster
kubectl cluster-info
kubectl get namespaces
kubectl get nodes

# get list of pods/ replica sets/ services and their status
kubectl get pods
kubectl get rs

# get detailed info on particular pod/ replica set/ service
kubectl describe service ctr-service
kubectl describe pod ctr-app

# delete pod/ replica set/ service
kubectl delete rs ctr-app

```

### Результаты

Для визуального отображения элементов кластера k8s мы воспользуемся приложением [Lens](https://k8slens.dev/)

Ноды в кластере k8s VKCloud
![img.png](imgs/img.png)

Запущенный под в кластере и его лог после обработки запросов клиента
![img_1.png](imgs/img_1.png)

Replica Set из 4х подов
![img_2.png](imgs/img_2.png)

Сервис Load Balancer
![img_3.png](imgs/img_3.png)