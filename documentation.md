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
<strong>1.</strong>  открыть(страницу)   <br/>
<strong>2.</strong>  $(элемент).совершитьДействие()<br/>
<strong>3.</strong>  $(элемент).проверитьУсловие()<br/>

```java
  open("/login");
  $("#submit").click();
  $(".message").shouldHave(text("Привет"));
```

## Используй мощь IDE!

Весь Selenide API состоит из небольшого числа классов. Мы предлагаем прекратить читать, открыть твою любимую IDE и просто начать печатать.

Просто набери: `$(selector).` - и IDE предложит все возможные опции.

<img src="{{ BASE_PATH }}/images/ide-just-start-typing.png" alt="Selenide API: Просто начни писать"/>

Используй всю мощь современных средств разработки вместо того, чтобы забивать себе голову документацией!

<br/>

## Selenide API

Здесь можно найти <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}" target="_blank">Selenide javadoc</a>.

Для справки, ниже приведены некоторые классы Selenide с описанием основных определенных в них методов, которые тебе, возможно, понадобятся:

<h3>com.codeborne.selenide.Selenide
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Selenide.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html">[javadoc]</a>
</h3>

Ядро библиотеки Selenide. Основные методы - это `open`, `$` и `$$`:
<ul>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#open(java.lang.String)">open(URL)</a></li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$(java.lang.String)">$(String cssSelector)</a>   - возвращает объект типа SelenideElement, который представляет первый найденный по CSS селектору элемент на странице</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$(org.openqa.selenium.By)">$(By)</a>   - возвращает "первый SelenideElement" по локатору типа By</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$$(java.lang.String)">$$(String cssSelector)</a>   - возвращает объект типа ElementsCollection, который представляет коллекцию всех элементов найденных по CSS селектру</li>
  <li><a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#$$(org.openqa.selenium.By)">$$(By)</a>   - возвращает "коллекцию элементов" по локатору типа By</li>
</ul>

Обычно, когда ты получаешь с помощью доллара объект SelenideElement, ты можешь либо
совершить с ним какое-то действие:
* `$(byText("Sign in")).click();`

и даже несколько действий сразу:
* `$(byName("password")).setValue("qwerty").pressEnter();`

либо проверить какое-то условие: 
* `$(".wellcome-message").shouldHave(text("Welcome, user!"))`.

"Два доллара" же может быть удобно использовать когда нужный элемент является одним из группы однотипных элементов. Например вместо:
```java
$(byXpath("//*[@id='search-results']//a[contains(text(),'selenide.org')]")).click();
```
можно использовать более читабельный и лаконичный вариант:
```java
$$("#search-results a").findBy(text("selenide.org")).click();
```

Такой "составной локатор" удобный еще и тем, что в случае ошибки нахождения элемента позволяет по сообщению об ошибке - сразу определить какая из "частей" не сработала. В первом же случае мы сможем лишь получить информацию о том, что "целый локатор" не сработал, и потратим больше времени на поиск той "части" которая привела к ошибке.

Также, если нужно работать с разными элементами одной и той же коллекции, у нас теперь есть возможность вынести коллекцию элементов в переменную:
```java
ElementsCollection resultLinks = $$("#search-results a");
//...
resultLinks.first().shouldHave(text("selenide.org"));
resultLinks.get(1).shouldHave(text("ru.selenide.org"));
resultLinks.shouldHave(size(10));
//...
resultLinks.findBy(text("github.com/codeborne/selenide")).click()
```
Таким образом мы не повторяем локатор `"#search-results a"` в разных местах, и следовательно, если он поменяется - нам нужно будет внести изменения только в одном месте. 

Также теперь удобно в IDE использовать autocomplete при наборе имени уже созданной переменной `resultLinks`.

Не стесняйся поискать и найти намного больше методов внутри класса Selenide, которые могут тебе понадобиться;), просто набрав в любимом IDE `Selenide.`,

Вот лишь несколько примеров:
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#sleep(long)">sleep()</a>,
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#refresh()">refresh()</a>,
<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#title()">title()</a>.<a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Selenide.html#executeJavaScript()">executeJavaScript(String jsCode, Object... arguments)</a>.


<h3>com.codeborne.selenide.SelenideElement
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/SelenideElement.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/SelenideElement.html">[javadoc]</a>
</h3>

