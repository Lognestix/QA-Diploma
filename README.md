# Дипломный проект по профессии «Тестировщик»
**Дипломный проект представляет собой автоматизацию тестирования комплексного сервиса, взаимодействующего с СУБД и API Банка.** 
 
Приложение представляет собой веб-сервис, который предоставляет возможность купить тур по определённой цене с помощью двух способов:
1. Обычная оплата по дебетовой карте
1. Уникальная технология: выдача кредита по данным банковской карты

**В процессе работы над данным проектом были созданы следующие документы:**
- [План автоматизации тестирования](https://github.com/Lognestix/QA-Diploma/blob/master/docs/Plan.md)
- [Отчет о проведенном тестировании](https://github.com/Lognestix/QA-Diploma/blob/master/docs/Report.md)
- Отчет о проведённой автоматизации тестирования

## Предварительные требования
1. Получить доступ к удаленному серверу
1. На удаленном сервере должны быть установлены и доступны:
	- GIT
	- Docker	
	- Bash
1. На компьютере пользователя должна быть установлена:
	- Git Bash
	- Intellij IDEA
## Подготовка и запуск
1. Залогиниться на удаленный сервер
1. Клонировать проект на удаленный сервер командой:
	```
	git clone https://github.com/Lognestix/QA-Diploma
	```
1. Перейти в созданный каталог командой:
	```
	cd QA-Diploma
	```
1. Создать и запустить необходимые Docker Container'ы командой:
	```
	docker-compose -p qa-diploma up -d --force-recreate
	```
	`где:`
	- `-p qa-diploma` - присваивает указанный префикс контейнерам (имя проекта);
	- `-d` - включает автономный режим (режим демона);
	- `--force-recreate` - принудительно пересоздает все контейнеры.
1. Клонировать проект на свой компьютер:
	- открыть терминал (командная строка) и ввести команду:	
		```
		git clone https://github.com/Lognestix/QA-Diploma
		```
1. Открыть с клонированный проект в Intellij IDEA
### Для работы с базой данных MySQL
**Проект пред настроен под работу с базой данных MySQL развернутой по ip-адресу 185.119.57.164.**

**Как изменить ip-адрес описано в разделе "Для работы с базой данных PostgreSQL", для MySQL - делается аналогично.**
1. Запустить SUT во вкладке Terminal Intellij IDEA командой:
	```
	java -jar artifacts/aqa-shop.jar
	```
	**Дождаться появления строки:**  
	`ru.netology.shop.ShopApplication         : Started ShopApplication in 17.116 seconds (JVM running for 19.968)`	
1. Для запуска авто-тестов в Terminal Intellij IDEA открыть новую сессию и ввести команду:
	```
	./gradlew clean test allureReport -Dheadless=true
	```
	`где:`
	- `allureReport` - подготовка данных для отчета Allure;
	- `-Dheadless=true` - запускает авто-тесты в headless-режиме (без открытия браузера).
1. Для просмотра отчета Allure в терминале ввести команду:
	```
	./gradlew allureServe
	```
### Для работы с базой данных PostgreSQL
1. Запустить SUT, можно несколькими способами:  
	**`Способ №1:`**
	- В находящемся в проекте файле application.properties закомментировать строку ниже "# For MySQL" и раскомментировать строку ниже "# For PostgreSQL", выглядеть будет так:
		```
		# For MySQL
		# spring.datasource.url=jdbc:mysql://185.119.57.164:3306/base_mysql
		# For PostgreSQL
		spring.datasource.url=jdbc:postgresql://185.119.57.164:5432/base_postgresql
		```
		`где:`
		- `185.119.57.164` - ip-адрес удаленной машины с развернутой PostgreSQL, в случае необходимости заменить на ip-адрес своей удаленной машины с развернутой PostgreSQL.
	- Далее во вкладке Terminal Intellij IDEA запустить SUT командой:
		```
		java -jar artifacts/aqa-shop.jar
		```
	**`Способ №2:`**
	- Запустить SUT во вкладке Terminal Intellij IDEA командой:
		```
		java -D:spring.datasource.url=jdbc:postgresql://185.119.57.164:5432/base_postgresql -jar artifacts/aqa-shop.jar
		```
		`где:`
		- `185.119.57.164` - ip-адрес удаленной машины с развернутой PostgreSQL, в случае необходимости заменить на ip-адрес своей удаленной машины с развернутой PostgreSQL.
		
	**Не зависимо от способа дождаться появления строки:**  
	`ru.netology.shop.ShopApplication         : Started ShopApplication in 17.116 seconds (JVM running for 19.968)`	
1. Запустить авто-тесты, также можно несколькими способами:  
	**`Способ №1:`**
	- В находящемся в проекте файле build.gradle в разделе test закомментировать строку ниже "//Для работы с базой данных mySQL" и раскомментировать строку ниже "//Для работы с базой данных postgreSQL", выглядеть будет так:
		```
		//Для работы с базой данных mySQL (со строки ниже необходимо снять комментарий):
		//systemProperty 'datasource', System.getProperty('datasource', 'jdbc:mysql://185.119.57.164:3306/base_mysql')
		//Для работы с базой данных postgreSQL (со строки ниже необходимо снять комментарий):
		systemProperty 'datasource', System.getProperty('datasource', 'jdbc:postgresql://185.119.57.164:5432/base_postgresql')
		```
		`где:`
		- `185.119.57.164` - ip-адрес удаленной машины с развернутой PostgreSQL, в случае необходимости заменить на ip-адрес своей удаленной машины с развернутой PostgreSQL.
	- Применить изменения (Ctrl+Shift+O);
	- Далее в Terminal Intellij IDEA открыть новую сессию и ввести команду:
		```
		./gradlew clean test allureReport -Dheadless=true
		```
	**`Способ №2:`**
	- Открыв новую сессию в Terminal Intellij IDEA запустить авто-тесты командой:
		```
		./gradlew clean test allureReport -Dheadless=true -Ddatasource=jdbc:postgresql://185.119.57.164:5432/base_postgresql
		```
		`где:`
		- `185.119.57.164` - ip-адрес удаленной машины с развернутой PostgreSQL, в случае необходимости заменить на ip-адрес своей удаленной машины с развернутой PostgreSQL.
1. Для просмотра отчета Allure в терминале ввести команду:
	```
	./gradlew allureServe
	```