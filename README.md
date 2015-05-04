# itcod-search
SOA ITCOD. SEARCH module for Nginx.

SOA ITCOD

Сервис "search" 


Назначение: Обеспечить поиск отбор в закрытых пользовательских массивах и выполнение простейших статистических операций с формированием результатов. Операции выполняются над пользовательскими массивами к которым владелец разрешил доступ сервису "search".

Массив это группа любых файлов в определённой папке. Доступ разрешается наличием в папке файла .htsearch 

c строками разрешённых действий (пример: all:all - разрешен доступ ко всем файлам и любой метод обработки полученных данных)



Параметры (при разработке ПО п1-4 желательно выносить в конфигурационный файл)

1. путь к сервису http://home.itcod.com/search/

2. идентификатор хранителя массива (login/ИНН/etc) IDUser=000000000000

3. идентификатор массива в котором выполнить поиск IDCatalog=DosieMD5

4. шаблон отбора имён карточек по маске (формат паттернов lua) pattern=[%a%d+]-.%.atrib

5. метод:тип:ключ:[] и метод исполнения key=:file 

	:file - над списком отобранных по маске файлов, 

	:itcod:*:*[:$LNG$] - над ключами внутри объектов itcod (в [] необязательный параметр)

	:itcod:usr:$KEY$[:$LNG$]

	:itcod:sys:$KEY$[:$LNG$] 

	:itcod:all:$KEY$[:$LNG$] 

	:itcod:vas:$KEY$[:$LNG$] 

6. выполняемое правило rule=list 

list - список найденых файлов (аналогично list:name),

list:[$CRYPT$] - список файлов с хэшем openssl (имяфайла = MD5),

     [$CRYPT$] md5 md4 sha1 sha ripemd160

sum - сумма, 

union - текстовое последовательное соединение


Формат запроса 1?2&3&4&5&6

Запрос выполняется только методом GET. Примеры см. ниже.

При неверном запросе/запрете доступа к массиву(п3)/методам исполнения(п5)/отсутствии файла сервис возвращает код 403


ПРИМЕРЫ ПО получению данных из единого массива карточек DosieMD5


ЧАСТЬ 1 Общие статистические данные

1.1. показать список всех карточек .atrib

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-.%.atrib&key=:file&rule=list



1.1.1 показать список всех карточек .atrib c MD5 файлов

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-.%.atrib&key=:file&rule=list:md5



1.1.2 показать список всех карточек .atrib c SHA1 файлов

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-.%.atrib&key=:file&rule=list:sha1



1.2. показать кол-во карточек .atrib

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-.%.atrib&key=:file&rule=sum



1.3. показать список карточек .atrib по ИНН 4443022434 (Дворец Творчества г. Кострома)

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-4443022434.atrib&key=:file&rule=list



1.4. показать сумму карточек .atrib по ИНН 4443022434

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-4443022434.atrib&key=:file&rule=sum



ЧАСТЬ 2 Статистические разрезы по ключевым параметрам 



2.1. показать список данных из ключей CountBudget карточек .atrib по ИНН 4443022434

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-4443022434.atrib&key=:itcod:usr:CountBudget&rule=list



2.2. показать сумму ключей CountBudget карточек .atrib по ИНН 4443022434

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-4443022434.atrib&key=:itcod:usr:CountBudget&rule=sum



2.3. показать текстовое последовательное соединение данных ключей CountBudget карточек .atrib по ИНН 4443022434

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=[%a%d+]-4443022434.atrib&key=:itcod:usr:CountBudget&rule=union



2.4. тестовый блок проверки суммирования CountBudget двух различных организаций

http://home.itcod.com/search/?IDUser=000000000000&IDCatalog=DosieMD5&pattern=^79bbeeef658b2a63fdb5eaea9f1941d2-.+%.atrib$&key=:itcod:usr:CountBudget&rule=sum



ОТВЕТ html (данные ответа в блоке START-STOP)

<b>SOA ITCOD<br>SERVICE "SEARCH"<br><br>ANSWER THE QUESTION</b><hr><br>

<!-- START REPLY  -->

2

<!-- STOP REPLY -->


-- в разработке --

	:itcod:sys:$KEY$[:$LNG$] 

	:itcod:all:$KEY$[:$LNG$] 

	:itcod:vas:$KEY$[:$LNG$] 

