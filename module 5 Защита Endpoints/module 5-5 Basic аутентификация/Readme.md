<p>Давайте скачаем репозиторий с простыми&nbsp;<code>CRUD</code>&nbsp;операциями для студента:&nbsp;<a href="https://github.com/jusan-singularity/spring-school-crud" rel="noopener noreferrer nofollow">ссылка</a></p>

<p>В репозитории есть пакет&nbsp;<code>user</code>&nbsp;в котором есть&nbsp;<code>UserService</code>&nbsp;- выполняющий операции бизнес-логики,&nbsp;<code>StudentRepository</code>&nbsp;- расширяет&nbsp;<code>JDBC CRUD</code>&nbsp;интерфейс.&nbsp;<code>Student</code>&nbsp;- описывает наш&nbsp;<code>Entity</code>,&nbsp;<code>StudentRequest</code>&nbsp;и&nbsp;<code>StudentResponse</code>&nbsp;это&nbsp;<code>DTO</code>&nbsp;(Data Transfer Object) объекты.&nbsp;<code>StudentController</code>&nbsp;- это&nbsp;<code>REST</code>&nbsp;контроллер, которые обрабатывает запросы по ресурсу&nbsp;<code>/students</code>. В зависимостях проекта есть&nbsp;<code>openapi</code>&nbsp;для генерации документации.</p>

<p>В файлах проекта есть&nbsp;<code>resources/data.sql</code>, который добавляет пользователей с паролем&nbsp;<code>password</code>.</p>

<p>Наша задача реализовать аутентификацию на основе связки&nbsp;<code>email:password</code>. Данную аутентификацию мы будем выполнять на основе&nbsp;<a href="https://datatracker.ietf.org/doc/html/rfc7617" rel="nofollow noopener noreferrer"><code>Basic</code></a>&nbsp;аутентификации.</p>

<p>Запрос на аутентификацию происходит через&nbsp;<code>Header</code>&nbsp;-&nbsp;<code>Authorization</code>. После чего следует&nbsp;<code>Basic</code>&nbsp;и&nbsp;<code>base64</code>&nbsp;закодированная связка&nbsp;<code>username:password</code>.</p>

