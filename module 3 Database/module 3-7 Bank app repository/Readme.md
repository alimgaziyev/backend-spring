<h3>Какие навыки вы приобретете, выполнив данный проект:</h3>

<ul>
	<li>Работа с СУБД <code>h2</code>;</li>
	<li>Использование <code>JDBC</code>;</li>
	<li>Работа с миграциями <code>flywaydb</code>;</li>
</ul>

<h2>Описание:</h2>

<p>Вы реализовали рабочую программу с бизнес логикой по работе с банковскими операции. Команда разработки просит вас добавить взаимодействия с базой данных.</p>

<p>В данном проекте Вам необходимо реализовать <code>AccountDAO</code> и <code>TransactionDAO</code> с использованием базы данных.</p>

<p>Архитектор проекта отправил вам новое сообщение:</p>

<p>Привет! Ты уже очень близок к завершению. Следующее сообщение будет от меня последним.</p>

<p>На прошлом проекте ты работал с <code>SQL</code>. В реальной работе, операции с базами данных является одним из ключевых моментов в разработке. Любой программе, особенно веб-приложению, необходимо хранить данные в удобном формате.</p>

<p>Но в процессе планирования проекта, не заостряй внимание на выборе типа базы данных. Выбор базы данных должен быть после обсуждения всех бизнес процессов.</p>

<p><img alt="" height="469" name="image.png" src="https://ucarecdn.com/ab40e4af-5ca4-4531-99be-cda03c52c738/" width="651"></p>

<p>Данный график описывает структуру проекта, который подходит под описание чистой архитектуры. <code>DB</code> находится отдельно и его методы вызываются через <code>Gateway</code> из <code>Use Cases</code>.</p>

<p>В нашем примере <code>UseCase</code> - это сервисы <code>AccountDepositService</code>, <code>AccountListingService</code> и другие.</p>

<p><code>Gateway</code> в данном примере это интерфейс <code>AccountDAO</code> и <code>TransactionDAO</code>.</p>

<p>В прошлых проектах мы использовали реализацию <code>AccountDAO</code> в памяти: <code>MemoryAccountDAO</code>. Теперь нам необходимо использовать <code>БД</code>.</p>

<p>В этот раз ты можешь изменять структуру проекта по своему усмотрению. Ты можешь удалить все методы внутри интерфейса <code>AccountDAO</code> и вместо это наследовать его от <code>CrudRepository</code>.</p>

<p>Там где это возможно, используй <code>lombok</code>. Так код станет более читабельным, скрывая очевидные операции.</p>

<p>С наилучшими пожеланиями, Джон Доу</p>

<h2>Основные задачи:</h2>

<ol>
	<li>Подключить <code>Spring Boot JDBC</code>.</li>
	<li>Добавить миграции по созданию таблиц для <code>Account</code> и <code>Transaction</code>.</li>
	<li>Привязать <code>Java</code> классы <code>Account</code>, <code>Transaction</code> к таблицам.</li>
	<li>Создать методы <code>CRUD</code> для <code>Account</code>, <code>Transaction</code>.</li>
</ol>

<p>Конечная программа должна выполнять традиционные банковские операции.</p>

<p>К основным операциям, выполняемым пользователем со счетами в данной версии относятся:</p>

<ol>
	<li>Получение информации о всех счетах</li>
	<li>Создание счета</li>
	<li>Пополнение счета (debit)</li>
	<li>Снятие денег со счета (withdraw)</li>
</ol>

<p>Ключевые факторы при проверке проекта:</p>

<ul>
	<li>Воспользуйтесь репозиторием <a href="https://classroom.github.com/a/85eCW3uN" rel="noopener noreferrer nofollow">GitHub Classroom</a> для этого задания. Работайте на ветке <code><span style="color: #000000;">feature/repository</span></code>. После выполнения задания пришлите ссылку на репозиторий ревьюеру. После успешной приемки проекта не забудьте смерджить ветку <code><span style="color: #000000;">feature/repository</span></code> с <code><span style="color: #000000;">master</span></code>. Это понадобится вам для следующего проекта.</li>
	<li>В репозитории должен быть <code>README.md</code> с описанием запуска программы.</li>
	<li>В проекте не должна происходить инициализация через <code>xml</code>, используй Аннотации.</li>
	<li>Программа должна работать в командной строке.</li>
	<li>Пользователь может создать счет. К счетам относятся:
	<ol>
		<li>Текущий счет (CHECKING)</li>
		<li>Сберегательный счет (SAVING)</li>
		<li>Фиксированный счет (Fixed)</li>
	</ol>
	</li>
	<li>При вызове команды для создания счета, программа должна вывести результат выполнения команды (успешно, неуспешно).</li>
	<li>Пользователь может получить список всех счетов.</li>
	<li>Каждый счет имеет уникальный <code>AccountID</code>.</li>
	<li>Нумерация счетов должна начинаться с 1.</li>
	<li>Сумма на счете не может быть меньше нуля.</li>
	<li>Снимать деньги можно только с <code>Checking</code> и <code>Saving</code> счетов. Формат для <code>AccountID</code> счета:</li>
</ul>

<pre><code>String accountNumber = String.format("%03d%06d", 1, accountID);</code></pre>

<ul>
	<li>Все данные должны сохраняться в базе данных.</li>
</ul>

<p>Пример работы:</p>

<p><img alt="" height="611" name="image.png" src="https://ucarecdn.com/4f99674b-c562-48ce-8c76-6752d8624bcf/" width="600"></p>

<p>Пример точки входа программы:</p>

<pre><code>package com.example.demo;

import com.example.demo.delivery.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class DemoApplication implements CommandLineRunner {
	@Autowired
	private ApplicationContext context;

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class);
	}

	@Override
	public void run(String... arg0) throws Exception {
		boolean running = true;
		String clientID = "1";

		MyCLI myCLI = context.getBean(MyCLI.class);
		AccountBasicCLI accountBasicCLI = context.getBean(AccountBasicCLI.class);
		TransactionDepositCLI transactionDepositCLI = context.getBean(TransactionDepositCLI.class);
		TransactionWithdrawCLI transactionWithdrawCLI = context.getBean(TransactionWithdrawCLI.class);

		String helpMessage = "1 - show accounts\n2 - create account\n3 - deposit\n4 - withdraw\n5 - transfer\n6 - this message\n7 - exit\n";
        System.out.printf("Welcome to CLI bank service\n");
        System.out.printf("Enter operation number: \n");
        System.out.printf(helpMessage);
        while(running){
            switch(myCLI.getScanner().nextLine()){

                case "1":
                    accountBasicCLI.getAccounts(clientID);
                    break;

                case "2":
                    accountBasicCLI.createAccountRequest(clientID);
                    break;

                case "3":
                    transactionDepositCLI.depositMoney(clientID);
                    break;

                case "4":
                    transactionWithdrawCLI.withdrawMoney(clientID);
                    break;

                case "6":
                    System.out.printf(helpMessage);
                    break;

                case "7":
                    System.out.printf("Application Closed\n");
                    running = false;
                    break;

                default:
                    System.out.printf("Command not recognized!\n");
                    break;
            }
        }
        myCLI.getScanner().close();
	}
}</code></pre>