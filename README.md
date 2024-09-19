# GIT tutorial

## Общая информация

Данная инструкция написана как практическая работа к курсу "основы работы с Git".

GIT относится к системам контроля версий(VCS - Version Control System, SCM-Source Control Management) - ПО, помогающему отслеживать изменения в программах, файлах, документах, ... 
* хранит историю изменений в виде отдельных ревизий  
_Ревизия_ - одно изменение или группа изменений, имеет описание того, что изменилось, кто внес изменения и когда, комментарии к изменениям.  
* позволяет манипулировать историей: менять порядок ревизий, полностью удалять версии, возвращаться назад в истории
* позволяет анализировать изменения:кто икогда вносит изменения, кто чащевносит изменения и т.д.

Важная возможность GIT - поддержка параллельной работы нескольких пользователей.

GIT- консольное приложение, оно не имеет интерфейса, поэтому вся работа с ним ведется через командную строку(CLI - command line interface).

GIT немного по-разному работает на Linux/MacOS и Windows. В данной инструкции рассмотрим работу с GIT на Windows.

## Установка GIT на Windows

Для начала надо установить GIT Bash. При установке обязательно выберите галочки, как показано на ![картинке](bash.jpg)

При открытии Git Bash можно будет увидеть сообщение:

```
USER_NAME@HOST_NAME MINGW64 /
$
```

Знак '$' означает, что компьютер  ожидает команду от пользователя.


## Основные команды для работы с командной строкой в GIT Bash

| Команда | Описание |
| ----------- | ----------- |
|```git version```|показывает текущую версию git на компьютере(показатель наличия git на компьютере)|
| ```pwd``` | выводит путь к текущей директории |
| ```cd ~``` | перейти в домашнюю директорию |
|```ls```|вывести содерждимое текущей директории|
|```cd folder```|перейти в папку folder, которая находится в текущей директории|
|```cd ..```|перейти на папку выше|
|```cd .```|обращение к текущей директории для запуска скриптов/программ, которые принимают папку в качестве параметра|
|```cd folder1/folder2```|переход через несколько директорий|
|```ls -a```|вывод расширенного списка содержимого текущей директории(включая скрытые файлы, начинающиеся с ., и файлы . и .., обозначающие текущую и родительскую директории) |
|```ls ~```|вывод содержимого домашней директории  |
|```ls ..```|вывод содержимого родительской директории|
|```touch new_file.extention```|создать в текущей директории файл|
|```mkdir new_folder```|создать в текущей директории папку|
|```mkdir -p new_folder1/new_folder2/.../new_folderN```|создать иерархию папок|
|```cp file path```|скопировать файл file в указанную папку path|
|```cp file1 file2 file3 path```|скопироватьфайлы в указанную папку path  |
|```mv file path```|переместить файл в указанную папку|
|```cat file```|прочитать содержимое файла(работает только с текстовыми файлами!)  |
|```rm file```|удалить файл  |
|```rmdir folder```|удалить пустую папку, если папка непустая, то удаление не произойдет |
|```rm -r folder```|удалить всю иерархию папок |
|```command1 && command2 && command3```|команды к выполнению можно перечислить и они будут выполняться по очереди|
|```cd /```|корневая папка(для Linux/MacOS)|
|```cd /c```|корневая папка(для Windows)|



## Настройка GIT и его подготовка к работе с системой контроля версий

Вся информация о пользователе находится в файле .gitconfig, который лежит в домашней директории.

| Команда | Описание |
| ----------- | ----------- |
|```git config --global user.name "username"```| занесение информации об имени пользователя|
|```git config --global user.email email```|занесение информации об email пользователя|
|```cat ~/.gitconfig```|вывод содержимого файла .gitconfig|
|```git config --list```|вывод более полной информации про параметры конфигурации|

После того, как информация в конфигурационный файл занесена, необходимо сгенерировать SSH-ключи и привязать удаленный репозиторий к локальному.

**SSH(Secure Shell Protocol)** - сетевой протокол, обеспечивающий безопасный обмен данными в сети. С его помощью можно получать данные с удаленного компьютера или отправлять их на него.
Ключи SSH:
* приватный(private key) - используется для расшифровки данных. Хранится на компьютере и никому не передается.
* публичный(public key) - доступен всем и используется для шифрования данных.

Зашифровать данные публичным ключом может любой пользователь, но расшифровать -только владелец приватного ключа. Private и public ключи образуют SSH-пару.

### Генерация SSH-пары и привязывание публичного ключа к GitHub

| Команда | Описание |
| ----------- | ----------- |
|```ls -la .ssh/```|проверка наличия ключей, если ничего не выводится, то переходим к генерации |
|```ssh-keygen -t ed25519 -C email```|генерация пары ключей, где ed25519 - алгоритм шифрования|
|```ssh-keygen -t rsa -b 4096 -C email```|если алгоритм шифрования ed25519 не поддерживается, то используем rsa|
|```ls -a ~/.ssh```|проверка созданных ключей(файл с расширением .pub - публичный ключ, без расширения - приватный)|
|```clip < ~/.ssh/id_rsa.pub```|копирование содержимого(публичного ключа) в буфер обмена|

Для привязки к GitHub необходимо:

1. Зайти в свой аккаунт на GitHub
2. Settings --> SSH and GPG keys --> New SSH key
3. В открывшемся поле ввести скопированный SSH-ключ и сохранить.


### Создание локального и удаленного репозиториев, их привязка друг к другу


Создать папки и файлы локально можно или вручную, или с помощью команд ```mkdir``` и ```touch```  
Удаленный репозиторий создаем на GitHub, его название должно совпадать с названием локального репозитория.

Для привязки репозиториев надо скопировать ssh URL ссылку удаленного репозитория, она представляется в виде git@github.com:Git_username/Git_project_name.git

| Команда | Описание |
| ----------- | ----------- |
|```git init```|делает текущую папку репозиторием в GIT|
|```rm -rf .git```|"разгит" папки, папка прекращает быть репозиторием для GIT|
|```git remote add origin ssh_URL_ссылка```|привязка репозиториев|
|```git remote -v```|проверка того, что репозитории связаны. Флаг ```-v``` равносилен флагу ```--verbose``` и означает показать больше информации при выводе|

Теперь можно работать с репозиториями.


## Работа с файлами и изменениями в GIT

| Команда | Описание |
| ----------- | ----------- |
|```git status```|показывает текущее состояние репозитория и папок/файлов в ней|
|```git add file```|добавить файл в отслеживание, вызывать после каждого сохранения для актуализации версии файла к дальнейшему коммиту|
|```git add --all```|добавить все файлы в отслеживание|
|```git add .```|добавить все файлы в отслеживание|
|```git commit -m'message for commit'```|закоммитить измененияс определенным сообщением(обычно в нем говорится что было сделано)|
|```git log```|история коммитов, показывает комментарии, с которыми были закоммичены изменения|
|```git push -u origin master/main```|загружает все изменения на GitHub. master/main-название ветки, в которую идет коммит. Далее можно использовать команду ```git push origin master/main``` или ```git push```|