Класс SelenideElement - обёртка вокруг Selenium WebElement, которая добавляет несколько весьма полезных методов.

Cам объект SelenideElement, который возвращается методом `$` - является прокси-элементом. В момент создания с помощью `$` реальный элемент на странице не ищется. Зато потом при любой попытке совершить с прокси-элементом какое-то действие или проверку - прокси-элемент получает последнюю актуальную "версию" реального элемента со страницы (типа WebElement) и "проксирует" ей указанное действие или проверку. 

Во-первых, это позволяет с помощью Selenide эффективно автоматизировать тестирование современных динамических веб приложений. 

Во-вторых, с помощью таких прокси-элементов, можно конструировать объекты страниц в соответствии с шаблоном PageObject. За счет того, что элемент не ищется сразу в момент создания обьекта SelenideElement, мы можем сохранить его в поле обьекта PageObject еще до открытия страници:

```java
//до открытия браузера и загрузки страницы
class HomePage {
  private SelenideElement menu = $("#menu");
  
  public void openStream() {
      this.menu.find(byLinkText("Stream")).click();
  }
  
  public void openProfile() {
      this.menu.find(byLinkText("Profile")).click();
  }
}
HomePage home = new HomePage();
//теперь команда $ уже была вызвана и был создан SelenideElement, а даже браузер еще не октрыт
//...
open("/home"); //открылся браузер и была загружена страница
home.openProfile(); // только теперь была получена актуальная версия элемента-меню со страницы, в ней внутри был найден элемент-ссылка с текстом Stream, и ему была "проксирована" команда click()
//...много чего могло произойти
home.openStream(); //и тепеь перед нахождением элемента-ссылки с текстом Profile для того, что-бы совершить на нем click() - снова была получена актуальная версия элемента-меню на странице
//...
```

Для SelenideElement определенны методы следующего типа:

* методы поиска внутренних элементов
  * а точнее методы создания прокси-элементов представляющих элементы внутри данного
* методы-проверки состояния элемента - assertions
* методы-действия над элементом
* методы получения статусов элементов и значений их атрибутов
* другие

Все они имеют дополнительные встроенные особенности для их более удобного использования. Большинство из них имеют встроенные неявные ожидания. Это позволяет просто работать с динамически загружаемыми элементами не опускаясь до уровня техничских деталей настройки дополнительных явных ожиданий. 

<h4>
Методы поиска внутренних элементов
</h4>


* find(String cssSelector) | $(String cssSelector)
* find(By) | $(By)
* findAll(String cssSelector) | $$(String cssSelector)
* findAll(By) | $$(By)

Здесь $ и $$ просто лаконичные "алиасы" (синонимы) для соответствующих команд.

Все эти методы возвращают "прокси-элементы", то есть не ищут реальные элементы до тех пор пока не будет вызвано какое то действие над элементом (например `.click()`)

Таким образом, можно пошагово уточнять - какой внутренний элемент необходимо получить внутри внешнего элемента, строя цепочку последовательних вызовов, например:
```java
$("#header").find("#menu").findAll(".item")
```

И при этом можно сохранить "цепочку поиска" любой длины в переменную, независимо от того, загружена ли страница на момент инициализации переменной, или даже открыт ли браузер, например:

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

<h4>
Методы-проверки состояния элемента - assertions
</h4>

Методы-проверки - assertions - инициируют для прокси-элемента поиск актуальной версии элемента на странице, совершают проверку по предикату ("условию" - объекту-кондишену класса Condition), и возвращают объекты SelenideElement, позволяя использовать цепочки вызовов.

*  should(Condition) | shouldBe(Condition) | shouldHave(Condition)
*  shouldNot(Condition) | shouldNotBe(Condition) | shouldNotHave(Condition)

Рекомендуется выбирать такой метод, чтобы строка кода легко воспринималась, как обычная фраза, e.g. :
```java
$("input").should(exist);  
$("input").shouldBe(visible);
$("input").shouldHave(exactText("Some text"));
```

Проверки играют роль явных ожиданий (explicit waits) в Selenide. Они **ждут** до удовлетворения условия (visible, enabled, text("some text")) пока не истечет таймаут (значение `Configuration.timeout`, которое установлено по умолчанию в 4000 миллисекунд).

