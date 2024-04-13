# Лабораторная работа по Git и GitHub

## **ПРЕДИСЛОВИЕ**:

**Проверка установки `ssh-keygen`:**
   Откройте терминал и выполните команду `ssh-keygen`. Если у вас откроется помощь по использованию `ssh-keygen`, значит он установлен.

После выполнения этих шагов вы сможете использовать `ssh-keygen` для генерации SSH-ключей на вашей платформе.

### Установка `ssh-keygen` на Windows:

### Windows 10:

1. Откройте "Параметры", выберите "Система", затем выберите "Дополнительные функции".
2. Прокрутите список, чтобы увидеть, установлен ли уже OpenSSH. Если нет, вверху страницы выберите "Добавить функцию", затем:
   - Найдите "Клиент OpenSSH", затем выберите "Установить".
   - Найдите "Сервер OpenSSH", затем выберите "Установить".
3. Откройте приложение "Службы". (Выберите "Пуск", введите "services.msc" в строке поиска, затем выберите приложение "Службы" или нажмите "Ввод".)
4. В области сведений дважды щелкните "Сервер SSH OpenSSH".
5. На вкладке "Общее" из списка "Тип запуска" выберите "Автоматически", затем выберите "ОК".
6. Чтобы запустить службу, выберите "Запустить".

### Windows 11:

1. Откройте "Параметры", выберите "Система", затем выберите "Дополнительные функции".
2. Прокрутите список, чтобы увидеть, установлен ли уже OpenSSH. Если нет, вверху страницы выберите "Просмотреть функции", затем:
   - Поиск "Клиент OpenSSH", выберите "Далее", затем выберите "Установить".
   - Поиск "Сервер OpenSSH", выберите "Далее", затем выберите "Установить".
3. Откройте приложение "Службы". (Выберите "Пуск", введите "services.msc" в строке поиска, затем выберите приложение "Службы" или нажмите "Ввод".)
4. В области сведений дважды щелкните "Сервер SSH OpenSSH".
5. На вкладке "Общее" из списка "Тип запуска" выберите "Автоматически", затем выберите "ОК".
6. Чтобы запустить службу, выберите "Запустить".

### POWERSHELL:

Для установки OpenSSH с использованием PowerShell запустите PowerShell от имени администратора. Чтобы убедиться, что OpenSSH доступен, выполните следующий cmdlet:

```
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
The command should return the following output if neither are already installed:
```

#### Вывод

      Copy
      Name  : OpenSSH.Client~~~~0.0.1.0
      State : NotPresent

      Name  : OpenSSH.Server~~~~0.0.1.0
      State : NotPresent
      Then, install the server or client components as needed:

#### Установка клиента OpenSSH
```
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```
#### Установка сервера OpenSSH
```
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Both commands should return the following output:
```
#### Вывод
      Copy
      Path          :
      Online        : True
      RestartNeeded : False
      To start and configure OpenSSH Server for initial use, open an elevated PowerShell prompt (right click, Run as an administrator), then run the following commands to start the sshd service:

#### Запуск службы sshd
```
Start-Service sshd
```
#### Рекомендуется:

#### Подтвердите, что правило брандмауэра настроено. Оно должно быть создано автоматически во время установки. Выполните следующее, чтобы проверить.
```
Set-Service -Name sshd -StartupType 'Automatic'

if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```
### Установка `ssh-keygen` на macOS:

1. **Установка через терминал:**
   - macOS поставляется с предустановленным `ssh-keygen`.
   - Откройте Terminal из папки "Applications/Utilities".
   - Выполните команду `ssh-keygen` для проверки установки.

### Установка `ssh-keygen` на Linux:

1. **Установка через пакетный менеджер:**
   - В большинстве дистрибутивов Linux `ssh-keygen` поставляется вместе с пакетом OpenSSH.
   - Выполните команду в терминале для установки OpenSSH:

     - **Debian/Ubuntu:**
       ```bash
       sudo apt update
       sudo apt install openssh-client
       ```

     - **Red Hat/CentOS:**
       ```bash
       sudo yum install openssh-clients
       ```

     - **Arch Linux:**
       ```bash
       sudo pacman -Sy openssh
       ```


# Настройка SSH подключения между локальным Git и внешним GitHub:

 1. Генерация SSH-ключа (если его нет):
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
 2. Добавление SSH-ключа в ssh-agent (если используется):
 ```
eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa
```
 3. Добавление SSH-ключа в настройки GitHub:
    1. Войдите в свой аккаунт на GitHub.

    2. Перейдите в настройки профиля
(В верхнем правом углу вашей страницы нажмите на ваш аватар и выберите "Settings" из выпадающего меню.
)
    3. Выберите "SSH and GPG keys" в меню слева.

    4. Нажмите на кнопку "New SSH key" или "Add SSH key".

    5. Вставьте ваш публичный SSH-ключ в поле "Key".
    6. Вставьте скопированный ранее публичный SSH-ключ в это поле.

    7. Укажите заголовок (Title) для ключа.
