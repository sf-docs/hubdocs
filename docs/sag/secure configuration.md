# Безопасная сетевая конфигурация

Для установки и дальнейшего функционирования системы должны быть разрешены внутренние HTTPS сетевые соединения.

## Настройка HTTPS соединения

Для настройки HTTPS-соединения следует получить подписанные удостоверяющим центром (CA, Certificate Authority) сертификаты (открытый и закрытый) в формате PEM.

Шаги настройки:

1.	Cкопируйте полученные сертификаты в папку **/opt/apphub/ssl**:

		$ ls -l ./ssl/
		total 16
		-rw-r--r-- 1 root root 1277 Mar 9 14:33 hub.crt
		-rw------- 1 root root 1679 Mar 9 14:33 hub.key

2.	Измените конфигурацию запуска **hub-ui** — в секцию **volumes** добавьте папку, в которой будут храниться ключ и сертификат:

		volumes:
			- ./nginx/hub.conf:/etc/nginx/conf.d/default.conf:ro
			- ./logs/hub-ui:/var/log/nginx
			- ./ssl:/etc/ssl/certs/ssl-cert:ro

3.	Измените конфигурацию **/opt/apphub/nginx/hub.conf** (**hub-ui**), для использования SSL:

		listen 443 ssl;
		ssl_certificate /etc/ssl/certs/ssl-cert/hub.crt; # certificate
		ssl_certificate_key /etc/ssl/certs/ssl-cert/hub.key; # private key
		if ($scheme != "https") { # redirect 80 to 443
			return 301 https://$host$request_uri;
		}

## Особенности настройки HTTPS в закрытом контуре

Если AppSec.Hub устанавливается в закрытом контуре, возникает необходимость использования самоподписанных сертификатов.

### Создание самоподписанных сертификатов

#### Создание корневого сертификата CA

1.	Сгенерируйте ключ корневого сертификата.

		openssl genrsa -out rootCA.key 2048

2.	Создайте корневой сертификат CA.

		openssl req -x509 -new -key rootCA.key -days 10000 -out rootCA.crt

	В ходе создания корневого сертификата указываются следующие параметры:

		Country Name (2 letter code) [AU]:
		State or Province Name (full name) [Some-State]:
		Locality Name (eg, city) []:
		Organization Name (eg, company) [Internet Widgits Pty Ltd]:
		Organizational Unit Name (eg, section) []:
		Common Name (e.g. server FQDN or YOUR name) []: 
		Email Address []:

	Созданный корневой сертификат может использоваться для заверения сертификатов серверов и устанавливаться на клиентские машины.

	!!! fail "Внимание"
		Сертификат **rootCA.crt** может копироваться на сервера и клиентские машины, а ключ **rootCA.key** следует надежно защитить от несанкционированного доступа.

#### Создание самоподписанного сертификата

1.	Сгенерируйте приватный ключ.

		openssl genrsa -out server101.mycloud.key 2048

2.	Сформируйте запрос на сертификат.

		openssl req -new -key server101.mycloud.key -out server101.mycloud.csr

	В ходе выполнения данной команды указываются следующие параметры:

		Country Name (2 letter code) [AU]:
		State or Province Name (full name) [Some-State]:
		Locality Name (eg, city) []:
		Organization Name (eg, company) [Internet Widgits Pty Ltd]:
		Organizational Unit Name (eg, section) []:
		Common Name (e.g. server FQDN or YOUR name) []:server101.mycloud
		Email Address []:
		A challenge password []:
		An optional company name []:

	Важно отметить, что на данном этапе в качестве параметра `Common Name` необходимо указать имя вашего сервера (домен или IP, например, домен **server101.mycloud**).

