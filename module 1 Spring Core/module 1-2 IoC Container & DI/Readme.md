<p><code>Inversion of Control</code> - это принцип ООП, который позволяет уменьшить явную связь объектов друг с другом.</p>

<p>Одной из реализаций, используемых в <code>Spring</code> для <code>IoC</code>, является <code>Dependency Injection</code> - это паттерн, который описывает возможность присваивать разные реализации для объекта класса.</p>

<p>Рассмотрим пример:</p>

<p>У нас есть класс <code>Персональный Компьютер</code>, он включает в себя разные периферийные устройства:</p>

<ul>
	<li>монитор;</li>
	<li>мышь;</li>
	<li>клавиатура;</li>
</ul>

<pre><code>class HPMonitor {
    public void draw() {
    }
}

class RazerMouse {
    public int getMove() {
        return 1;
    }

}

class AnneProKeyboard {
    public int getSignal() {
        return 1;
    }
}

public class PC {
    RazerMouse mouse;
    AnneProKeyboard keyboard;
    HPMonitor monitor;

    public PC(RazerMouse mouse, AnneProKeyboard keyboard, HPMonitor monitor) {
        this.mouse = mouse;
        this.keyboard = keyboard;
        this.monitor = monitor;
    }

    public void turnOn() {
        this.mouse.getMove();
        this.keyboard.getSignal();
        this.monitor.draw();
    }
}</code></pre>

<p>В данном примере мы создали жесткие связи для класса <code>PC</code> с классами: <code>AnneProKeyboard</code>, <code>RazerMouse</code>, <code>HPMonitor</code>;</p>

<p>Но что, если мы хотим поменять девайсы?</p>

<p>Для этого нам будет необходимо переписывать наш класс <code>PC</code>.</p>

<p>Давайте теперь рассмотрим пример, который описывает реализацию <code>dependency injection</code>:</p>

<pre><code>interface Monitor {
    void draw();
}

interface Mouse {
    int getMove();
}

interface Keyboard {
    int getSignal();
}

class HPMonitor implements Monitor {
    public void draw() {
    }
}

class RazerMouse implements Mouse {
    public int getMove() {
        return 1;
    }

}

class AnneProKeyboard implements Keyboard {
    public int getSignal() {
        return 1;
    }
}

public class PC {
    Mouse mouse;
    Keyboard keyboard;
    Monitor monitor;

    public PC(Mouse mouse, Keyboard keyboard, Monitor monitor) {
        this.mouse = mouse;
        this.keyboard = keyboard;
        this.monitor = monitor;
    }

    public void turnOn() {
        this.mouse.getMove();
        this.keyboard.getSignal();
        this.monitor.draw();
    }
}</code></pre>

<p>Теперь мы избавились от зависимости и при инициализации в конструктор <code>PC</code> можно добавлять различные реализации для периферийных устройств.</p>

<pre><code>public class DemoApplication {
    public static void main(String[] args) {
        Monitor monitor = new HPMonitor();
        RazerMouse razerMouse = new RazerMouse();
        AnneProKeyboard anneProKeyboard = new AnneProKeyboard();
        PC pc = new PC(razerMouse, anneProKeyboard, monitor);
    }
}</code></pre>

<h2>Как это работает в Spring</h2>

<p>В <code>Spring</code> имеется <code>IoC Container</code> он отвечает за жизненный процесс экземпляров класса: создание, изменение, удаление, встраивание.</p>

<p><code>Container</code> - состоит из нескольких модулей.</p>

<p><img alt="" height="212" name="image.png" src="https://ucarecdn.com/fcb0ca05-b477-4078-a3f6-b88bcef54612/" width="1174"></p>

<ul>
	<li>Модули <code>Core</code> и <code>Bean</code> предоставляют основные части платформы, включая функции <code>IoC</code> и <code>внедрения зависимостей</code>.</li>
	<li>Модуль <code>Bean</code> предоставляет <code>BeanFactory</code>, сложную реализацию шаблона <code>Фабрика</code>. Он отделяет <code>конфигурацию</code> и спецификацию зависимостей от фактической логики программы.</li>
	<li>Модуль <code>Context</code> основан на модулях <code>Core</code> и <code>Beans</code>. Это средство доступа к любым определенным и настроенным объектам.</li>
</ul>

<p><img alt="" height="505" name="image.png" src="https://ucarecdn.com/1b187915-bfd1-4394-a4db-b0d426061353/" width="618"></p>

<p>В данном графике видно, как происходит процесс запуска контейнера.</p>

<ol>
	<li>Контейнер загружает все классы, написанные разработчиком.</li>
	<li>Контейнер считывает конфигурации, которые описывают как создавать объекты и что с чем связывать, указываются дополнительные настройки, описывающие действия при создании объектов.</li>
</ol>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction" rel="nofollow noopener noreferrer">Spring IoC Container Intro</a></li>
</ul>