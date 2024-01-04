<h2>Структура кода проекта</h2>

<p>Любой проект нуждается в хорошей структуре пакетов. Хорошая структура обеспечивает читаемость кода и возможность параллельной работы над проектом несколькими людьми. При выборе названий для директорий следует четко понимать, какие операции будет выполнять данный пакет.</p>

<p>В первую очередь, необходимо соблюдать конвенции названия вашего пакета:</p>

<p>Обычно это перевернутое доменное имя организации в которой вы работаете.</p>

<p><code>kz.jusan.market.cartservice</code></p>

<p>Если вы делаете проекты для себя и хотите загрузить их на github, то пакет будет выглядеть так:</p>

<p><code>com.github.username.fintechapp</code></p>

<p>Название пакета в <code>Java</code> и в <code>Spring</code> важно по нескольким причинам:</p>

<ul>
	<li>Позволяет избежать коллизий c другими проектами;</li>
	<li>Используется <code>ComponentScan</code> для поиска компонентов;</li>
</ul>

<p>В остальном <code>Spring</code> не ограничивает в выборе подхода для структуры проекта.</p>

<p>Вот несколько рекомендаций по структуре проекта.</p>

<p>Разделение кода проекта может быть:</p>

<ol>
	<li>По типам</li>
</ol>

<pre><code>com 
    +- github 
        +- username
            +- fintechapp 
                +- Application.java 
                +- customer 
                    +- Customer.java 
                    +- CustomerController.java 
                    +- CustomerService.java 
                    +- CustomerRepository.java 
                +- order 
                    +- order.java 
                    +- OrderController.java 
                    +- OrderService.java 
                    +- OrderRepository. java
</code></pre>

<p>В данной структуре все связанные по типу классы помещаются в один пакет.</p>

<p>Для <code>Customer.java</code>: <code>com.github.username.fintechapp.customer</code></p>

<p>В папке заказ находятся: файл с описанием класса, сервис, реализующий бизнес логику и <code>repository</code>, выполняющий доступ в базу.</p>

<p>Плюсом данного подхода является легкость при изменении логики для определенного типа - если мы хотим добавить новое поля для заказа, достаточно изменить файлы в папке <code>order</code>.</p>

<ol>
	<li>По слоям.</li>
</ol>

<p>В данной структуре каждый пакет представляет определенный слой.</p>

<pre><code>com
 +- github
     +- username
        +- fintechapp
            +- MyApplication.java
            |
            +- domain
            |   +- Customer.java
            |   +- Order.java
            |
            +- controllers
            |     +- OrderController.java
            |   +- CustomerController.java
            |
            +- services
            |    +- CustomerService.java
            |    +- OrderService.java
            |
            +- repositories
                +- CustomerRepository.java
                +- OrderRepository.java    </code></pre>