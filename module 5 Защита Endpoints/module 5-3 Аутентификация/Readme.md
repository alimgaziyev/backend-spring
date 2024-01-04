<p>В Spring Security имеется множество компонентов, связанных между собой и предоставляющих механизм для аутентификации.</p>

<p>Давайте рассмотрим данный график, описывающий принципы работы Аутентификации в <code>Spring Security</code>.</p>

<p><img alt="" height="718" name="image.png" src="https://ucarecdn.com/e6a9b8fd-3668-48d7-93d5-875e92784a01/" width="1050"></p>

<p><code>AuthenticationFilter</code> - это точка входа для запроса. Он, в свою очередь, пытается перевести <code>HTTPRequest</code> в <a href="https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/Authentication.html" rel="nofollow noopener noreferrer"><code>Authentication</code></a> (UsernamePasswordAuthenticationToken). В классе <code>UsernamePasswordAuthenticationToken</code> имеются методы для получения</p>

<p><code>AuthenticationManager</code> - это <code>API</code>, которое описывает поведение <code>Spring Security Filter</code> для аутентификации.</p>

<p><code>AuthenticationProvider</code> - это интерфейс, который описывает выполнение аутентификации. <code>AuthenticationProvider</code> принимает <code>Authentication</code> и возвращает <code>Authentication</code> с заполненными полями. Данные поля в дальнейшем могут пыть использованы при авторизации. К полям относятся: <code>principal</code>, <code>credentials</code> - пароль, <code>authorities</code> - роли.</p>

<p>Существуют встроенные реализации данного интерфейса - <code>DaoAuthenticationProvider</code> и другие.</p>

<p><code>UserDetailsService</code> - используется <code>DaoAuthenticationProvider</code> для получения информации о пользователе для аутентификации.</p>

<p><img alt="" height="584" name="image.png" src="https://ucarecdn.com/4f36171b-e142-44b5-a60b-7e2be23e613e/" width="803"></p>

<ol>
	<li>Создается <code>UsernamePasswordAuthenticationToken</code> в котором есть <code>Username</code> и <code>Password</code>, отправленные пользователем.</li>
	<li><code>ProviderManager</code> вызывает метод аутентификации у <code>DaoAuthenticationProvider</code>.</li>
	<li><code>DaoAuthenticationProvider</code>, вызывает методы у <code>UserDetailsService</code> для получения <code>UserDetails</code> на основе <code>username</code> из <code>UsernamePasswordAuthenticationToken</code>.</li>
	<li><code>PasswordEncoder</code> описывает метод шифрования паролей. Используется для сравнения пароля, отправленного пользователем с паролем из <code>UserDetails</code>.</li>
</ol>

<pre><code>public interface UserDetailsService {
    UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}</code></pre>

<ol>
	<li>В случае, когда пароли совпадают, обновляется <code>UsernamePasswordAuthenticationToken</code> с данными <code>UserDetails</code> и <code>Authorities</code>, которые используются для авторизации.</li>
</ol>

<pre><code>public interface UserDetails extends Serializable {
    Collection&lt;? extends GrantedAuthority&gt; getAuthorities();

    String getPassword();

    String getUsername();

    boolean isAccountNonExpired();

    boolean isAccountNonLocked();

    boolean isCredentialsNonExpired();

    boolean isEnabled();
}</code></pre>

<p><code>Spring Security</code> позволяет самим реализовывать интерфейсы для выполнения аутентификации.</p>

<p>Дополнительные материалы для изучения:</p>

<ul>
	<li><a href="https://docs.spring.io/spring-security/reference/servlet/authentication/index.html" rel="nofollow noopener noreferrer">Spring Security Authentication</a></li>
	<li><a href="https://ru.wikipedia.org/wiki/%D0%90%D1%83%D1%82%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F" rel="nofollow noopener noreferrer">Аутентификация</a></li>
</ul>