3.	Подпишите запрос сертификата созданным корневым сертификатом, см. раздел «[Создание корневого сертификата CA](./secure%20configuration.md#ca)».

		openssl x509 -req -in server101.mycloud.csr -CA rootCA.crt -CAkey rootCA.key \
			-CAcreateserial -out server101.mycloud.crt -days 5000

### Установка корневого сертификата (rootca.crt) на стороне клиента

!!! note "Примечание"
	Все действия выполняются в директории, в которой находится **docker-compose.yml**.

#### Добавление rootca.crt в hub-core

1.	При необходимости запросите корневой сертификат (**rootCA.crt**) у администратора (ответственного лица) закрытого контура для добавления его в список доверенных корневых сертификатов. Если такая возможность отсутствует, можно скачать сертификаты всех узлов, к которым будет обращаться AppSec.Hub, и для каждого выполнить операции добавления в **keystore**.

2.	Измените конфигурацию запуска **hub-core**.

	1. Добавьте в **JAVA_OPTS** следующие строки (путь и пароль можно выбрать произвольные).

			–Djavax.net.ssl.trustStore=/etc/ssl/certs/self-signed/keystoreFile
			–Djavax.net.ssl.trustStorePassword=qwerty123

	2. В секцию **volumes** добавьте папку, в которой будет храниться **keystore**.

			volumes:
				–  ./logs/core/logs:/usr/local/tomcat/logs
				–  ./certs:/etc/ssl/certs/self-signed

	3. В папку **./certs** скопируйте сертификат **rootCA.crt** и при необходимости другие сертификаты.

3.	Запустите **hub-core** с новыми параметрами.

4.	Зайдите в docker-контейнер **hub-core** и выполните следующие команды:

		$ docker exec -ti hub-core /bin/bash
		bash-4.4# keytool  -import  -trustcacerts -alias rootCAAlias \
			-file /etc/ssl/certs/self-signed/rootCA.crt \
			-keystore /etc/ssl/certs/self-signed/keystoreFile
		Enter keystore password:
		Re-enter new password:
		...
		Trust this certificate? [no]:  yes
		Certificate was added to keystore

	где:
		
	– `rootCAAlias` — имя, которое будет отображаться при просмотре сертификатов в keystore;

	– пароль и имя **keystoreFile** должны соответствовать указанным в конфигурации для **JAVA_OPTS**, см. шаг 2. Важно понимать, что именно в этой команде задается пароль, а в **JAVA_OPTS** он уже используется, как установленный.
	
	– `/etc/ssl/certs/self-signed` — папка, в которую будет скопирован **keystoreFile**, чтобы он сохранился при перезапуске контейнера (подключена как **volume**).

	Если **rootCA.crt** получить не удалось, аналогичную операцию следует выполнить для сертификата каждого узла, к которому будет обращаться AppSec.Hub.

5.	При использовании дефолтного **keystore** — **/etc/ssl/certs/java/cacerts** (**/usr/lib/jvm/java-11-amazon-corretto/lib/security/cacerts** для **hub-core** на сборке **amazon-corretto-java-11**) установлен пароль по умолчанию: `changeit`.

!!! note "Примечание"
	Этот же порядок действий применим для Jenkins и его нод, сборки которых поставляются клиентам.
	Пути дефолтного **cacert** в конейнерах приведены в таблице ниже.

	Нода|Путь
	-|-
	Jenkins|`/opt/java/openjdk/lib/security/cacerts`
	Fortify|`/opt/Fortify/Fortify_SCA_and_Apps_21.1.1/jre/lib/security/cacerts`<br>`/usr/lib/jvm/java-11-openjdk-amd64/lib/security/cacerts`
	Прочие|`/etc/default/cacerts`<br>`/etc/ssl/certs/java/cacerts`<br>`/usr/lib/jvm/java-11-openjdk-amd64/lib/security/cacerts`
	
#### Добавление сертификатов в hub-ui

1.	При необходимости запросите сертификат и приватный ключ для AppSec.Hub у администратора (ответственного лица) закрытого контура или создайте свой ключ и передайте его на подпись, а затем дождитесь подписанного сертификата.

2.	Скопируйте полученный сертификат в папку **./ssl**.

		$ ls -al ./ssl/
		total 16
		drwxr-xr-x 2 root   root   4096 Mar  9 14:33 .
		drwxr-xr-x 7 ubuntu ubuntu 4096 Mar 10 07:45 ..
		-rw-r--r-- 1 root   root   1277 Mar  9 14:33 hub.crt
		-rw------- 1 root   root   1679 Mar  9 14:33 hub.key

3.	Измените конфигурацию запуска hub-ui. В секцию volumes добавьте папку, в которой будут храниться ключ и сертификат:

		volumes:
			- ./nginx/hub.conf:/etc/nginx/conf.d/default.conf:ro
			- ./logs/nginx/logs:/var/log/nginx
			- ./ssl:/etc/ssl/certs/ssl-cert:ro

4.	Измените конфигурацию **./nginx/hub.conf** (**hub-ui**) , для использования SSL:

		listen 443 ssl; 
		ssl_certificate /etc/ssl/certs/ssl-cert/hub.crt; # certificate
		ssl_certificate_key /etc/ssl/certs/ssl-cert/hub.key; #  private key

		if ($scheme != "https") {   #uncomment for redirect 80 to 443
			return 301 https://$host$request_uri;
		}

5.	Перезапустите **hub-ui**.

### Запуск python-скриптов

Для запуска python-скриптов, которые будут обращаться к хостам с самоподписанными SSL-сертификатами, необходимо установить следующую переменную окружения:

	REQUESTS_CA_BUNDLE=/etc/ssl/certs/self-signed/rootCA.crt

.