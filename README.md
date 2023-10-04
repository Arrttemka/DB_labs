# 1. Инфо:
Талай Артем, 153505
Тема приложения: Сайт для работы фирмы грузоперевозок
СУБД: Postgre SQL

# 2. Функциональные требования:
  1. Авторизация пользователя.
  2. Управление пользователями (CRUD).
  3. Ролевая система(Клиент, Водитель, Диспетчер, Админ).
  4. Ведение журнала действий пользователя.(Перевозки, Утилизация)
  5. Управление заказами(Диспетчер).
  6. Управление водителями и диспетчерами CRUD (Админ).

# 3. Use-case:
Неавторизованный пользователь:

  Просматривать список водителей
  Просматривать каталог машин
  Авторизовываться 
  
Авторизованный пользователь(Клиент):
  
  Все, что и неавторизованный пользователь
  Совершать заказ
  
Диспетчер:
  
  Все, что и клиент
  CRUD ко всем заказам
  
Админ:

  Все, что и диспетчер
  CRUD ко всем моделям
     
# 4. Список таблиц для базы данных:
  1. "Машины":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Модель (VARCHAR(64), NN)
        Год выпуска (DATETIME, NN)
        ID типа кузова(FOREIGN KEY REFERENCES тип кузова(ID), NN)
    Связи:  
       Связь с таблицей "Тип кузова" в отношении "Многие к одному" (Many-to-One) через поле ID тип кузова.

  2. "Заказы":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Номер заказа (DECIMAL, NN)
        Дата создания (DATETIME, NN)
        Дата доставки (DATETIME, NN)
        Статус заказа (VARCHAR(32), NN)
        ID адреса (INT, FOREIGN KEY REFERENCES Адресы(ID), NN)
        ID клиента (INT, FOREIGN KEY REFERENCES Клиенты(ID), NN)
        ID водителя (INT, FOREIGN KEY REFERENCES Водители(ID), NN)
        ID диспетчера (INT, FOREIGN KEY REFERENCES Диспетчеры(ID), NN)

        
    Связи:
       Связь с таблицей "Адресы" в отношении "Многие к одному" (Many-to-One) через поле ID адреса.
       Связь с таблицей "Клиенты" в отношении "Многие к одному" (Many-to-One) через поле ID клиента.
       Связь с таблицей "Водители" в отношении "Многие к одному" (Many-to-One) через поле ID водители.
       Связь с таблицей "Диспетчеры" в отношении "Многие к одному" (Many-to-One) через поле ID диспетчеры.


  3. "Клиенты":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Номер телефона (CHAR(13), NN) 
        Вес груза (INT, NN)
        ID пользователя (INT, UNIQUE, NN)
   Связи:
       Связь с таблицей "Пользователи" в отношении "Один к одному" (One-to-One) через поле ID клиента.

  4. "Роли":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Название роли (VARCHAR(25), UNIQUE, NN)

  5. "Диспетчер":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Имя (VARCHAR(32), NN)
        Фамилия (VARCHAR(32), NN)
        Номер телефона (CHAR(13), NN) 
        ID фрукта (INT, FOREIGN KEY REFERENCES Фрукты(ID), NN)
        ID клиента (INT, FOREIGN KEY REFERENCES Клиенты(ID), NN)
    Связи:
        Связь с таблицей "Фрукты" в отношении "Многие к одному" (Many-to-One) через поле ID автомобиля.
        Связь с таблицей "Клиенты" в отношении "Многие к одному" (Many-to-One) через поле ID клиента.

  6. "Сотрудники":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Зарплата (INT, NN)
        ID пользователя (INT, UNIQUE, NN)
        
        ID времени (INT, FOREIGN KEY REFERENCES Рабочее время сотрудников(ID))
    Связи:
        Связь с таблицей "Рабочее время сотрудников" в отношении "Многие к одному" (Many-to-One) через поле ID времени.
       Связь с таблицей "Пользователи" в отношении "Один к одному" (One-to-One) через поле ID клиента.
       
  7. "Рабочее время сотрудников":
    
    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Дата и время начала работы (DATETIME, NN)
        Дата и время окончания работы (DATETIME, NN)
        
  8. "Доставка":
      
    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        ID заказа (INT, UNIQUE, NN)
        Дата доставки (DATETIME, NN)
    Связи:
        Связь с таблицей "Заказы" в отношении "Один к одному" (One-to-One) через поле ID заказа.
      
  9. "Производители":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Страна производителя (VARCHAR(50), NN)
        Имя комании (VARCHAR(50), NN)
        
 10. "Пользователи":

    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Имя ((VARCHAR(50), NN)
        Фамилия ((VARCHAR(50), NN)
        Телефон (CHAR(13) ~ '+375[0-9]{9}' NN,)
        Пароль ((VARCHAR(50), NN)
        ID Роли (INT, FOREIGN KEY REFERENCES Роли(ID))
    Связи:
        Связь с таблицей "Роли" в отношении "Многие к одному" (Many-to-One) через поле ID Роли.

  11. "Должности":
    Поля:
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        Название(VARCHAR(50), NN)
      
  12. "Должности/Сотрудники"(вспомогательная):
        ID (INT, PRIMARY KEY, AUTOINCREMENT, NN)
        ID должности(INT, FOREIGN KEY REFERENCES(Должности), NN)
        ID сотрудника(INT, FOREIGN KEY REFERENCES(Сотрудники), NN)
    Связи:
        Связь между Должности/Сотрудники(Many to many) через поля  ID должности и ID сотрудника
      
        


    
# 5. Диаграмма: 

![image](https://github.com/ArseniiShmatov09/Python_Labs_Shmatov_Arsenii_153505/assets/94675119/b0c63305-9511-420c-a901-82a86d353d7b)

![image](https://github.com/ArseniiShmatov09/Python_Labs_Shmatov_Arsenii_153505/assets/94675119/88c304f9-bd17-4379-ad9f-3933c589598f)