<pre>
<code>Authorization`: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
</code></pre>

<p>Давайте добавим зависимость в наш проект:</p>

<pre>
<code>&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
    &lt;artifactId&gt;spring-boot-starter-security&lt;/artifactId&gt;
&lt;/dependency&gt;</code></pre>

<p>Данная зависимость добавит все необходимые пакеты для работы со&nbsp;<code>Spring Security</code>. Дополнительно, она автоматически создает страницу для аутентификации и устанавливает авторизацию на все ресурсы.</p>

<p>Наша задача добавить аутентификацию на основе аутентификации пользователей, которые есть в нашей таблице&nbsp;<code>students</code>.</p>

<p>Для этого мы можем реализовать собственный&nbsp;<code>UserDetailsService</code>.</p>

<pre>
<code>public interface UserDetailsService {
    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}</code></pre>

<p>Назовем нашу реализацию&nbsp;<code>CustomUserDetailsService</code></p>

<pre>
<code>@AllArgsConstructor
@Service // 1
public class CustomUserDetailsService implements UserDetailsService { 
    private StudentRepository studentRepository; // 2

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException { // 3
        // realization
    }
}</code></pre>

<ol>
	<li>Добавляем аннотацию&nbsp;<code>@Service</code>, чтобы&nbsp;<code>Spring Boot</code>&nbsp;автоматически встроил нашу реализацию.</li>
	<li>Добавляем указатель на&nbsp;<code>CRUD</code>&nbsp;репозиторий.</li>
	<li>Функция, будет принимать&nbsp;<code>email</code>&nbsp;из&nbsp;<code>Header</code>&nbsp;параметра&nbsp;<code>Authorization</code>.</li>
</ol>

<p>Далее, нам необходимо создать реализацию интерфейса&nbsp;<code>UserDetails</code>.</p>

<pre>
<code>public interface UserDetails extends Serializable {
    Collection&lt;? extends GrantedAuthority&gt; getAuthorities();

    String getPassword();

    String getUsername();

    boolean isAccountNonExpired();

    boolean isAccountNonLocked();

    boolean isCredentialsNonExpired();

    boolean isEnabled();
}</code></pre>

<p>Данный объект будет помещен в&nbsp;<code>SecurityContext</code>&nbsp;<code>Spring</code>а. Он будет использоваться при авторизации пользователей.</p>

<pre>
<code>static final class CustomUserDetails extends Student implements UserDetails {
    private static final List&lt;GrantedAuthority&gt; ROLE_USER = Collections
            .unmodifiableList(AuthorityUtils.createAuthorityList("ROLE_USER")); // Создаем роль

    CustomUserDetails(Student student) {
        super(student.getId(), student.getFirstName(), student.getLastName(), student.getPassword(), student.getEmail());
    }
    @Override
    public Collection&lt;? extends GrantedAuthority&gt; getAuthorities() {
        return ROLE_USER;
    }

    @Override
    public String getUsername() { 
        return getEmail(); // Возвращаем email объекта `Student`
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}</code></pre>

<p>Теперь функция для загрузки пользователя будет выглядеть так:</p>

<pre>
<code>@Override
public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
    Student student = this.studentRepository.findStudentByEmail(email);
    if (student == null) {
        throw new UsernameNotFoundException("email " + email + " is not found");
    }
    return new CustomUserDetails(student);
}</code></pre>

<p>Теперь нам нужно сконфигурировать&nbsp;<code>Filter</code>&nbsp;для авторизации.</p>

<p>Создадим конфигурационный файл:</p>

<pre>
<code>@Configuration
@EnableWebSecurity // аннотация для WebSecurity модуля Spring Security
@AllArgsConstructor
public class RestConfig {
    
}</code></pre>

<p>Добавим&nbsp;<code>PasswordEncoder</code>, который будет использоваться для сравнения пароля в запросе и в базе.</p>

<pre>
<code>@Bean
public PasswordEncoder passwordEncoder() {
    BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(16);
    return encoder;
}</code></pre>

<p>В нашей базе пароли зашифрованы через хэш-функцию&nbsp;<a href="https://ru.wikipedia.org/wiki/Bcrypt" rel="nofollow noopener noreferrer"><code>bcrypt</code></a>.</p>

<p>И теперь настроим авторизацию.</p>

<p>Нам требуется оставить открытым доступ к ресурсу&nbsp;<code>swagger</code>&nbsp;и сделать пути к ресурсе&nbsp;<code>/students</code>&nbsp;только для авторизированных пользователей.</p>

<pre>
<code>@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception{
    return http.authorizeHttpRequests()
            .antMatchers("/v3/api-docs/**", "/swagger-ui/**", "/swagger-ui.html").permitAll() // разрешить доступ всем
            .anyRequest().authenticated() // любой запрос требует авторизации
            .and() 
            .httpBasic() // Basic аутентификация
            .and()
            .csrf().disable() // Отключаем csrf токены
            .build();
}</code></pre>

<p>Запустив приложение и перейдя по на страницу с&nbsp;<a href="http://localhost:8080/swagger-ui/index.html" rel="nofollow noopener noreferrer">документацией</a>&nbsp;мы увидим все возможные операции с&nbsp;<code>API</code>, но при попытке выполнения запроса мы увидим окно для ввода&nbsp;<code>email:password</code>. Введя их, мы получим активную сессию и мы будет авторизованы выполнять запросы.</p>

<p><img alt="" height="2258" name="image.png" src="https://ucarecdn.com/ff05644f-6a86-499f-b038-09c19c09ebfc/" width="3034" /></p>

<p><img alt="" height="434" name="image.png" src="https://ucarecdn.com/6e60d95e-20ca-463e-999a-e8bd2d9c43be/" width="1382" /></p>

<p>Для управления&nbsp;<code>аутентификацией</code>&nbsp;в swagger-ui мы можем добавить несколько аннотаций в код.</p>

<pre>
<code>@SpringBootApplication
@SecurityScheme(name = "basicauth", scheme = "basic", type = SecuritySchemeType.HTTP, in = SecuritySchemeIn.HEADER) // 1
public class SchoolApplication {
	public static void main(String[] args) {
		SpringApplication.run(SchoolApplication.class, args);
	}

	@Bean
	public ModelMapper modelMapper() {
		return new ModelMapper();
	}
}</code></pre>

<ol>
	<li>Создание поля для аутентификации в&nbsp;<code>Swagger</code>.</li>
</ol>

<p>И теперь нам нужно дать информацию, для какого контроллера использовать поле&nbsp;<code>basicauth</code>:</p>

<pre>
<code>@RestController
@SecurityRequirement(name = "basicauth")
@RequestMapping("/students")
public class StudentController {
    // rest
}</code></pre>

<p><img alt="" height="2258" name="image.png" src="https://ucarecdn.com/098b4ea9-01f7-48ba-b909-67dc6f8df25e/" width="3034" /><img alt="" height="2258" name="image.png" src="https://ucarecdn.com/66e3c90e-cbb9-422d-84a6-5fe44ba8dfc2/" width="3034" /></p>
