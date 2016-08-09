---
layout: page
title :
header : Документация
group: navigation
cssClass: docs
header-text: >
  <h4>Плохой софт <span class="bold">не имеет</span> документации.
  Отличный софт <span class="bold">не нуждается</span> в документации.</h4>

  Мы с гордостью заявляем, что Selenide настолько прост, что не нужно читать тонны документации, чтобы начать с ним работать.<br/>
  Вся работа с Selenide состоит всего из трёх простых шагов:
---
{% include JB/setup %}

{% include documentation-menu.md %}

## Три простых шага:
<strong>1.</strong>  Открыть страницу   <br>
<strong>2.</strong>  $(элемент).совершитьДействие()<br>
<strong>3.</strong>  $(элемент).проверитьУсловие()<br>

```java
  open("/login");
  $("#submit").click();
  $(".message").shouldHave(text("Привет"));
```

## Используй мощь IDE!

Весь Selenide API состоит из небольшого числа классов. Мы предлагаем прекратить читать, открыть вашу любимую IDE и просто начать печатать.

Просто набери: `$(selector).` - и IDE предложит все возможные опции.

<img src="{{ BASE_PATH }}/images/ide-just-start-typing.png" alt="Selenide API: Просто начни писать"/>

Используй всю мощь современных средств разработки вместо того, чтобы забивать себе голову документацией!

<br/>

## Selenide API

Здесь можно найти <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}" target="_blank">Selenide javadoc</a>.

Для справки, ниже приведены некоторые классы Selenide, которые тебе, возможно, придётся использовать:

<h3>com.codeborne.selenide.Selenide
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Selenide.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html">[javadoc]</a>
</h3>

Ядро библиотеки Selenide. Основные методы - это `open`, `$` и `$$`:
<ul>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#open(java.lang.String)">open(URL)</a></li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$(java.lang.String)">$(String cssSelector)</a>   - возвращает первый SelenideElement</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$(org.openqa.selenium.By)">$(By)</a>   - возвращает первый SelenideElement</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$$(java.lang.String)">$$(String cssSelector)</a>   - возвращает список элементов</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$$(org.openqa.selenium.By)">$$(By)</a>   - возвращает список элементов</li>
</ul>

Обычно, когда ты получаешь с помощью доллара объект SelenideElement, ты можешь либо
совершить с ним какое-то действие (click, setValue), либо проверить какое-то условие: `shouldHave(text("abc"))`.

И есть ещё несколько методов, которые тоже иногда нужны:
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#sleep(long)">sleep()</a>,
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#refresh()">refresh()</a>,
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#title()">title()</a>.

Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.SelenideElement
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/SelenideElement.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/SelenideElement.html">[javadoc]</a>
</h3>

Класс SelenideElement - обёртка вокруг Selenium WebElement, которая добавляет несколько весьма полезных методов.

Cам объект SelenideElement, который возвращается методом `$` - не является найденным элементом на странице. Метод `$` не ищет реальный элемент, а по сути только создает объект-искатель-элемента. Этот объект-искатель-элемента позволит тебе работать с актуальным элементом - у этого объекта есть все методы самого элемента - методы действий над элементом, проверок и т.д.
 
Объект SelenideElement ищет элемент "лениво" - т.е. только тогда, когда это нужно - поиск элемента в DOM страницы будет выполнен только в момент, когда над обьектом SelenideElement совершится какое-то действие (либо проверка какого-то условия, либо попытка получить какую-то информацию о нем  - text(), val()).
 
Таким образом, один раз получив SelenideElement c помощью метода `$`, и далее работая с этим  объектом-искателем-элемента, ты будешь использовать элемент, найденный на момент действия.

Среди методов SelenideElement есть такие, которые не вызывают поиск по DOM - это методы создания "искателей других элементов внутри данного":

* $(String cssSelector) | find(String cssSelector)
* $(By) | find(By)
* $$(String cssSelector) | findAll(String cssSelector)
* $$(By) | findAll(By)

