гит на яндекс диск https://www.borodatiyadmin.ru/udalennyj-repozitorij-git-i-yandex-disk/


в гите это было написано, переработать
…or create a new repository on the command line
echo "# cheatsheets" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/Crenom/cheatsheets.git
git push -u origin master

…or push an existing repository from the command line
git remote add origin https://github.com/Crenom/cheatsheets.git
git push -u origin master

…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.



Как перейти на другую ветку:
git checkout mybranch
git branch --set-upstream-to=origin/mybranch

Удалить файлы из git репозитория, но оставить у себя локально:
https://bogachev.biz/2016/03/28/kak-udalit-papku-idea-i-lishnie-faily-iz-git/
- Добавить файлы в .gitignore
- В idea нажать alt+f12 и выполнить команды в появившейся командной строке
    git rm -r -f --cached .
    git add .
    git commit -m "Remove files"
    git push -u origin master (или другая ветка, из которой удаляем)
    
Как создать .gitignore:
.gitignore можно создавать тут: https://www.gitignore.io

Docker
Конфигурация прокси для докера:
https://www.thegeekdiary.com/how-to-configure-docker-to-use-proxy/ (Method 2)
/etc/systemd/system/docker.service.d -> http-proxy.conf; https-proxy.conf

Надо добавить пользователя в группу докера: sudo usermod -aG docker ${USER}
Потом надо рестартнуть сервис докера: sudo service docker restart

docker rm $(docker ps -aq)






#### если прокся отвалилась
    git config --global http.proxy http://proxy.mycompany:80






#### в гитлабе так:

    Command line instructions

    Git global setup
    git config --global user.name "user"
    git config --global user.email "user@email.ru"

    Create a new repository
    git clone git@gitlab.alfastrah.ru:user/pythonrestapi.git
    cd pythonrestapi
    touch README.md
    git add README.md
    git commit -m "add README"
    git push -u origin master

    Existing folder
    cd existing_folder
    git init
    git remote add origin git@gitlab.alfastrah.ru:user/pythonrestapi.git
    git add .
    git commit -m "Initial commit"
    git push -u origin master

    Existing Git repository
    cd existing_repo
    git remote rename origin old-origin
    git remote add origin git@gitlab.alfastrah.ru:user/pythonrestapi.git
    git push -u origin --all
    git push -u origin --tags
