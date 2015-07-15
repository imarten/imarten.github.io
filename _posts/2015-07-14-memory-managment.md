---
layout  : post
title   : Управление памятью

---

В JS память выделяется динамически при создание сущностей(объектов, строк, ...) и автоматически освобождается когда больше не используется.  
Для очистки памяти в браузерах истольуются *Сборщик мусора* (англ. Garbage collection, GC), встроенный в интерпретатор, который наблюдает за объектами и время от времени удаляет недостежимые.

### Жизненный цикл памяти
1. Выделение необходимой памяти
2. Ее использование (чтение, запись)
3. Освобождение

Интерпретатор JS динамически выделяет необходимую помять при объявлении переменных

` var i = 5; // выделяет память для типа number `
  
 ` var str = 'test'; // выделяет память для типа string`
 
"Использование значений", как правило, означает -  чтение и запись значений из/в выделенной для них области памяти. Это происходит при чтении или записи значения какой-либо переменной, или свойства объекта или даже при передаче аргумента функции.
 
### Алгоритм сборки мусора ("Mark-and-sweep")
Данный алгоритм сужает понятие "объект более не нужен" до "объект недоступен".

Основывается на понятии о наборе объектов, называемых roots (в JavaScript root'ом является глобальный объект). Сборщик мусора периодически запускается из этих roots, сначала находя все объекты, на которые есть ссылки из roots, затем все объекты, на которые есть ссылки из найденных и так далее. Стартуя из roots, сборщик мусора, таким образом, находит все доступные объекты и уничтожает недоступные.
 
#### Замыкания

Объекты переменных, о которых шла речь ранее, в главе про замыкания, также подвержены сборке мусора. Они следуют тем же правилам, что и обычные объекты.

Объект переменных внешней функции существует в памяти до тех пор, пока существует хоть одна внутренняя функция, ссылающаяся на него через свойство [[Scope]].


#### Оптимизация в V8 и её последствия

Современные JS-движки делают оптимизации замыканий по памяти. Они анализируют использование переменных и в случае, когда переменная из замыкания абсолютно точно не используется, удаляют её.

В коде выше переменная value никак не используется. Поэтому она будет удалена из памяти.

**Важный побочный эффект в V8 (Chrome, Opera) состоит в том, что удалённая переменная станет недоступна и при отладке!**