Таким образом, можно пошагово уточнять - какой внутренний элемент необходимо получить внутри внешнего элемента, строя цепочку вызовов метода `$`, e.g. `$("#page").$("#table").$("#header")`, и это не вызовет поиск по DOM, а опишет способ поиска элемента. Соответственно, можно сохранить "цепочку" любой длины в переменную, независимо от того, загружена ли страница на момент инициализации переменной, или даже открыт ли браузер, e.g.:

```java
//до открытия браузера и загрузки страницы
SelenideElement menu = $("#menu");
SelenideElement streamMenu = menu.$(By.linkText("Stream"));
SelenideElement profileMenu = menu.$(By.linkText("Profile"));
//...
open("/main");
profileMenu.click();
//...
streamMenu.click();
//...
```

Методы-проверки - assertions - вызывают поиск по DOM и возвращают объекты SelenideElement, позволяя использовать цепочки вызовов.

*  should(Condition) | shouldBe(Condition) | shouldHave(Condition)
*  shouldNot(Condition) | shouldNotBe(Condition) | shouldNotHave(Condition)

Рекомендуем выбирать такой метод, чтобы строка кода легко воспринималась, как обычная фраза, e.g. :
```java
$("input").should(exist);  
$("input").shouldBe(visible);
$("input").shouldHave(exactText("Some text"));
```

Проверки играют роль явных ожиданий (explicit waits) в Selenide. Они ждут до удовлетворения условия (visible, enabled, text("some text")) пока не истечет таймаут (значение `Configuration.timeout`, которое установлено по умолчанию в 4000 миллисекунд).

Можно использовать проверки явно - с целью ожиданий нужного состояния у элементов перед действием, e.g. `$("#submit").shouldBe(enabled).click();`

Есть версии явных ожиданий с указанием таймаута:

*  waitUntil(Condition, milliseconds)
*  waitWhile(Condition, milliseconds)

Действия над элементами 

*  click()
*  doubleClick()
*  contextClick()
*  hover()
*  setValue(String) | val(String)
*  pressEnter()
*  pressEscape()
*  pressTab()
*  selectRadio(String value)
*  selectOption(String)
*  append(String)
*  dragAndDropTo(String)
*  ...

имеют встроенные неявные ожидания (implicit waits) до видимости элемента, поэтому код `$("#submit").shouldBe(visible).click()` избыточен, будет достаточно `$("#submit").click()`.

Большинство действий также возвращают обьект SelenideElement, позволяя использовать цепочки вызовов, e.g. `$("#edit").setValue("text").pressEnter();`.

Методы для получения статусов и значений элементов имеют разную логику неявных ожиданий в зависимости от контекста:

Выполняется ожидание до появления элемента в DOM:

*  getValue() | val()
*  data()
*  attr(String)
*  text()        // возвращает "видимый текст на странице"
*  innerText()   // возвращает "текст элемента в DOM"
*  getSelectedOption()
*  getSelectedText()
*  getSelectedValue()

Нет ожидания :

*  isDisplayed() //возвращает false, если элемент либо невидимый, либо его нет в DOM
*  exists() //возвращает true, если элемент есть в DOM, иначе - false 


Другие полезные команды:

*  uploadFromClasspath(String fileName)
*  download()
*  toWebElement()
*  uploadFile(File...)

Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.Condition
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Condition.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Condition.html">[javadoc]</a>
</h3>

Условия используются в конструкциях should / waitUntil / waitWhile. Мы рекомендуем статически импортировать используемые условия, чтобы получить все преимущества читаемого кода.

