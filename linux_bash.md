инструкция по настройке интернета через прокси
Проверено на Ubuntu 16.04.2 LTS

Формат записи:
http://login:pass@wg.vesta.ru:8080

Для теста: PROXY_ADDREES = 172.16.29.115

1. Environment
	В конец файла:
		$ vim .bashrc
	Добавить:
		# proxy
		export http_proxy="{{PROXY_ADDREES}}"
		export https_proxy="{{PROXY_ADDREES}}"
	Прочитать файл:
	    $ source .bashrc
	Можно проверить:
		wget http://yandex.ru - должен скачать страничку

2. Certs
	Скопировать сертификаты с расширением .crt в:
		/usr/share/ca-certificates/extra/ (если нет создать)
	Прописать имена добавленных сертфикатов (по аналогии) в файл:
		$ vim /etc/ca-certificates.conf
		like this:
			extra/IssuingCA01.crt
			extra/RootCA.crt
	Выполнить команду:
		$ sudo update-ca-certificates
	Можно проверить:
		wget https://yandex.ru - должен скачать страничку через ssl

3. Утилиты, которые не смотрят переменные окружения
	Некоторые проги надо настраивать собственноручно, если они не смотрят на переменные окружения.
		- Apt-get одна из них. Идем в файл:
			$ sudo vim /etc/apt/apt.conf
		Добавляем запись:
			Acquire::http::Proxy "{{PROXY_ADDREES}}";
		Можно проверить:
			$ sudo apt-get install pip - должен установить pip.