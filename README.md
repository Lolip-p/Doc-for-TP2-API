# Doc-for-TP2-API
Это документация к API от https://t.me/vaIerkaaaaaaa для TP2

## Установка:
1. Скачать API и положить в папку, чтобы на пути не было русских символов! Пример: `C:\TP2_Maps`
2. Скачать эмулятор Android на выбор (Nox, BlueStacks и т.д.), главное получить Root права.
3. Скачать на эмулятор игру `TP2` (зайти в игру) и `Total Commander`
4. В Total Commander идём по пути `data\data\com.BFGGames.Teleportal2\shared_prefs`
5. Копируем файл `com.BFGGames.Teleportal2.v2.playerprefs.xml` на пк в нашу папку `C:\TP2_Maps`
6. Создаём файл `config.py` и пишем в него следующий код:
```
game_name = "Teleportal2"
studio = "BFGGames"

original_path = fr"C:\TP2_Maps\com.{studio}.{game_name}.v2.playerprefs.xml"
modified_path = fr"C:\TP2_Maps\map_mod\com.{studio}.{game_name}.v2.playerprefs.xml"
```
7. В строку `original_path` пишем путь к файлу `com.BFGGames.Teleportal2.v2.playerprefs.xml`
8. В строку `modified_path` пишем путь в выходную папку. Например в `C:\TP2_Maps\map_mod`
9. Создаём файл `map_editor.py` и пишем в него следующий код:
```
from tp2_save_api import *
import config

name = "TP2"
prefs_folder_path = f"PlayerPrefs{name}"

decode_save_to_folder(config.original_path, prefs_folder_path, encoding="utf-8")

encode_save_from_folder(prefs_folder_path, config.modified_path, encoding="utf-8")
```
10. Запускаем `map_editor.py` и в папке появятся новые файлы. В конце концов всё должно выглядеть так:
![image](https://github.com/Lolip-p/Doc-for-TP2-API/assets/95537683/84c61fc5-f30a-41e9-9b15-0729cfa58f3d)

## Использование:
- Создаём экземпляр класса Level `level = Level()` и вызваем у него один из трёх методов: `load_from_string(level_string)`, `load_from_file(path)`, `load_by_level_index(playerprefs_folder, level_pack_index, level_index)`
- Создание объектов или стен: `create_object(name, **kwargs)`, `create_wall(name, **kwargs)`, `name` - берётся из соответствующего словаря, `**kwargs` - это сами атрибуты объекта (позиция, поворот и т.д.)
- Добавление объектов или стен на уровень: `Level.add_object(self, obj)`, `Level.add_objects(self, objects)`, `Level.add_wall_segment(self, wall_segment)`, `Level.add_wall_segments(self, wall_segments)`
- Конвертирует уровень в json файл (чтобы всё работало как надо)

Нихера не понятно? Не ссать, вот пример кода с комментариями:
```
from tp2_save_api import *
import config

name = "TP2"
prefs_folder_path = f"PlayerPrefs{name}"

decode_save_to_folder(config.original_path, prefs_folder_path, encoding="utf-8") # Декодирует файл

level = Level()
level.load_by_level_index(prefs_folder_path, 0, 1) # Вытаскиваем 1 уровень из пака карт 0

#create lvl
new_object = create_object("Start", position=Vector3(200, 200, 200), rotation=ObjectRotation.down())
level.add_object(new_object)
level.add_objects([new_object, new_object])

# convert
level.convert_to_json_file(prefs_folder_path, 0, 1) # Записываем наши изменения в 1 уровень из пака карт 0

encode_save_from_folder(prefs_folder_path, config.modified_path, encoding="utf-8") # Кодирует файл обратно, чтобы Unity схавал его.
```
