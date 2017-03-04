Text cleaner
============

I cleaned the text using a small C # program that I wrote in the evening. I'm not going to put the source code here at this time, because it needs to be optimized and improved.

But I'm putting out a step-by-step algorithm so that you can repeat the same as needed.

Input parameters: file name, text language (English or Russian).

1. Open the file, read all content in `input` list
2. Start the walk cycle of all rows in the `input` list (clears from bad characters)
    1. Decode string using `System.Web.HttpUtility.HtmlDecode` method
    2. If the text is in English, it starts an analogy [php function iconv //TRANSLIT](http://stackoverflow.com/questions/2173825/slugify-and-character-transliteration-in-c-sharp): `Encoding.ASCII.GetString(Encoding.GetEncoding("Cyrillic").GetBytes(line))`
    3. Replace all HTML tags with ` ` (space) by using a regular expression: `<[^>]*>`
    4. Replace all links/URLs on ` ` (space) by using a regular expression: `(http|ftp|https|www)(:\/\/|)([\w+?\.\w+])+([a-zA-Z0-9\~\!\@\#\$\%\^\&\*\(\)_\-\=\+\\\/\?\.\:\;\'\,]*)?`
    5. If the text is in Russian, then check whether English characters are in the current line using the regular expression: `[a-z]` - if any, skip this line
    6. If the text is in English, then replace all the symbols ` ` (space) by regular expression: `[^a-z\d\n\r\!\#\$\%\&\*\(\)\-\=\№\;\:\?\+\,\. ]`
    7. If the text is in Russian, then replace all the symbols ` ` (space) by regular expression: `[^а-яё\d\n\r\!\#\$\%\&\*\(\)\-\=\№\;\:\?\+\,\. ]`
    8. Replace character `\r` to ` ` (space)
    9. Replace character `\n` to ` ` (space)
    10. Replace by regular expression `[ |  |   ]([\:\.\,\!\%]{1})` to group №1 ($1)
    11. If the text is in English, then replace symbol `&` to `and`
    12. Replace symbols `--` to `-`
    13. Replace double white space with single
    14. Applying the `Trim()` method to the current line
    15. If the string is not empty, add it to a temporary list `tmp`
3. Remove duplicates from the list `tmp`
4. Set the variable `all_length` = `0`
5. Set the variable `elements_count` = count elements in the list `tmp`
6. Start the walk cycle of all rows in the list `tmp` (Average length count)
    1. To variable `all_length` add the current line length
7. Set the variable `min_length` = `all_length / elements_count`
8. Starts the walk cycle of all rows in the `tmp` (Get final Result)
    1. If the length of the current line >= `min_length`, then add it to the list `output`
9. Unite elements of the `output` list to `output_content` variable with `\n` separator
10. Save the contents of a variable `output_content` in file



Очистка текста
==============

Я чистил текст, используя небольшую C# программу, которую написал за вечер. В данный момент я не буду выкладывать тут её исходный код, так как его нужно оптимизировать и улучшить.

Но я выкладываю пошаговый алгоритм, чтобы вы при необходимости могли повторить у себя тоже самое.

Входящие параметры: имя файла, язык текста (английский или русский).

1. Открыть файл, прочитать всё содержимое в список `input`
2. Запуск цикла обхода всех строк в списке `input` (очистка от плохих символов)
    1. Декодируем строку с помощью `System.Web.HttpUtility.HtmlDecode`
    2. Если текст на английском, то запускаем аналог [php функции iconv //TRANSLIT](http://stackoverflow.com/questions/2173825/slugify-and-character-transliteration-in-c-sharp): `Encoding.ASCII.GetString(Encoding.GetEncoding("Cyrillic").GetBytes(line))`
    3. Замена всех html-тегов на ` ` (пробел) с помощью регулярного выражения: `<[^>]*>`
    4. Замена всех ссылок/url на ` ` (пробел) с помощью регулярного выражения: `(http|ftp|https|www)(:\/\/|)([\w+?\.\w+])+([a-zA-Z0-9\~\!\@\#\$\%\^\&\*\(\)_\-\=\+\\\/\?\.\:\;\'\,]*)?`
    5. Если текст на русском, то проверяем есть ли в текущей строке английские символы с помощью регулярного выражения: `[a-z]` - если они есть, то пропускаем эту строку
    6. Если текст на английском, то заменяем все символы на ` ` (пробел) по регулярному выражению: `[^a-z\d\n\r\!\#\$\%\&\*\(\)\-\=\№\;\:\?\+\,\. ]`
    7. Если текст на русском, то заменяем все символы на ` ` (пробел) по регулярному выражению: `[^а-яё\d\n\r\!\#\$\%\&\*\(\)\-\=\№\;\:\?\+\,\. ]`
    8. Замена символа `\r` на ` ` (пробел)
    9. Замена символа `\n` на ` ` (пробел)
    10. Замена по регулярному выражению `[ |  |   ]([\:\.\,\!\%]{1})` на группу №1 ($1)
    11. Если текст на английском, то заменяем символ `&` на `and`
    12. Замена символа `--` на `-`
    13. Замена двойного пробела на одиночный
    14. Применение метода `Trim()` к текущей строке
    15. Если строка не пустая, то добавляем её во временный список `tmp`
3. Удалить дубли из списка `tmp`
4. Присвоить переменной `all_length` = `0`
5. Присвоить переменной `elements_count` = количество элементов в списке `tmp`
6. Запуск цикла обхода всех строк в списке `tmp` (подсчет средней длинны строки)
    1. К переменной `all_length` прибавить длину текущей строки
7. Присвоить переменной `min_length` = `all_length / elements_count`
8. Запуск цикла обхода всех строк в списке `tmp` (получение итогового результата)
    1. Если длина текущей строки >= `min_length`, то добавляем её в список `output`
9. Объеденить элементы списка `output` в переменную `output_content` с разделителем `\n`
10. Сохранить содержимое переменной `output_content` в файл