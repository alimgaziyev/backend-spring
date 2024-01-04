<p>В <code>Spring Boot</code> имеются стартеры, один из них это <code>spring-boot-starter-web</code>. Он задается в <code>pom.xml</code> и автоматический считывает <code>Controller</code>ы в коде и запускается их вместе с <code>tomcat</code>.</p>

<pre><code>&lt;dependency&gt;  
    &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;  
    &lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;  
&lt;/dependency&gt;</code></pre>

<p>Это позволяет не тратить время на настройку конфигураций в коде, а сразу заниматься реализацией проекта.</p>

<p>Давайте начнем с простого. Нам нужно создать <code>CRUD</code> для студентов.</p>

<p>Создадим класс <code>Student</code> вместе с миграцией и инициализацией данных. В этом примере не будет использоваться <code>flywaydb</code>.</p>

<pre><code>// Student.java
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import org.springframework.data.annotation.Id;

@Data
@AllArgsConstructor
@Builder
public class Student {
    @Id
    private Long id;
    private String firstName;
    private String lastName;
    private String email;
}</code></pre>

<pre><code>-- resources/data.sql
INSERT INTO student (first_name, last_name, email) values ('a', 'b', 'w@a.com'), ('s', 's', 'w2@a.com');</code></pre>

<pre><code>-- resources/schema.sql
CREATE TABLE student (
    id IDENTITY NOT NULL PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    email TEXT
);</code></pre>

<p>Здесь поле <code>id</code> автоматически увеличивается при добавлении нового поля.</p>

<p>Теперь создадим для него <code>Repository</code>:</p>

<pre><code>// StudentRepository.java
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface StudentRepository extends CrudRepository&lt;Student, Long&gt; {
}</code></pre>

<p>И создадим сервис для <code>CRUD</code> операций:</p>

<pre><code>// StudentService.java
import lombok.AllArgsConstructor;
import org.springframework.data.util.Streamable;
import org.springframework.stereotype.Service;
import java.util.List;

@Service
@AllArgsConstructor
public class StudentService {
    private StudentRepository studentRepository; // 1

    public Long createStudent(StudentRequest userRequest) { // 2
        Student user = Student.builder(). // 3
                email(userRequest.getEmail()).
                firstName(userRequest.getFirstName()).
                lastName(userRequest.getLastName()).
                build();
        Student createdUser = studentRepository.save(user); // 4
        return createdUser.getId(); // 5
    }

    public void deleteStudent(Long id) { // 6
        studentRepository.findById(id).orElseThrow(() -&gt; new StudentNotFound(id)); // 7
        studentRepository.deleteById(id); // 8
    }

    public Student updateStudent(StudentRequest userRequest, Long id) { // 9
        studentRepository.findById(id).orElseThrow(() -&gt; new StudentNotFound(id));
        Student user = Student.builder().
                email(userRequest.getEmail()).
                firstName(userRequest.getFirstName()).
                lastName(userRequest.getLastName()).
                id(id).
                build();
        return studentRepository.save(user);
    }

    public List&lt;Student&gt; getStudents() { // 10
        Iterable&lt;Student&gt; iterable = studentRepository.findAll();
        return Streamable.of(iterable).toList();
    }
    public Student getStudentById(Long id){ // 11
        return studentRepository.findById(id).orElseThrow(() -&gt; new StudentNotFound(id));
    }
}</code></pre>

<ol>
	<li>Объявляем переменную для репозитория с <code>CRUD</code> методами в бд;</li>
	<li>Создаем метод для создания студента, в аргументах принимаем <code>DTO</code> - Data Transfer Object. Эти данные будет отправлять пользователь. В нем нет <code>id</code>.</li>
</ol>

<pre><code>// StudentRequest.java
import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class StudentRequest {
        private String firstName;
        private String lastName;
        private String email;
}</code></pre>

<ol>
	<li><code>.builder()</code> - это метод из <code>lombok</code>, который позволяет функционально создавать объект класса <code>Student</code>.</li>
	<li>Сохраняем студента в бд.</li>
	<li>Возвращаем <code>id</code> нового студента.</li>
	<li>Метод для удаления будет принимать <code>id</code> студента.</li>
	<li>Проверяем наличие студента в, базе либо выбрасываем <code>Exception</code>.</li>
	<li>Вызов метода для удаления студента из бд.</li>
	<li>При обновлении мы принимаем <code>DTO</code> объект и <code>id</code> студента.</li>
	<li>Получение всех студентов.</li>
	<li>Получение студента по <code>id</code>.</li>
</ol>

<p>Наш <code>exception</code> выглядит так:</p>

<pre><code>// exceptions/StudentNotFound.java
public class StudentNotFound extends RuntimeException {
    public StudentNotFound(Long id) {
        super(String.format("Could not find student %d", id));
    }
}</code></pre>

<p>Отлично, мы разобрали основные процессы. Теперь реализуем <code>REST API</code>.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://javarush.ru/groups/posts/2203-stream-api" rel="nofollow noopener noreferrer">Stream</a></li>
	<li><a href="https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html" rel="nofollow noopener noreferrer">Optional</a></li>
	<li><a href="https://projectlombok.org/features/Builder" rel="nofollow noopener noreferrer">Lombok Builder</a></li>
</ul>