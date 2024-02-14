# Файл ответов для автоматической установки Windows 11 Pro

Данный файл ответов позволяет "из коробки" выполнить автоматическую установку Windows 11 Pro с использованием оригинального образа версии Consumer Edition.

:exclamation: Рекомендуется его использовать на оригинальных образах Windows 11. На репаках тестирование не проводилось.

Файл ответов рассчитан на русскую версию ОС. Изменить его для версии с другим языком не составит большого труда. Сделать это можно в обычном текстовом редакторе путем правок нескольких параметров:

```xml
<component name="Microsoft-Windows-International-Core-WinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<SetupUILanguage>
	<WillShowUI>OnError</WillShowUI>
	</SetupUILanguage>
	<InputLocale>ru-RU</InputLocale>
	<SystemLocale>ru-RU</SystemLocale>
	<UILanguage>ru-RU</UILanguage>
	<UserLocale>ru-RU</UserLocale>
</component>
```

```xml
<component name="Microsoft-Windows-International-Core" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
	<InputLocale>ru-RU; en-US</InputLocale>
	<SystemLocale>ru-RU</SystemLocale>
	<UILanguage>ru-RU</UILanguage>
	<UserLocale>ru-RU</UserLocale>
</component>
```

## Как им пользоваться?

Скопируйте файл autounattend.xml в корень установочной флешки. Всё!<br/>
![explorer_wCmpqtT0fi](https://github.com/vaskov/answer-file-for-windows-11/assets/2478130/cac3e832-1823-4b03-a8fe-a5c5de4d8c5e)

## Что дает данный файл ответов?

С помощью этого файла ответов во время установки Windows пропускаются все окна, такие как:

- Выбор языка во время установки Windows.
- Принятие лицензии.
- Ввод ключа лицензии.
- Выбор редакции ОС.
- Выбор региона.
- Выбор основной и дополнительной раскладки клавиатуры.
- Создание имени учетной записи и пароля.
- Отклонение согласия на сбор телеметрии.
- Обход обязательного подключения к интернету и последующей авторизации с помощью учетной записи Microsoft. Данные требования были применены компанией Microsoft как обязательные в одном из последних билдов Windows 11.
- Игнорируются несовместимые процессоры, пропускается проверка наличия TPM 2, минимального количества RAM, размера системного диска, а также игнорируется Legacy BIOS или UEFI с отключенным Secure Boot.

  За данную проверку отвечает следующий блок, если не нужна проверка, то его можно удалить:

  ```xml
  <RunSynchronous>
  	<RunSynchronousCommand wcm:action="add">
  		<Path>REG ADD HKLM\System\Setup\LabConfig /v BypassTPMCheck /t REG_DWORD /d 1 /f</Path>
  		<Order>1</Order>
  		<Description>Игнорировать отсутствие TPM 2.0</Description>
  		</RunSynchronousCommand>
  		<RunSynchronousCommand wcm:action="add">
  		<Order>2</Order>
  		<Path>REG ADD HKLM\System\Setup\LabConfig /v BypassSecureBootCheck /t REG_DWORD /d 1 /f</Path>
  		<Description>Игнорировать наличие Legacy BIOS, а так же UEFI с отключенным Secure Boot</Description>
  	</RunSynchronousCommand>
  	<RunSynchronousCommand wcm:action="add">
  		<Path>REG ADD HKLM\System\Setup\LabConfig /v BypassRAMCheck /t REG_DWORD /d 1 /f</Path>
  		<Order>3</Order>
  		<Description>Пропустить проверку на соответствие минимальной оперативной памяти</Description>
  	</RunSynchronousCommand>
  	<RunSynchronousCommand wcm:action="add">
  		<Order>4</Order>
  		<Path>REG ADD HKLM\System\Setup\LabConfig /v BypassStorageCheck /t REG_DWORD /d 1 /f</Path>
  		<Description>Не выполнять проверку размера системного диска</Description>
  	</RunSynchronousCommand>
  	<RunSynchronousCommand wcm:action="add">
  		<Order>5</Order>
  		<Path>REG ADD HKLM\System\Setup\LabConfig /v BypassCPUCheck /t REG_DWORD /d 1 /f</Path>
  		<Description>Пропускать проверку процессора на совместимость</Description>
  	</RunSynchronousCommand>
  </RunSynchronous>
  ```

- И др.

Окно выбора и форматирования жесткого диска оставлен для ручного управления, чтобы исключить проблемы с наличием на ПК двух или более дисков и дать возможность выбора на какой носитель устанавливать ОС.
  <br/>
  <br/>
  <br/>
Если вы хотите установить другую редакцию Windows, то вам необходимо поменять индекс на необходимый:

```xml
<MetaData wcm:action="add">
	<Key>/IMAGE/INDEX</Key>
	<Value>4</Value>
</MetaData>
```

Индексы для образа **Consumer Edition**:
- 1 – Windows 11 Домашняя
- 2 – Windows 11 Домашняя для одного языка
- 3 – Windows 11 для образовательных учреждений
- 4 – Windows 11 Pro
- 5 – Windows 11 Pro для образовательных учреждений
- 6 – Windows 11 Pro для рабочих станций

Индексы для образа **Business Edition**:
- 1 – Windows 11 для образовательных учреждений
- 2 – Windows 11 Корпоративная
- 3 – Windows 11 Pro
- 4 – Windows 11 Pro для образовательных учреждений
- 5 – Windows 11 Pro для рабочих станций

<br/>

Для изменения часового пояса поправьте следующий параметр:

```xml
<TimeZone>Ekaterinburg Standard Time</TimeZone>
```
Полные названия часовых поясов можно найти [здесь](https://support.microsoft.com/en-us/topic/time-zone-changes-for-russia-in-windows-43f3d691-0bab-eb64-3ec1-6451d148194c).
  <br/>
  <br/>
  <br/>
```xml
<Password>
	<Value>UABhAHMAcwB3AG8AcgBkAA==</Value>
	<PlainText>false</PlainText>
</Password>
```
Данный параметр означает, что пароль пустой. Не удаляйте и не изменяйте его!
В этой строке пароли записываются в зашифрованном виде, поэтому если прописать пароль в явном виде, например pass123, то система его не примет.
  <br/>
  <br/>
  <br/>
```xml
<SynchronousCommand wcm:action="add">
	<Order>3</Order>
	<CommandLine>powershell -command &quot;(Get-Volume).DriveLetter | Foreach-Object {if (Test-Path &quot;${PSItem}:\reg\tweaks.reg&quot;) {reg load HKEY_USERS\Custom &quot;%systemdrive%\Users\Default\NTUSER.DAT&quot;; reg import &quot;${PSItem}:\reg\tweaks.reg&quot;; reg unload HKEY_USERS\Custom}}&quot;</CommandLine>
	<Description>Импорт настроек из reg-файла (команда взята с outsidethebox.ms)</Description>
</SynchronousCommand>
```

Этот скрипт отвечает за запуск файла tweaks.reg при первом входе в систему. Необходимо создать папку reg в корне флешки и скопировать в нее tweaks.reg. Посмотреть какие изменения он вносит можно внутри, в нем есть комментарии почти по каждому пункту.

При копировании материала просьба ссылаться на данный репозиторий. :pray:
