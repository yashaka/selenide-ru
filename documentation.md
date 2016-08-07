---
layout: page
title :
header : Документация
group: navigation
cssClass: docs
header-text: >
  <h4>Плохой софт <span class="bold">не имеет</span> документации.
  Отличный софт <span class="bold">не нуждается</span> в документации.</h4>

  Мы с гордостью заявляем, что Selenide настолько прост, что вам не нужно читать тонны документации, чтобы начать с ним работать.<br/>
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

Для справки, ниже приведены некоторые классы Selenide, которые вам, возможно, придётся использовать:

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

Обычно, когда вы получаете с помощью доллара объект SelenideElement, вы можете либо
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

Cам объект SelenideElement, который возвращает `$` - не есть реально найденным элементом на странице. Доллар не ищет реальный элемент, а создает объект "искателя элемента". Реальный же поиск в DOM страницы будет выполнен в момент, когда вы совершите с обьектом SelenideElement какое-то действие, либо проверите какое-то условие, либо попробуете получить какую то информацию о нем (text(), val()). Таким образом, один раз получив SelenideElement c помощью доллара вы можете быть уверены, что при любом действии вы будете работать с его последней актуальной версией.

Среди методов SelenideElement есть такие, которые не вызывают поиск по DOM - это методы создания "искателей других элементов внутри даного":

* $(String cssSelector) | find(String cssSelector)
* $(By) | find(By)
* $$(String cssSelector) | findAll(String cssSelector)
* $$(By) | findAll(By)

Таким образом вы можете "связать" объекты SelenideElements в цепочку с помощью `$`, например `$("#page").$("#table").$("#header")`, и это не вызовет поиск по DOM. Соответственно вы можете сохранить "цепочку" любой длины в переменную, в независимости от того загружена ли страница на текущий момент, или даже открыт ли браузер, например:

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

*  should(Condition)
*  shouldBe(Condition)
*  shouldHave(Condition)
*  shouldNot(Condition)
*  shouldNotBe(Condition)
*  shouldNotHave(Condition)

Проверки играют роль явных ожиданий (explicit waits) в Selenide. Они ждут до удовлетворения условия (visible, enabled, text("some text")) пока не истечет таймаут (установленный по умолчанию в `Configuration.timeout`).

Можно использовать проверки явно с целью ожиданий нужного состояния у элементов перед действием, например `$("#submit").shouldBe(enabled).click();`

Есть версии явных ожиданий с указанием таймаута:

*  waitUntil(Condition, milliseconds)
*  waitWhile(Condition, milliseconds)

Действия над элементами 

*  click()
*  doubleClick()
*  pressEnter(String)
*  selectOption(String text)
*  selectOptionByValue(String value)
*  setValue(String)
*  val(String)
*  append(String)

имеют встроеные неявные ожидания (implicit waits) до видимости элемента, поэтому код `$("#submit").shouldBe(visible).click()` избыточен.

Большинство действий также возвращают обьект SelenideElement, позволяя использовать цепочки вызовов, например `$("#edit").setValue("text").pressEnter();`.

Методы для получения статусов и значений элементов имеют разную логику неявных ожиданий в зависимости от контекста:

*  val()         // ждет до появления в DOM
*  data()        // ждет до появления в DOM
*  text()        // ждет до появления в DOM и возвращает "видимый текст на странице"
*  innerText()   // ждет до появления в DOM и возвращает "текст элемента в DOM"
*  isDisplayed() // не ждет, возвращает false если элемента нет в DOM
*  exists()      // не ждет
*  getSelectedOption()     // ждет до появления в DOM
*  getSelectedText()       // ждет до появления в DOM
*  getSelectedValue()      // ждет до появления в DOM <br/>

Другие полезные команды:

*  uploadFromClasspath(String fileName)
*  download()
*  toWebElement()

Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.Condition
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Condition.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Condition.html">[javadoc]</a>
</h3>

Условия используются в конструкциях should/waitUntil/waitWhile. Мы рекомендуем статически импортировать com.codeborne.selenide.Condition.* чтобы получить все преимущества читаемого кода.

*   visible | appear   // например, $("input").shouldBe(visible)
*   present | exist    // условия присутствия элемента в DOM 
*   hidden | disappear | not(visible)
*   readonly           // например, $("input").shouldBe(readonly)
*   attribute(String)
*   name               // например, $("input").shouldHave(name("fname"))
*   value              // например, $("input").shouldHave(value("John"))
*   type               // $("#input").shouldHave(type("checkbox"))
*   id                 // $("#input").shouldHave(id("myForm"))
*   empty              // $("h2").shouldBe(empty)
*   options
*   attribute(name)    // $("#input").shouldHave(attribute("required"))
*   attribute(name, value) // $("#list li").shouldHave(attribute("class", "active checked"))
*   cssClass(String)       // $("#list li").shouldHave(cssClass("checked"))
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

