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


    $ df -h - узнать куда примаунчен диск

    $ df -h
    Файловая система      Разм  Исп  Дост  Исп% смонтирована на
    /dev/sda3              70G   29G   38G  43% /
    udev                  1,9G  236K  1,9G   1% /dev
    /dev/sda1             107M   25M   77M  24% /boot
    /dev/sdb1             466G  452G   14G  97% /home
    
    $ df -h /boot/grub/
    Файловая система      Разм  Исп  Дост  Исп% смонтирована на
    /dev/sda1             107M   25M   77M  24% /boot
    
    $ df -h /usr/local/
    Файловая система      Разм  Исп  Дост  Исп% смонтирована на
    /dev/sda3              70G   29G   38G  43% /
    
    $ df -h ~/tmp
    Файловая система      Разм  Исп  Дост  Исп% смонтирована на
    /dev/sdb1             466G  452G   14G  97% /home




---------
for i in $(find . -name "*.war" -printf "%f\n" | cut -d. -f1); do echo $(mvn -Dmaven.test.skip=true -pl $i tomcat7:redeploy -Ppprd); done;
