# Задание 7. Триггеры

## Отчет о тестировании

В ходе работы была решена проблема рассогласованности спец. таблицы и отслеживаемой таблицы. В функцию `get_next_id` добавлена логика создания триггера. Триггер срабатывает при `INSERT` или `UPDATE` в обход функции и автоматически обновляет `current_max_value` в таблице `spec`, если новое значение превышает текущий максимум

### Создание спец. таблицы, триггерной функции и ХП
Создана таблица `spec`, триггерная функция `sync_spec_trigger` и обновленная функция `get_next_id`, которая автоматически навешивает триггер на новую таблицу
<img width="1110" height="719" alt="image" src="https://github.com/user-attachments/assets/a34e9734-899d-4ee1-a7e6-a239194624ce" />

### Создание таблицы test и регистрация в ХП
Создана таблица `test`. При вызове `get_next_id('test', 'id')` функция добавила запись в `spec` и автоматически создала триггер на таблицу `test`
<img width="475" height="215" alt="image" src="https://github.com/user-attachments/assets/25f313f1-c9cd-4f2a-b6d5-d8f2c04dbc06" />

### Проверка рассинхрона через INSERT
В таблицу `test` напрямую (в обход ХП) вставлено число `100`. Сработал триггер, и значение в `spec` автоматически обновилось до `100`.
<img width="473" height="207" alt="image" src="https://github.com/user-attachments/assets/1610ecee-3769-46d1-afdd-0d7e46bd6603" />

### Вызов ХП после INSERT
Вызов функции `get_next_id('test', 'id')` после прямого добавления вернул `101`, что доказывает, что функция прочитала обновленный максимум из `spec` и не сломалась
<img width="342" height="159" alt="image" src="https://github.com/user-attachments/assets/3c0072ef-47ea-433b-8ac7-f10f7e0fe18f" />

### Проверка рассинхрона через UPDATE
С помощью прямого `UPDATE` значение в таблице `test` изменено на `500`. Триггер снова сработал и обновил `spec` до `500`
<img width="473" height="207" alt="image" src="https://github.com/user-attachments/assets/a7528134-38cb-4b8e-a59f-8a80ed4a2b9b" />

### Вызов ХП после UPDATE
Вызов функции `get_next_id('test', 'id')` после обновления вернул `501`
<img width="344" height="162" alt="image" src="https://github.com/user-attachments/assets/cefe5f91-7936-4ec6-b72d-3aec750702d0" />
