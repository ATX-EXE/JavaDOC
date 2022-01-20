# Документирование JavaDOC

<b>JavaDOC</b> — это генератор документации в HTML-формате из комментариев исходного кода Java и определяет стандарт для
документирования классов Java. Для создания доклетов и тэглетов, которые позволяют программисту анализировать структуру
Java-приложения, javadoc также предоставляет API. В каждом случае комментарий должен находиться перед документируемым
элементом.

При написании комментариев к кодам Java используют три типа комментариев:

```java
// однострочный комментарий;
/* многострочный 
комментарий */
/** комментирование документации */
```

### Дескрипторы JavaDOC

| Дескриптор                                | Применение                    | Описание                                                  |
|:------------------------------------------|:------------------------------|:----------------------------------------------------------|
| `@author`                                 | Класс, интерфейс              | Автор                                                     |
| `@version`                                | Класс, интерфейс              | Версия. Не более одного дескриптора на класс              |
| `@since`                                  | Класс, интерфейс, поле, метод | Указывает, с какой версии доступно                        |
| `@see`                                    | Класс, интерфейс, поле, метод | Ссылка на другое место в документации                     |
| `@param`                                  | Метод                         | Входной параметр метода                                   |
| `@return`                                 | Метод                         | Описание возвращаемого значения                           |
| `@exception` имя_класса описание          | Метод                         | Описание исключения, которое может быть послано из метода |
| `@throws` имя_класса описание             | Метод                         | Описание исключения, которое может быть послано из метода |
| `@deprecated`                             | Класс, интерфейс, поле, метод | Описание устаревших блоков кода                           |
| `{@link reference}`                       | Класс, интерфейс, поле, метод | Ссылка                                                    |
| `{@value}`                                | Статичное поле                | Описание значения переменной                              |

Для документирования кода можно использовать HTML теги. При использовании ссылочных дескрипторов `@see` и `@link` нужно
сначала указать имя класса и после символа `#` его метод или поле.

### Форма документирования кода

Документирование класса, метода или переменной начинается с комбинации символов `/**` , после которого следует тело
комментариев; заканчивается комбинацией символов `*/`.

В тело комментариев можно вставлять различные дескрипторы. Каждый дескриптор, начинающийся с символа `@` должен стоять
первым в строке. Несколько дескрипторов одного и того же типа необходимо группировать вместе. Встроенные дескрипторы (
начинаются с фигурной скобки) можно помещать внутри любого описания.

### Пример

```java
/**
 * Класс продукции со свойствами <b>maker</b> и <b>price</b>.
 * @autor Киса Воробьянинов
 * @version 2.1
 */
class Product {
    /** Поле производитель */
    private String maker;

    /** Поле цена */
    public double price;

    /**
     * Конструктор - создание нового объекта
     * @see Product#Product(String, double)
     */
    Product() {
        setMaker("");
        price = 0;
    }

    /**
     * Конструктор - создание нового объекта с определенными значениями
     * @param maker - производитель
     * @param price - цена
     * @see Product#Product()
     */
    Product(String maker, double price) {
        this.setMaker(maker);
        this.price = price;
    }

    /**
     * Функция получения значения поля {@link Product#maker}
     * @return возвращает название производителя
     */
    public String getMaker() {
        return maker;
    }

    /**
     * Процедура определения производителя {@link Product#maker}
     * @param maker - производитель
     */
    public void setMaker(String maker) {
        this.maker = maker;
    }
}
```
### Сценарии ant и javadoc

Для создания документации можно использовать ant.

Пример сценария (задания) ant для формирования документации к приложению MyProject:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project name="MyProject.doc" default="create-javadoc" basedir="../../../../../..">

    <target name="create-javadoc" description="documentation" depends="">
        <property name="app.name" value="MyProject"/>
        <property name="app.version" value="1.21"/>
        <property name="app.author" value="AuthorName"/>
        <property name="app.year" value="2015"/>
        <property name="dir.package" value="examples.projects.*"/>
        <property name="dir.src" value="./src"/>
        <property name="dir.doc" value="./doc"/>

        <echo message="Create MyProject.doc."/>
        <mkdir dir="${dir.doc}"/>

        <javadoc destdir="${dir.doc}"
                 use="true"
                 private="true"
                 author="${app.author}"
                 version="${app.version}"
                 windowtitle="${app.name} API"
                 doctitle="${app.name}">
            <fileset dir="${dir.src}" defaultexcludes="yes">
                <include name="examples/projects/**"/>
            </fileset>
        </javadoc>
    </target>
</project>
```

Взято с [java-online.ru](https://java-online.ru/java-javadoc.xhtml)