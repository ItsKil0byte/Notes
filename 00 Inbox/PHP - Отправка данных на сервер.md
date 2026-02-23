Source - https://metanit.com/php/tutorial/3.2.php

- `$_GET` - глобальный ассоциативный массив, куда попадают все параметры из строки запроса.
- `$_POST` - ещё один глобальный ассоциативный массив, куда попадают все данные при POST запросе на сервер.
- Во избежании проблем с безопасностью во время работы с формами рекомендуется использовать функцию htmlentities, что будет экранировать входящие данные.
- Также есть htmlspecialchars, что конвертирует только &, ", ', <, >.
- strip_tags - удаляет только HTML теги.
- Чтобы отправить на сервер массив данных мы можем определить имя массива с квадратными скобками внутри тега input. По умолчанию он индексируется, но можно задать ключи самостоятельно в том же теге.
- Для флажков в форме по умолчанию передаётся значение on. Можно переопрделить значение если указать в теге input атрибут value.
- Переключатели передают на сервер значение, что указывается в атрибуте value.
- При отправке данных из списка мы получаем само значение из option при одиночном выборе, и массив значений при множественном выборе.

```php
// Пример формы
<h2>Анкета</h2>
<form action="input.php" method="POST"> // Файл и метод для обработки
	<p>Введите имя:<br> 
	<input type="text" name="firstname" /></p>
	
	<p>Форма обучения: <br> 
	<input type="radio" name="eduform" value="очно" />очно <br>
	<input type="radio" name="eduform" value="заочно" />заочно </p>
	
	<p>Требуется общежитие:<br>
	<input type="checkbox" name="hostel" />Да</p>
	
	<p>Выберите курсы: <br>
	<select name="courses[]" size="5" multiple="multiple">
	    <option value="ASP.NET">ASP.NET</option>
	    <option value="PHP">PHP</option>
	    <option value="Ruby">RUBY</option>
	    <option value="Python">Python</option>
	    <option value="Java">Java</option>
	</select></p>
	
	<p>Краткий комментарий: <br>
	<textarea name="comment" maxlength="200"></textarea></p>
	<input type="submit" value="Отправить">
</form>
```

```php
// Обработка формы
<?php
if (isset($_POST["firstname"]) && 
	isset($_POST["eduform"]) && 
    isset($_POST["comment"]) && 
    isset($_POST["courses"])) 
{
    $name = htmlentities($_POST["firstname"]);
    $eduform = htmlentities($_POST["eduform"]);
    $hostel = "нет";
    
    if(isset($_POST["hostel"])) $hostel = "да";
    
    $comment = htmlentities($_POST["comment"]);
    $courses = $_POST["courses"];
    $output ="
    <html>
    <head>
    <title>Анкетные данные</title>
    </head>
    <body>
    Вас зовут: $name<br />
    Форма обучения: $eduform<br />
    Требуется общежитие: $hostel<br />
    Выбранные курсы:
    <ul>";
    
    foreach($courses as $item)
        $output.="<li>" . htmlentities($item) . "</li>";
        
    $output.="</ul></body></html>";
    echo $output;
}
else
{   
    echo "Введенные данные некорректны";
}
?>
```

- Отправленные на сервер файлы попадают в ассоциативный массив `$_FILES`. Этот массив является двумерным, то есть его элементы - это набор параметров загруженных файлов. Для их получения можно использовать:
	- `$_FILES["file"]["name"]`: имя файла
	- `$_FILES["file"]["type"]`: тип содержимого файла, например, `image/jpeg`
	- `$_FILES["file"]["size"]`: размер файла в байтах
	- `$_FILES["file"]["tmp_name"]`: имя временного файла, сохраненного на сервере
	- `$_FILES["file"]["error"]`: код ошибки при загрузке
- Если в ключе error находится значение UPLOAD_ERR_OK, то это означает, что файл загрузился без ошибки.
- При загрузке файлы попадают во временное место, после чего их можно переместить с помощью функции move_upload_file. По умолчанию функция перемещает файлы в тот же котолог, где и находится php файл с их обработкой. Можно определить и другой путь, передав его вторым аргументом.
- В файле php.ini можно настроить размер для загружаемых и временных файлов.

```php
// Форма загрузки файлов
<h2>Загрузка файла</h2>
<form method="post" enctype="multipart/form-data">
    <input type="file" name="uploads[]" /><br />
    <input type="file" name="uploads[]" /><br />
    <input type="file" name="uploads[]" /><br />
    <input type="submit" value="Загрузить" />
</form>
```

```php
// Обработка формы
<?php
if($_FILES)
{
    foreach ($_FILES["uploads"]["error"] as $key => $error) {
        if ($error == UPLOAD_ERR_OK) {
            $tmp_name = $_FILES["uploads"]["tmp_name"][$key];
            $name = $_FILES["uploads"]["name"][$key];
            move_uploaded_file($tmp_name, "$name");
        }
    }
    echo "Файлы загружены";
}
?>
```