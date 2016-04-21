# Корутины

Корутины похожи на генераторы за исключением нескольких отличий, основные из
которых:

- генераторы возвращают данные
- корутины потребляют данные

Для начала рассмотрим процесс создания генератора. Мы можем создать один
таким образом:

```python
def fib():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```

Такой генератор зачастую используется в цикле `for`:

```python
for i in fib():
    print(i)
```

Такой подход отличается скоростью и отсутствием повышенной нагрузки на память,
поскольку значения **генерируются** на лету и не хранятся в списке. Теперь,
если мы используем `yield` в примере выше, то, условно говоря, получим
корутину. Корутины потребляют данные, которые им передаются. Вот простой пример
реализации `grep` на Python:

```python
def grep(pattern):
    print("Searching for", pattern)
    while True:
        line = (yield)
        if pattern in line:
            print(line)
```

Подождите! Что возвращает `yield`? Ну, мы преобразовали её в корутину.
Сначала она не содержит значения, вместо этого мы передаем значение из внешнего
источника. Мы передаем значения используя метод `.send()`. Вот простой
пример:

```python
search = grep('coroutine')
next(search)
# Output: Searching for coroutine
search.send("I love you")
search.send("Don't you love me?")
search.send("I love coroutines instead!")
# Output: I love coroutines instead!
```

Передаваемые значения используюся в `yield`. Почему мы вызвали `next()`?
Это требуется для запуска корутины. Так же как и в случае с генераторами,
корутины не запускают функцию сразу же. Вместо этого они запускают её в ответ
на вызов методов `__next__()` и `.send()`. По этой причине вам требуется
вызвать `next()`, чтобы исполнение дошло до `yield`.

Мы можем закрыть корутину при помощи метода `.close()`:

```python
search = grep('coroutine')
# ...
search.close()
```

Это лишь основы работы с корутинами. Рекомендую дополнительно просмотреть
эту шикарную [презентацию](http://www.dabeaz.com/coroutines/Coroutines.pdf)
от David Beazley.