Можно использовать проверки явно - с целью ожиданий нужного состояния у элементов перед действием, e.g. `$("#submit").shouldBe(enabled).click();`

Есть версии явных ожиданий с указанием таймаута:

*  waitUntil(Condition, milliseconds)
*  waitWhile(Condition, milliseconds)



<h4>
Методы-действия над элементом
</h4>

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

инициируют для прокси-элемента поиск актуальной версии элемента на странице, и совершают соответствующее действие.

Действия имеют встроенные неявные ожидания (implicit waits) до видимости элемента пока не истечет таймаут (значение `Configuration.timeout`, которое установлено по умолчанию в 4000 миллисекунд). 

Следовательно код `$("#submit").shouldBe(visible).click()` избыточен и будет достаточно `$("#submit").click()`.

Большинство действий также возвращают обьект SelenideElement, позволяя использовать цепочки вызовов, e.g. `$("#edit").setValue("text").pressEnter();`.

<h4>
Методы получения статусов элементов и значений их атрибутов
</h4>

Инициируют для прокси-элемента поиск актуальной версии элемента на странице и имеют разную логику неявных ожиданий в зависимости от контекста:

* Выполняется ожидание до появления элемента в DOM:

  *  getValue() | val()
  *  data()
  *  attr(String)
  *  text()        // возвращает "видимый текст на странице"
  *  innerText()   // возвращает "текст элемента в DOM"
  *  getSelectedOption()
  *  getSelectedText()
  *  getSelectedValue()

* Нет ожидания :

  *  isDisplayed() //возвращает false, если элемент либо невидимый, либо его нет в DOM
  *  exists() //возвращает true, если элемент есть в DOM, иначе - false 

<h4>
Другие полезные команды
</h4>

*  uploadFromClasspath(String fileName)
*  download()
*  toWebElement()
*  uploadFile(File...)

Более подробная информация доступна в Wiki (скоро). // TODO

<h3>com.codeborne.selenide.Condition
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Condition.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Condition.html">[javadoc]</a>
</h3>

Условия используются в конструкциях should / shouldNot / waitUntil / waitWhile. Мы рекомендуем статически импортировать используемые условия, чтобы получить все преимущества читаемого кода.

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