*   visible | appear   // e.g. $("input").shouldBe(visible)
*   present | exist    // условия присутствия элемента в DOM 
*   hidden | disappear | not(visible)
*   readonly           // e.g. $("input").shouldBe(readonly)
*   name               // e.g. $("input").shouldHave(name("fname"))
*   value              // e.g. $("input").shouldHave(value("John"))
*   type               // e.g. $("#input").shouldHave(type("checkbox"))
*   id                 // e.g. $("#input").shouldHave(id("myForm"))
*   empty              // e.g. $("h2").shouldBe(empty)
*   attribute(name)    // e.g. $("#input").shouldHave(attribute("required"))
*   attribute(name, value) // e.g. $("#list li").shouldHave(attribute("class", "active checked"))
*   cssClass(String)       // e.g. $("#list li").shouldHave(cssClass("checked"))
*   focused
*   enabled
*   disabled
*   selected
*   matchText(String regex)
*   text(String substring)
*   exactText(String wholeText)
*   textCaseSensitive(String substring)
*   exactTextCaseSensitive(String wholeText)

Более подробная информация доступна в Wiki (скоро).

Можно легко добавлять свои условия, реализовав подкласс `com.codeborne.selenide.Condition`.

e.g.:

```java
    public static Condition css(final String propName, final String propValue) {
        return new Condition("css") {
            @Override
            public boolean apply(WebElement element) {
                return propValue.equalsIgnoreCase(element.getCssValue(propName));
            }

            @Override
            public String actualValue(WebElement element) {
                return element.getCssValue(propName);
            }
        };
    }

// Пример использования:
$("h1").shouldHave(css("font-size", "16px"));
```


<h3>com.codeborne.selenide.Selectors
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Selectors.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html">[javadoc]</a>
</h3>

Класс содержит некоторые `By` селекторы для поиска элементов по тексту или атрибуту (которых не хватает в стандартном Selenium WebDriver API):

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html#byText(java.lang.String)">byText</a>     - поиск элемента по точному тексту
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html#withText(java.lang.String)">withText</a>   - поиск элемента по тексту (подстроке)
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html#by(java.lang.String, java.lang.String)">by</a>    - поиск элемента по атрибуту
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html#byTitle(java.lang.String)">byTitle</a>   - поиск по атрибуту "title"
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selectors.html#byValue(java.lang.String)">byValue</a>   - поиск по атрибуту "value"

```java
// Пример использования:
$(byText("Login")).shouldBe(visible));
$(By.xpath("//div[text()='Login']")).shouldBe(visible); // можно использовать любой org.openqa.selenium.By.* селектор
$(byXpath("//div[text()='Login']")).shouldBe(visible); // или его аналог
```

Более подробная информация доступна в Wiki (скоро).


<h3>com.codeborne.selenide.ElementsCollection
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/ElementsCollection.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html">[javadoc]</a>
</h3>

Объект этого класса можно получить с помощью вызова метода `$$`, при этом - не будет выполняться поиск в DOM. Представляет список веб-элементов, которые будут найдены после "вызова поиска в DOM" соответствующими методами ElementsCollection

Assertions (проверки), которые вызывают поиск по DOM, и играют роль явных ожиданий:

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldBe(com.codeborne.selenide.CollectionCondition)">shouldBe</a>     - e.g. `$$(".errors").shouldBe(empty)`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldHave(com.codeborne.selenide.CollectionCondition)">shouldHave</a>     - e.g. `$$("#mytable tbody tr").shouldHave(size(2))`

Методы для получения статусов и значений, вызывают поиск по DOM (встроенных неявных ожиданий не имеют)

*   size()
*   isEmpty()
*   getTexts()  // возвращает массив видимых текстов, e.g. для элементов `<li>a</li><li hidden>b</li><li>c</li>` вернет массив вида ["a", "", "c"]

Дополнительная фильтрация, *не* вызывает поиск по DOM и может быть безопасно сохранена в переменную, *не* имеет встроенных ожиданий
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#filterBy(com.codeborne.selenide.Condition)">filterBy(Condition)</a>  возвращает коллекцию (как ElementsCollection) из только тех элементов оригинальной коллекции, которые удовлетворяют условию - e.g. `$$("#multirowTable tr").filterBy(text("Norris"))`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#excludeWith(com.codeborne.selenide.Condition)">excludeWith(Condition)</a>     - e.g. `$$("#multirowTable tr").excludeWith(text("Chuck"))`

