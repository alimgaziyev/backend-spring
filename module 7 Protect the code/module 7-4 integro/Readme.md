<p>Давайте создадим класс <code>StudentController</code>:</p>

<pre><code>public class StudentController {
    @Autowired
    private StudentService studentService;
}</code></pre>

<p>Нам необходимо сразу добавить <code>StudentService</code>.</p>

<p><code>StudentController</code> будет принимать запросы. Для того, чтобы объявить, что класс это контроллер есть аннотация <code>@Controller</code>. <code>Spring Boot</code> автоматически считывает классы контроллеры.</p>

<p>Для определения того, по какому ресурсу должен вызываться метод в контроллере имеется аннотация <code>@RequestMapping</code>. Она может сопоставлять запрос не только по ресурсу но и по методам, параметрам запроса, заголовкам в <code>header</code> и по типу данных.</p>

<p>Каждый метод внутри контроллера может возвращать <code>JSON</code> объекты. Для этого имеется аннотация <code>@ResponseBody</code>, которая сериализует <code>Java</code> объекты в строки в формате <code>JSON</code>.</p>

<p><code>@RestController</code> - аннотация, которая объединяет вместе <code>@Controller</code> и <code>@ResponseBody</code>.</p>

<p>Теперь создадим наш первый метод для обработки <code>GET</code> запросов.</p>

<pre><code>@RestController
@RequestMapping("/students") // Обрабатываем запросы по `URL` - localhost:8080/students
public class StudentController {
    @Autowired
    private StudentService studentService;

    @GetMapping()
    public String hello() {
        return "Hello World!";
    }
}</code></pre>

<p><code>Maven</code> зависимости будут выглядеть так:</p>

<pre><code>&lt;dependencies&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;
    &lt;/dependency&gt;

    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-data-jdbc&lt;/artifactId&gt;
    &lt;/dependency&gt;

    &lt;dependency&gt;
        &lt;groupId&gt;com.h2database&lt;/groupId&gt;
        &lt;artifactId&gt;h2&lt;/artifactId&gt;
        &lt;scope&gt;runtime&lt;/scope&gt;
    &lt;/dependency&gt;

    &lt;dependency&gt;
        &lt;groupId&gt;org.projectlombok&lt;/groupId&gt;
        &lt;artifactId&gt;lombok&lt;/artifactId&gt;
        &lt;optional&gt;true&lt;/optional&gt;
    &lt;/dependency&gt;

    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-test&lt;/artifactId&gt;
        &lt;scope&gt;test&lt;/scope&gt;
    &lt;/dependency&gt;
&lt;/dependencies&gt;</code></pre>

<p>Переменные окружения приложения:</p>

<pre><code># resources/application.properties
spring.h2.console.enabled:true # включаем веб доступ к бд
spring.datasource.url:jdbc:h2:mem:mydatabasename;DB_CLOSE_DELAY=-1; # устанавливаем путь к бд
</code></pre>

<p>Запустим наше приложение:</p>

<pre><code>./mvnw spring-boot:run
</code></pre>

<p>Мы увидим в терминале информацию о запущенном <code>tomcat</code> сервере и порт по которому мы можем взаимодействовать с сервером.</p>

<pre><code>2022-03-15 18:01:39.571  INFO 21749 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-03-15 18:01:39.575  INFO 21749 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-03-15 18:01:39.575  INFO 21749 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/10.0.18]
2022-03-15 18:01:39.612  INFO 21749 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-03-15 18:01:39.612  INFO 21749 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 472 ms
2022-03-15 18:01:39.625  INFO 21749 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2022-03-15 18:01:39.704  INFO 21749 --- [           main] com.zaxxer.hikari.pool.HikariPool        : HikariPool-1 - Added connection conn0: url=jdbc:h2:mem:mydatabasename user=SA
2022-03-15 18:01:39.705  INFO 21749 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2022-03-15 18:01:39.710  INFO 21749 --- [           main] o.s.b.a.h2.H2ConsoleAutoConfiguration    : H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:mydatabasename'
2022-03-15 18:01:40.104  INFO 21749 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-03-15 18:01:40.108  INFO 21749 --- [           main] c.g.z.school.SchoolApplication           : Started SchoolApplication in 1.294 seconds (JVM running for 1.411)
2022-03-15 18:01:44.019  INFO 21749 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2022-03-15 18:01:44.020  INFO 21749 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2022-03-15 18:01:44.020  INFO 21749 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 0 ms
2022-03-15 18:01:44.347  INFO 21749 --- [nio-8080-exec-6] o.springdoc.api.AbstractOpenApiResource  : Init duration for springdoc-openapi is: 74 ms
</code></pre>

<p>Выполним <code>curl</code> запрос:</p>

<pre><code>curl -X 'GET' \
  'http://localhost:8080/students' \
  -H 'accept:</code></pre>

<p>В ответе получим:</p>

<pre><code>Hello World!
</code></pre>

<p>Существуют другие аннотации для различных <code>HTTP</code> методов:</p>

<ul>
	<li>@GetMapping</li>
	<li>@PostMapping</li>
	<li>@PutMapping</li>
	<li>@DeleteMapping</li>
	<li>@PatchMapping</li>
