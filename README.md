# module-3-itnsa-public

## Техническое задание

Компания "Облачные решения для бизнеса" занимается проектированием, развертыванием и миграцией IT-систем. Вы являетесь инженером, который занимается установкой и настройкой информационных систем. 

Внутренний отдел разработки ПО разработали REST API сервис для учета статистики от компаний, которые пользуются услугами вашей организации. К вам обратились с задачей разработать процесс непрерывной интеграции и доставки данного ПО в облачный провайдер.

На данный момент вам необходимо разработать и протестировать работоспособный вариант данной задачи, который в последствии будет использоваться в продуктовой среде. За основу используйте следующий репозиторий https://github.com/WalkerPM/module-3-itnsa-public .

Создайте собственный публичный репозиторий и скопируйте все файлы из основного репозитория, предоставленного выше.

Продуктовая среда представляет из себя 3 EC2 инстанса в US-East-1, которые имеют доступ парольный доступ через SSH, все необходимые данные для работы вам будут предоставлены.

CI/CD процесс должен выполнять следующие задачи
* Сборка Docker контейнера для приложения
* Запуск экземпляра приложения на каждом из серверов
* Настройка Full-mesh балансировки запросов между экземплярами приложения
* Настройка репликации файлов, создаваемых экземпляром приложения между всеми инстансами.
* API должно быть доступно как по HTTP, так и по HTTPS (сертификат может быть самоподписанный) (Можете использовать Let's Encrypt и https://sslip.io)
* У инстансов должны быть доступны только необходимые порты и протоколы (HTTP, HTTPS, SSH, ICMP-Echo)
  
Ваш CI/CD процесс должен стартовать через build.sh и использовать следующие переменные окружения:
* INSTANCE-1
* INSTANCE-2
* INSTANCE-3
* SSH_USERNAME
* SSH_PASSWORD
* COMPETITOR_ID 

и они должны быть записаны в отдельный файл "variables", переменная "COMPETITOR_ID" должна быть доступна приложению. Учтите, при проверке данные переменные будут изменяться.


### Веб-приложение
Веб приложение работает с создаваемыми им файлами, каждый запущенный экземпляр приложения должен иметь доступ файлам, который создал другой экземпляр, для чего рекомендуется использовать общее хранилище.

Приложение умеет отправлять healthcheck - http://fqdn/health

Проверка работоспособности приложения - http://fqdn/app

Приложение имеет файл конфигурации, который необходимо рассмотреть и настроить под ваше решение, учтите, основная задача приложения - ответ на HTTP/HTTPS запросы и создание файлов.

# Как это будет проверяться

Для проверки будут созданы новые инстансы, а скрипт запущен в ОС Amazon Linux. 

Группа оценки заменит переменные окружения (кроме COMPETITOR_ID) на свои и запустить build.sh от пользователя root, в нем должен выполняться весь CI/CD процесс, для выполнения которого можно использовать любые средства автоматизации. 

Если вы используете дополнительные пакеты или средства автоматизации(i.e docker, ansible, etc.), убедитесь, что они так же установятся в процессе выполнения "build.sh".