Дополнительный поиск элементов, *не* вызывает поиск по DOM и может быть безопасно сохранен в переменную, не имеет встроенных ожиданий, (при этом - встроенные неявные ожидания сработают при попытке действий над элементом)

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#get(int)">get(int)</a> - возвращает n-ый элемент как `SelenideElement`; при попытке действий над элементом сработает неявное ожидание до существования и видимости n-го элемента в оригинальной коллекции
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#findBy(com.codeborne.selenide.Condition)">findBy(Condition)</a> - возвращает элемент коллекции удовлетворяющий условию как `SelenideElement`, при попытке действий над элементом сработает неявное ожидание до появления в оригинальной коллекции соответствующего элемента - e.g. `$$("#multirowTable tr").findBy(text("Norris"))`

Дополнительная фильтрация и поиск позволяют почти полностью отказаться от менее читабельных xpath локаторов:
```java
$$("#list li").filterBy(cssClass("enabled")).findBy(exactText("foo")).find(".remove").click();
// вместо
$(By.xpath("//*[@id='list']//li[@class='enabled' and .//text()='foo']//*[@class='remove']")).click();
```
В особых случаях при большом количестве операций с большими списками элементов, xpath может быть предпочтительней из-за скорости работы.

Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.CollectionCondition
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/CollectionCondition.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/CollectionCondition.html">[javadoc]</a>
</h3>

Условия используются в конструкциях shouldBe / shouldHave для коллекции - объекта ElementsCollection. Мы рекомендуем статически импортировать используемые условия, чтобы получить все преимущества читаемого кода.

*   empty   // e.g. $$("#list li").shouldBe(empty)
*   size(int)    // e.g. $$("#list li").shouldHave(size(10))
*   sizeGreaterThan(int)
*   sizeGreaterThanOrEqual(int)
*   sizeLessThan(int)
*   sizeLessThanOrEqual(int)
*   sizeNotEqual(int)
*   texts(String... substrings)
*   exactTexts(String... wholeTexts)

Более подробная информация доступна в Wiki (скоро).


<h3>com.codeborne.selenide.WebDriverRunner
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/WebDriverRunner.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/WebDriverRunner.html">[javadoc]</a>
</h3>

Этот класс содержит некоторые функции, относящиеся к браузеру :

*  isChrome()
*  isFirefox()
*  isHeadless()
*  url() - возвращает текущий url
*  source() - возвращает исходный HTML текущего окна
*  getWebDriver() - возвращает обьект WebDriver (созданный Selenide автоматически или установленный пользователем), таким образом отдавая "чистый" интерфейс к Selenium, если нужно
*  setWebDriver(WebDriver) - указывает Selenide использовать драйвер, созданный пользователем. С этого момента пользователь сам ответственен за закрытие драйвера.


Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.Configuration
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Configuration.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Configuration.html">[javadoc]</a>
</h3>

Этот класс содержит конфигурации для запуска тестов e.g.:

*  timeout - время ожидания в миллисекундах, которое используется в явных (should/waitUntil/waitWhile) и неявных ожиданиях, может быть изменено во время исполнения, e.g. `Configuration.timeout = 6000;`
*  browser (e.g. `"chrome"`, `"ie"`, `"firefox"`)
*  baseUrl
*  reportsFolder

Также можно передать конфигурационные параметры как system properties для использования в CI (continuous integration) задачах (e.g. -Dselenide.baseUrl=http://staging-server.com/start)

Более подробная информация доступна в Wiki (скоро).

Более подробно эти и другие классы описаны в [javadoc](http://selenide.org/javadoc.html)

Оставайся на связи!
