<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Значение submit (input-type)</title>
</head>
<body>
<h1>Пример с кнопкой отправки "type=submit"</h1>
<form action="/examples/php-scripts/coffee.php" method="post">
<fieldset> <legend><b>Какой кофе вы любите?</b></legend>
<label><input type="radio" name="coffee" value="without"> просто кофе (без всего)</label>
<label><input type="radio" name="coffee" value="milk"> с молоком</label>
<p><input type="text" name="coffee_value" placeholder="Свой вариант"></p>
</fieldset>
<p><input type="reset"> <input type="submit" value="Отправить ответ"></p>
</form>
</body>
</html>