Более подробная информация доступна в Wiki (скоро). // TODO

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
$(byXpath("//div[text()='Login']")).shouldBe(visible); // или его аналог из Selectors
```

Более подробная информация доступна в Wiki (скоро).


<h3>com.codeborne.selenide.ElementsCollection
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/ElementsCollection.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html">[javadoc]</a>
</h3>

Объект этого класса также является прокси точно также как SelenideElement. Его можно получить с помощью вызова метода `$$`, и он представляет список веб-элементов на странице.

Для ElementsCollection определенны методы следующего типа:

* методы выборки внутренних элементов
  * а точнее методы создания прокси-элементов представляющих выбранные элементы из данной коллекции
* методы-проверки состояния коллекции элементов - assertions
* методы получения статусов и значений коллекции элементов

<h4>
Assertions
</h4>

инициируют для прокси-коллекции поиск актуальных версий элементов коллекции на странице, совершают проверку по предикату ("условию" - объекту-кондишену класса CollectionCondition), и возвращают объекты ElementsCollection, позволяя использовать цепочки вызовов.

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldBe(com.codeborne.selenide.CollectionCondition)">shouldBe</a>     - e.g. `$$(".errors").shouldBe(empty)`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#shouldHave(com.codeborne.selenide.CollectionCondition)">shouldHave</a>     - e.g. `$$("#mytable tbody tr").shouldHave(size(2))`

Также играют роль явных ожиданий (explicit waits). Они **ждут** до удовлетворения условия (size, empty) пока не истечет таймаут (значение `Configuration.collectionsTimeout`, которое установлено по умолчанию в 6000 миллисекунд).

<h4>
Методы получения статусов и значений коллекции элементов
</h4>

Инициируют для прокси-коллекции поиск актуальных версий элементов коллекции на странице, и возращают соответствуюе значение:

*   size()
*   isEmpty()
*   getTexts()  // возвращает массив видимых текстов, e.g. для элементов `<li>a</li><li hidden>b</li><li>c</li>` вернет массив вида ["a", "", "c"]

Эти методы - **не** не имеют встроенных неявных ожиданий.

<h4>
Методы выборки внутренних элементов
</h4>

Дополнительная фильтрация - возвращает обьект прокси-коллекции представляющий отфильтрованные элементы оригинальной коллекции на странице, **не** имеет встроенных ожиданий.

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#filterBy(com.codeborne.selenide.Condition)">filterBy(Condition)</a>  возвращает коллекцию (как ElementsCollection) из только тех элементов оригинальной коллекции, которые удовлетворяют условию - e.g. `$$("#multirowTable tr").filterBy(text("Norris"))`
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#excludeWith(com.codeborne.selenide.Condition)">excludeWith(Condition)</a>     - e.g. `$$("#multirowTable tr").excludeWith(text("Chuck"))`

Дополнительный поиск элементов - возвращает обьект прокси-элемента представляющий найденный элемент оригинальной коллекции на странице, **не** имеет встроенных ожиданий. 

*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#get(int)">get(int)</a> - возвращает n-ый элемент как `SelenideElement`; при попытке действий над элементом сработает неявное ожидание до существования и видимости n-го элемента в оригинальной коллекции
*   <a href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/ElementsCollection.html#findBy(com.codeborne.selenide.Condition)">findBy(Condition)</a> - возвращает элемент коллекции как `SelenideElement` который удовлетворяет условию , при попытке действий над элементом сработает неявное ожидание до появления в оригинальной коллекции соответствующего элемента 

Дополнительная фильтрация и поиск позволяют почти полностью отказаться от менее читабельных xpath локаторов:
```java
$$("#list li").filterBy(cssClass("enabled")).findBy(exactText("foo")).find(".remove").click();
// вместо
$(By.xpath("//*[@id='list']//li[@class='enabled' and .//text()='foo']//*[@class='remove']")).click();
```
В особых случаях при большом количестве операций с большими списками элементов, xpath может быть предпочтительней из-за скорости работы.

Более подробная информация доступна в Wiki (скоро). // TODO: 

<h3>com.codeborne.selenide.CollectionCondition
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/CollectionCondition.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/CollectionCondition.html">[javadoc]</a>
</h3>

Условия используются в конструкциях shouldBe / shouldHave для коллекции - объекта ElementsCollection. Рекомендуется статически импортировать используемые условия, чтобы получить все преимущества читаемого кода.

*   empty   // e.g. $$("#list li").shouldBe(empty)
*   size(int)    // e.g. $$("#list li").shouldHave(size(10))
*   sizeGreaterThan(int)
*   sizeGreaterThanOrEqual(int)
*   sizeLessThan(int)
*   sizeLessThanOrEqual(int)
*   sizeNotEqual(int)
*   texts(String... substrings)
*   exactTexts(String... wholeTexts)

Более подробная информация доступна в Wiki (скоро). // TODO


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


Более подробная информация доступна в Wiki (скоро). // TODO

<h3>com.codeborne.selenide.Configuration
  <a target="_blank" href="https://github.com/codeborne/selenide/blob/master/src/main/java/com/codeborne/selenide/Configuration.java">[src]</a>
  <a target="_blank" href="http://selenide.org/javadoc/{{site.SELENIDE_VERSION}}/com/codeborne/selenide/Configuration.html">[javadoc]</a>
</h3>

Этот класс содержит конфигурации для запуска тестов e.g.:

*  timeout - время ожидания в миллисекундах, которое используется в явных (should/shouldNot/waitUntil/waitWhile) и неявных ожиданиях для SelenideElement, может быть изменено во время исполнения, e.g. `Configuration.timeout = 6000;`
*  collectionsTimeout - время ожидания в миллисекундах, которое используется в явных (should/shouldNot) ожиданиях для ElementsCollection, может быть изменено во время исполнения, e.g. `Configuration.collectionsTimeout = 8000;`
*  browser (e.g. `"chrome"`, `"ie"`, `"firefox"`)
*  baseUrl
*  reportsFolder
* ...

Также можно передать конфигурационные параметры как system properties для использования в CI (continuous integration) задачах (e.g. -Dselenide.baseUrl=http://staging-server.com/start)

Более подробная информация доступна в Wiki (скоро). // TODO

Более подробно эти и другие классы описаны в [javadoc](http://selenide.org/javadoc.html)

Оставайся на связи!