Вы можете легко добавлять свои условия, реализовав подкласс `com.codeborne.selenide.Condition`.

Например:

```java
public static Condition css(final String propName, final String propValue) {
    @Override
    public boolean apply(WebElement element) {
      return propValue.equalsIgnoreCase(element.getCssValue(propName));
    }

    @Override
    public String actualValue(WebElement element) {
        return element.getCssValue(propName);
    }
};

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

Это класс, объект которого возвращает метод `$$`, не вызывая поиска в DOM. Представляет список веб-элементов которые будут найдены после "вызова поиска в DOM" соответствующими методами ElementsCollection:

Assertions (проверки), которые вызывают поиск по DOM, и играют роль явных ожиданий:

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldBe(com.codeborne.selenide.CollectionCondition)">shouldBe</a>     - e.g. `$$(".errors").shouldBe(empty)`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldHave(com.codeborne.selenide.CollectionCondition)">shouldHave</a>     - e.g. `$$("#mytable tbody tr").shouldHave(size(2))`

Методы для получения статусов и значений, вызывают поиск по DOM (встроенных неявных ожиданий не имеют)

*   size()
*   isEmpty()
*   getTexts()  // возвращает массив видимых текстов, например для элементов `<li>a</li><li hidden>b</li><li>c</li>` вернет массив вида ["a", "", "c"]

Дополнительная фильтрация, *не* вызывает поиск по DOM и может быть безопасно сохранена в переменную, *не* имеет встроенных ожиданий
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#filterBy(com.codeborne.selenide.Condition)">filterBy(Condition)</a>  возвращает коллекцию (как ElementsCollection) из только тех элементов оригинальной коллекции которые удовлетворяют условию - e.g. `$$("#multirowTable tr").filterBy(text("Norris"))`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#excludeWith(com.codeborne.selenide.Condition)">excludeWith(Condition)</a>     - e.g. `$$("#multirowTable tr").excludeWith(text("Chuck"))`

Дополнительный поиск элементов, *не* вызывает поиск по DOM и может быть безопасно сохранен в переменную, имеет встроеные неявные ожидания

*   get(int) - возвращает n-ый элемент как `SelenideElement`; при попытке действий над элементом сработает неявное ожидание до существования n-го элемента в оригинальной коллекции
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#findBy(com.codeborne.selenide.Condition)">findBy(Condition)</a> - возвращает элемент коллекции удовлетворяющий условию как `SelenideElement`, при попытке действий над элементом сработает неявное ожидание до появления в оригинальной коллекции соответствующего элемента - e.g. `$$("#multirowTable tr").findBy(text("Norris"))`

Дополнительная фильтрация и поиск позволяют почти полностью отказаться от менее читабильных xpath локаторов:
```java
$$("#list li").filterBy(cssClass("enabled")).findBy(exactText("foo")).find(".remove").click();
// вместо
$(By.xpath("//*[@id='list']//li[@class='enabled' and .//text()='foo']//*[@class='remove']")).click();
```
В особых случаях при большом количестве операций с большими списками элементов, xpath может быть предпочтительней из-за скорости работы.

Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.WebDriverRunner
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/WebDriverRunner.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/WebDriverRunner.html">[javadoc]</a>
</h3>

Этот класс содержит некоторые функции относящиеся к браузеру :

*  isChrome()
*  isFirefox()
*  isHeadless()
*  url() - возвращает текущий url
*  source() - возвращает исходный HTML текущего окна
*  getWebDriver() - возвращает обьект WebDriver (созданный Selenide автоматически или установленный пользователем), таким образом отдавая "чистый" интерфейс к Selenium если нужно
*  setWebDriver(WebDriver) - указывает Selenide использовать драйвер созданный пользователем. С этого момента пользователь сам ответственен за закрытие драйвера.


Более подробная информация доступна в Wiki (скоро).

<h3>com.codeborne.selenide.Configuration
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Configuration.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Configuration.html">[javadoc]</a>
</h3>

Этот класс содержит конфигурации для запуска тестов например:

*  timeout - время ожидания в милисекундах, которое используется в явных (should/waitUntil/waitWhile) и неявных ожиданиях, может быть изменено во время исполнения, e.g. `Configuration.timeout = 6000;`
*  browser (напр. `"chrome"`, `"ie"`, `"firefox"`)
*  baseUrl
*  reportsFolder

Вы так же можете передать конфигурационные параметры как system properties для использования в CI (continiuous integration) задачах (напр. -Dselenide.baseUrl=http://staging-server.com/start)

Более подробная информация доступна в Wiki (скоро).

Более подробно эти и другие классы описаны в [javadoc](http://selenide.org/javadoc.html)

Оставайтесь на связи!
