# popup

Блок `popup` используется для создания выпадающих элементов и позволяет изменять их состояние, поведение и внешний вид. Блок отображается поверх остальных элементов страницы.

Попап может быть вызван различными элементами страницы, например, кнопками или псевдоссылками.

В момент первого показа (установка модификатора `_visible`) DOM-элемент блока создается в конце `<body>`.


## Модификаторы блока

### Темы блока `_theme`

Блок представлен в следующих темах:

 * simple
 * normal

Без указания темы к блоку применяются значения по умолчанию (*default*), установленные браузером.

Наглядно видно на примерах ниже:

#### default
```bemjson
{
    block : 'popup',
    content : 'default'
}
```


#### simple

```bemjson
{
    block : 'popup',
    mods : { theme : 'simple' },
    content : 'simple'
}
```


#### normal

```bemjson
{
    block : 'popup',
    mods : { theme : 'normal' },
    content : 'normal'
}
```


### Видимый `_visible`

Модификатор, отвечающий за видимость блока. При значении модификатора `false` блок не отображается.

```bemjson
{
    block : 'popup',
    mods : { theme : 'normal', visible : true },
    content : 'normal'
}
```


### Расположение относительно родителя `_direction`

Модификатор управляет положением попапа на странице относительно вызвавшего его блока. Расположение блока определяется автоматически, исходя из массива допустимых расположений и положения родителя на странице.

Выбранное расположение влияет на направление анимации раскрытия.

По умолчанию используется следующий массив допустимых расположений:

<table>
    <tr>
        <th> Направление раскрытия </td>
        <th> Индекс в массиве </td>
    </tr>
        <td> bottom-left </td>
        <td> 0 </td>
    </tr>
    <tr>
        <td> bottom-center </td>
       <td> 1 </td>
   </tr>
    <tr>
        <td> bottom-right</td>
        <td> 2 </td>
    </tr>
    <tr>
        <td> top-left </td>
        <td> 3 </td>
    </tr>
    <tr>
        <td> top-center </td>
        <td> 4 </td>
    </tr>
    <tr>
        <td> top-right </td>
        <td> 5 </td>
    </tr>
    <tr>
        <td> right-top </td>
        <td> 6 </td>
    </tr>
    <tr>
        <td> right-center </td>
        <td> 7 </td>
    </tr>
    <tr>
        <td> right-bottom </td>
        <td> 8 </td>
    </tr>
    <tr>
        <td> left-top </td>
        <td> 9 </td>
    </tr>
    <tr>
        <td> left-center </td>
        <td> 10 </td>
    </tr>
    <tr>
        <td> left-bottom </td>
        <td> 11 </td>
    </tr>
</table>

Чтобы управлять расположением попапа, можно ограничить массив расположений, оставив только требуемые. Пользовательский массив нужно передать в качестве JS-параметра с ключом `directions`. При этом из значений массива будет выбрано наиболее подходящее, исходя из положения родителя на странице.

Например, если требуется, чтобы попап раскрывался над родителем:

```bemjson
{
    block : 'popup',
    mods : { autoclosable : true, theme: 'simple' },
    js : { directions : ['top-left', 'top-center', 'top-right'] },
    content : 'Hello, world!'
}
```


Или разместить попап справа по центру:

```bemjson
{
    block : 'popup',
    mods : { autoclosable : true, theme: 'simple' },
    js : { directions : ['right-center'] },
    content : 'Hello, world!'
}
```


### Автоматическое скрытие `_autoclosable`

Модификатор отвечает за автоматическое скрытие попапа при клике вне блока. При установке модификатора `_autoclosable` в значении `_true` блок будет скрываться при щелчке за его пределами (будет снят модификатор `_visible`).

```bemjson
{
    block : 'popup',
    mods : { theme : 'normal', autoclosable : true },
    content : 'normal'
}
```


##Взаимосвязи между popup'ами

Так как попап выносит себя в конец `<body>`, чтобы перекрывать другие элементы страницы, возникает необходимость в построении взаимосвязей. Блок умеет выстраивать связи Parent → [ Child, Child, ... ]. Для этого он выполняет проверку, вложен ли блок, вызывающий попап в другой `popup`. Потомки знают о наличии родителя.
Если бы этих взаимосвязей не было, то родительский попап (при наличии модификатора `autoclosable` со значением `true`) закрылся бы при клике внутри дочернего.

Другими словами модификатор `autoclosable` со значением `true` означает, что попап закроется сам и закроет свои дочерние блоки при клике за пределами его самого и дочерних попапов.

Дочерние блоки popup можно рассматривать в виде цепочки 1 → 2 → 3 → 4. При клике на втором попапе – закрываются третий и четвертый. При клике в первом – закрываются второй, третий, четвертый. При клике за пределами любого попапа из цепочки – закроются все.

### Зависимости блока

Блок `popup` зависит от следующего набора блоков и элементов:

* `i-bem__dom `
* `jquery`
* `dom`
* `objects`
* `functions__throttle`
* `keyboard`
* `ua`
* `jquery__event_pointer`