(    Заголовок поможет вам идентифицировать этот ключ на GitHub. Например, "My Laptop" или "Work Desktop".)

    8. Нажмите на кнопку "Add SSH key" или "Add key".
    Это добавит ваш SSH-ключ в ваш аккаунт GitHub.

 Добавление других пользователей в контрибьютеры:
 1. Пользователь должен иметь аккаунт на GitHub.
 2. В репозитории перейдите в настройки (Settings) -> Manage access.
 3. Нажмите на кнопку "Invite a collaborator" и введите имя пользователя или их email.
 4. Пользователь получит приглашение и сможет принять его на странице своего аккаунта GitHub.
 5. После принятия приглашения пользователь сможет управлять репозиторием, включая создание веток, коммиты и т.д.

## ЗАДАНИЯ
### Задание 1: Основы работы с Git

**Цель:** Познакомиться с основными командами Git и понять базовые концепции контроля версий.

1. Создайте пустой репозиторий на GitHub.
2. Для входа в аккаунт на локальном компьютере используйте SSH-ключи. Для этого выполните команду:
   ```
   ssh-keygen -t rsa -b 4096 -C "ваш_email@example.com"
   ```
   Следуйте инструкциям для создания SSH-ключей. После этого добавьте ваш публичный ключ в настройки вашего аккаунта на GitHub.
3. Склонируйте репозиторий на свой компьютер с использованием SSH протокола, выполните команду:
   ```
   git clone git@github.com:пользователь/репозиторий.git
   ```
   Замените `пользователь` и `репозиторий` на соответствующие значения.
4. Создайте новый текстовый файл `README.md`.
5. Добавьте несколько строк текста в файл `README.md`.
6. Используя команды Git, добавьте изменения в индекс:
   ```
   git add README.md
   ```
7. Зафиксируйте изменения с комментарием "Добавлен README.md":
   ```
   git commit -m "Добавлен README.md"
   ```
8. Отправьте изменения в удаленный репозиторий на GitHub:
   ```
   git push origin master
   ```

**ВАЖНО:** Разделитеся на группы 2-5 чел и работать параллельно над одним файлом. Решить как вы будете разрешать конфликты в случае возникновения.

---

### Задание 2: Работа с ветками

Цель: Понять концепцию ветвления в Git и научиться создавать, переключаться и объединять ветки.

1. Создайте новую ветку с названием "feature/additional-content":
   ```bash
   git checkout -b feature/additional-content
   git status
   ```

2. Внесите изменения в файл README.md, добавив еще несколько строк текста. После внесения изменений выполните:
   ```bash
   git add README.md
   git status
   git commit -m "Добавлены дополнительные сведения"
   git status
   ```

3. Переключитесь обратно на ветку master:
   ```bash
   git checkout master
   git status
   ```

4. Создайте ветку hotfix/typo-fix для исправления опечатки в файле README.md:
   ```bash
   git checkout -b hotfix/typo-fix
   git status
   ```

5. Исправьте опечатку в файле README.md и зафиксируйте изменения. После фиксации выполните:
   ```bash
   git status
   ```

6. Переключитесь обратно на ветку master:
   ```bash
   git checkout master
   git status
   ```

7. Объедините ветку hotfix/typo-fix с веткой master:
   ```bash
   git merge hotfix/typo-fix
   git status
   ```

8. **Дополнительное задание**: Попробуйте выполнить слияние двух созданных вами веток: feature/additional-content и hotfix/typo-fix. Решите возможные конфликты, которые могут возникнуть при слиянии.

---

#### Задание 3: Работа с GitFlow (дополнительно)

**Цель:** Познакомиться с методологией GitFlow и освоить основные концепции работы с ней.

1. Ознакомьтесь с основными концепциями GitFlow, такими как основные ветки (`master` и `develop`), вспомогательные ветки (`feature`, `release`, `hotfix`).
2. Создайте новую ветку `develop` на вашем локальном репозитории:
   ```
   git checkout -b develop
   ```
3. Создайте ветку `feature/additional-feature` для разработки новой функциональности:
   ```
   git flow feature start additional-feature
   ```
4. Внесите несколько изменений в проект, связанных с добавлением новой функциональности.
5. Завершите работу над веткой `feature/additional-feature`:
   ```
   git flow feature finish additional-feature
   ```
6. Создайте ветку `release/v1.0.0` для подготовки нового релиза:
   ```
   git flow release start v1.0.0
   ```
7. Проведите необходимые тесты и исправьте ошибки, если они обнаружатся.
8. Завершите работу над релизной веткой:
   ```
   git flow release finish v1.0.0
   ```
9. Отправьте изменения в удаленный репозиторий:
   ```
   git push origin develop
   git push origin master
   ```

**Дополнительное задание :** Попробуйте использовать GitFlow для управления разработкой вашего проекта в команде. Разделите роли между участниками команды (например, кто будет отвечать за создание и завершение фичей, кто за релизы и т.д.) и следуйте процессу GitFlow.
