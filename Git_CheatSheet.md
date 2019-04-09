# Git CheatSheet

### .gitignore
    # java & idea
    .idea/
    target/
    *.iml
    ~*
    *.*~
    *[Oo]ut*

### Git global setup
    git config --global user.name "ФИО"
    git config --global user.email "email@email.ru"

### Create a new repository
    git clone http://...git
    cd alfaipoteka
    touch README.md
    git add README.md
    git commit -m "add README"
    git push -u origin master

### Existing folder
    cd existing_folder
    git init
    git remote add origin http://...git
    git add .
    git commit -m "Initial commit"
    git push -u origin master

### Existing Git repository
    cd existing_repo
    git remote rename origin old-origin
    git remote add origin http://...git
    git push -u origin --all
    git push -u origin --tags
