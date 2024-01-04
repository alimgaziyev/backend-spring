<p>В <code>Java</code> есть <code>Servlet API</code>, который позволяет нам создавать контроллеры для обработки запросов.</p>

<p>Данная технология была введена в далеких 2000-х годах. И код сервлета выглядит так.</p>

<pre><code>import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyServlet extends HttpServlet { // (1)

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) { // 2
        if (request.getRequestURI().startsWith("/account")) { // 3
            String userId = request.getParameter("userId"); // 4
            // return &lt;html&gt; or {json} or &lt;xml&gt; for an account get request
        } else if (request.getRequestURI().startsWith("/status")) {
            // return &lt;html&gt; or {json} or &lt;xml&gt; for a health status get request
        } // etc
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) { // 5
        // return &lt;html&gt; or {json} or &lt;xml&gt; for a post request, like a form submission
    }
}</code></pre>

<ol>
	<li>Мы расширяем <code>HttpServlet</code>.</li>
	<li>Мы перезаписываем метод <code>doGet</code> для обработки <code>http</code> <code>GET</code> запросов.</li>
	<li>Мы получаем <code>URN</code> - который является путем - <code>/account</code>.</li>
	<li>Мы получаем из запроса параметры - <code>/account?userId=6</code>.</li>
	<li>Мы перезаписываем метод <code>doPost</code> для обработки <code>http</code> <code>POST</code> запросов.</li>
</ol>

<p><code>Servlet</code>ы - должны быть собраны в <code>jar</code> файлы.</p>

<p>После создания <code>jar</code> файла он должен быть загружен на сервер для обработки запросов. Им может выступать <code>Tomcat</code> или <code>Jetty</code>.</p>

<p><code>Tomcat</code> - это веб-сервер, который обрабатывает запросы. Им можно управлять, расширяя функционал на основе загруженных <code>jar</code> <code>Servlet</code>ов.</p>

<p>Минусы использования <code>Servlet</code>:</p>

<ul>
	<li>Реализацию считывания параметров пишет разработчик.</li>
	<li>Нет встроенной валидации форм.</li>
	<li>Нет поддержки <code>I18N</code>.</li>
</ul>

<p><code>Spring Web MVC</code> — веб фреймворк, основанный на <code>Servlet API</code> и являющийся частью Spring framework. В нем учтены и доработаны минусы <code>Servlet API</code></p>

<pre><code>import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HealthController {

    @GetMapping("/health")
    @ResponseBody // (1)
    public HealthStatus health() {
        return new HealthStatus(); // (2)
    }
}</code></pre>

<p><code>Spring Web MVC</code> спроектирован на основе паттерна "Единая точка входа". В <code>Spring Web MVC</code> эту роль выполняет - <code>DispatcherServlet</code>.</p>

<p><code>DispatcherServlet</code> - является точкой входа всех запросов. Вместе с этим, он сканирует все классы в коде, определенные через аннотацию <code>Controller</code>. <code>DispatcherServlet</code> определяет какой из <code>Controller</code>ов должен выполнить запрос.</p>

<p>Конфигурации к <code>DispatcherServlet</code> создаются в <code>web.xml</code> файле или через <code>Java</code> аннотации.</p>

<p>В старых версиях <code>Spring</code> <code>DispatcherServlet</code> необходимо было конфигурировать. С появлением <code>Spring Boot</code> это операция выполняется автоматически.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://tomcat.apache.org/tomcat-5.5-doc/architecture/overview.html" rel="nofollow noopener noreferrer">Tomcat Overview</a></li>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#spring-web" rel="nofollow noopener noreferrer">Dispatcher Servlet</a></li>
</ul>