</ul>

<p>Для считывания значения из пути <code>/students/1</code> используется аннотация <code>@PathVariable</code>.</p>

<pre><code>// GET /pets/42

@GetMapping("/pets/{petId}")
public void findPet(@MatrixVariable(required=false, defaultValue="1") int q) {

    // q == 1
}</code></pre>

<p>Аннотация <code>@RequestBody</code> считывает <code>http body</code> запроса и формирует <code>Java</code> объект.</p>

<pre><code>@PostMapping("/accounts")
public void handle(@RequestBody Account account) {
    // ...
}</code></pre>

<p>Для считывания параметров используется <code>@RequestParam("name")</code> - <code>/students?name=az</code>.</p>

<pre><code>// GET /owners/42;q=11;r=12/pets/21;q=22;s=23

@GetMapping("/owners/{ownerId}/pets/{petId}")
public void findPet(
        @MatrixVariable MultiValueMap&lt;String, String&gt; matrixVars,
        @MatrixVariable(pathVar="petId") MultiValueMap&lt;String, String&gt; petMatrixVars) {

    // matrixVars: ["q" : [11,22], "r" : 12, "s" : 23]
    // petMatrixVars: ["q" : 22, "s" : 23]
}</code></pre>

<p>Финальная версия нашего контроллера будет выглядеть так:</p>

<pre><code>package com.github.zulbukharov.school.user;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/students")
public class StudentController {
    @Autowired
    private StudentService studentService;

    @GetMapping()
    public List&lt;Student&gt; getStudents() {
        return studentService.getStudents();
    }

    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) {
        return studentService.getStudentById(id);
    }

    @PostMapping
    public Long createUser(@RequestBody StudentRequest studentRequest) {
        return studentService.createStudent(studentRequest);
    }

    @PutMapping("/{id}")
    public Student updateStudent(@RequestBody StudentRequest studentRequest, @PathVariable Long id) {
        return studentService.updateStudent(studentRequest, id);
    }

    @DeleteMapping("/{id}")
    public boolean deleteStudent(@PathVariable Long id) {
        studentService.deleteStudent(id);
        return true;
    }
}</code></pre>

<ul>
	<li><code>@RestController</code> указывает, что данные, возвращаемые каждым методом, будут записаны прямо в текст ответа вместо отображения шаблона.</li>
	<li><code>@GetMapping("/{id}")</code> указывает, что в аргументах ожидается <code>id</code> - <code>/student/2</code>.</li>
	<li><code>@PathVariable Long id</code> указывает на считываемое значение из <code>URI</code>.</li>
</ul>

<p>Нам нужно создать <code>ControllerAdvice</code>, который будет обрабатывать <code>Exception</code>:</p>

<pre><code>import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.ResponseStatus;

@ControllerAdvice
public class StudentNotFoundAdvice {
    @ResponseBody
    @ExceptionHandler(StudentNotFound.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    String studentNotFound(StudentNotFound ex) {
        return ex.getMessage();
    }
}</code></pre>

<ul>
	<li><code>@ResponseBody</code> сигнализирует, что нужно вернуть результат функции.</li>
	<li><code>@ExceptionHandler</code> настраивает рекомендацию так, чтобы она реагировала только в случае возникновения исключения <code>StudentNotFound</code>.</li>
	<li><code>@ResponseStatus</code> устанавливает <code>HTTP</code> статус для ответа, то есть <code>HTTP</code> <code>404</code>.</li>
</ul>

<p>Запустив данное приложение, мы можем обращаться по созданным роутам.</p>

<p>Давайте теперь создадим документацию для нашего <code>REST</code> сервиса.</p>

<p>Для этого мы будем использовать <a href="https://springdoc.org/" rel="nofollow noopener noreferrer"><code>springdoc-openapi-ui</code></a>.</p>

<p>Он поможет сгенерировать веб страницу с информацией о наших роутах.</p>

<p>Добавим зависимость в <code>pom.xml</code>:</p>

<pre><code>&lt;dependency&gt;
    &lt;groupId&gt;org.springdoc&lt;/groupId&gt;
    &lt;artifactId&gt;springdoc-openapi-starter-webmvc-ui&lt;/artifactId&gt;
    &lt;version&gt;2.0.0-M1&lt;/version&gt;
&lt;/dependency&gt;</code></pre>

<p>Пересоберите проект и запустите его.</p>

<p>Теперь перейдите по ссылке <code>http://localhost:8080/swagger-ui/index.html#/</code></p>

<p> </p>

<p><img alt="" height="521" name="image.png" src="https://ucarecdn.com/adec3ac7-624c-4db5-b916-265815082044/" width="805"></p>

<p>Теперь нам доступна визуальная документация с возможностью выполнения запросов.</p>

<p><img alt="" height="455" name="image.png" src="https://ucarecdn.com/9291f2ba-0d6a-4b29-a99e-edbb72298f82/" width="770"></p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-controller" rel="nofollow noopener noreferrer">Spring Annotated Controllers</a></li>
	<li><a href="https://springdoc.org/" rel="nofollow noopener noreferrer">Springdoc openapi</a></li>
</ul>