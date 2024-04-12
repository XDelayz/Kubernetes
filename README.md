У новому репозиторії під назвою Кубернетіс я буду тестувати розгортання деплоймента в кубернетіс кластері, також буду налаштовувати доступ до додатка через dns, а також встановлю SSL сертифікат для доступу по https.
Цей репозиторій буде як мої нотатки, щоб я не забув, що і як робив.

1. Спочатку підключаємось до кубернетіс кластера по API взявши конфіг файл в провайдера де розгорнуто куб.
Використовуємо команду де вказуємо шлях до файлу конфігурації:
echo 'export KUBECONFIG=/home/andrii/Загрузки/kubeconfig-aturantsef.txt' >> ~/.bashrc
перезапускаємо термінал та перевіряємо наступною командою конект до API-сервера:
kubectl cluster-info

2. Далі робимо деплоймент нашого додатку вказавши назву файлу deployments.yaml
в розділі image: замість adv4000/k8sphp:latest - можна вказати свій образ який було підготовлено та завантажено в докер хаб. 

запускаємо командою деплоймент kubectl apply -f deployments.yaml
в моєму файлі одразу і деплой сервісу присутній для доступу додатка зовні, але після цього потрібно зробити деплой інгресс контролера який дасть можливість
додатку бути доступним не по адресі, а по домену. Треба перед цим обовязково на реєстраторі домена прописати А запис.
Зовнішній айпі можна дізнатись користуючись командою
kubectl get service
навпроти назви мого сервісу my-single-pod-service буде EXTERNAL-IP - його прописуємо на реєстраторі.

3. В файлі ingress.yaml в пункті hosts замість dec3mber.pp.ua підставляємо свій домен та запускаємо команду:
kubectl apply -f ingress.yaml

4. Перевіряємо доступність по домену dec3mber.pp.ua


