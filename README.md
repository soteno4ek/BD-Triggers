# Задание 7. Триггеры

## Отчет о тестировании

В ходе работы была решена проблема рассогласованности спец. таблицы и отслеживаемой таблицы. В функцию `get_next_id` добавлена логика создания триггера. Триггер срабатывает при `INSERT` или `UPDATE` в обход функции и автоматически обновляет `current_max_value` в таблице `spec`, если новое значение превышает текущий максимум

### Создание спец. таблицы, триггерной функции и ХП
Создана таблица `spec`, триггерная функция `sync_spec_trigger` и обновленная функция `get_next_id`, которая автоматически навешивает триггер на новую таблицу
<img width="1195" height="813" alt="image" src="https://github.com/user-attachments/assets/9a73b50f-2d51-44ba-8e52-ef958f082700" />

### Создание таблицы test и регистрация в ХП
Создана таблица `test`. При вызове `get_next_id('test', 'id')` функция добавила запись в `spec` и автоматически создала триггер на таблицу `test`
<img width="571" height="200" alt="image" src="https://github.com/user-attachments/assets/7a4bf607-0556-413b-9789-2527dd01112e" />

### Проверка рассинхрона через INSERT
В таблицу `test` напрямую (в обход ХП) вставлено число `100`. Сработал триггер, и значение в `spec` автоматически обновилось до `100`.
<img width="344" height="154" alt="image" src="https://github.com/user-attachments/assets/f05fb4ba-fabb-49b4-bc46-0cbce65f1bec" />

### Вызов ХП после INSERT
Вызов функции `get_next_id('test', 'id')` после прямого добавления вернул `101`, что доказывает, что функция прочитала обновленный максимум из `spec` и не сломалась
<img width="356" height="155" alt="image" src="https://github.com/user-attachments/assets/f96d052c-68b9-4b61-89c6-5463fb08cf3b" />

### Проверка рассинхрона через UPDATE
С помощью прямого `UPDATE` значение в таблице `test` изменено на `500`. Триггер снова сработал и обновил `spec` до `500`
<img width="573" height="198" alt="image" src="https://github.com/user-attachments/assets/7b25d46a-721a-4b65-98bc-da4a0109fb54" />

### Вызов ХП после UPDATE
Вызов функции `get_next_id('test', 'id')` после обновления вернул `501`
<img width="345" height="156" alt="image" src="https://github.com/user-attachments/assets/4e8f393e-a1b7-4445-9ac3-bb35a88ad47c" />
