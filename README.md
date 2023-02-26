## Spring Boot
>參考網址：https://zhuanlan.zhihu.com/p/385096203
>
>### SpringBoot概念
>SpringBoot是由Pivotal團隊在2013年開始研發、2014年4月發布第一個版本的全新開源的輕量級框架。它基於Spring4.0設計，不僅繼承了Spring框架原有的優秀特性，而且還通過簡化配置來進一步簡化了Spring應用的整個搭建和開發過程。另外SpringBoot通過集成大量的框架使得依賴包的版本衝突，以及引用的不穩定性等問題得到了很好的解決。
>### SpringBoot主要特性
>* SpringBoot Starter：他將常用的依賴分組進行了整合，將其合併到一個依賴中，這樣就可以一次性添加到項目的Maven或Gradle構建中，也一併解決了依賴衝突等問題
>
>* 使編碼變得簡單：SpringBoot採用JavaConfig的方式對Spring進行配置，並且提供了大量的註解，極大的提高了工作效率。
>
>* 自動配置：SpringBoot的自動配置特性利用了Spring對條件化配置的支持，合理地推測應用所需的bean並自動化配置他們；
>
>* 使部署變得簡單：SpringBoot內置了三種Servlet容器，Tomcat，Jetty,undertow.我們只需要一個Java的運行環境就可以跑SpringBoot的項目了，SpringBoot的項目可以打成一個jar包。
>
>官方參考文檔：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters
>
>**SpringBoot就是將所有的功能場景都抽取出來，做成一個個的starter （啟動器），只需要在項目中引入這些starter即可，所有相關的依賴都會導入進來，我們要用什麼功能就導入什麼樣的場景啟動器即可**
>![image](https://user-images.githubusercontent.com/101872264/220934414-9bc8432c-8212-4b5a-9e73-bece35a0f1ef.png)

### Spring Boot Dev Tools and Spring Boot Actuator
**spring-boot-devtools**
>參考網址：
>1. https://juejin.cn/post/6864404831093587976
>2.https://blog.csdn.net/qq_36134376/article/details/116947557
>
>Spring Boot Dev Tools鉤接（hooks into）到Spring Boot的類加載器中，以提供一種方法來按需重新啟動應用程序上下文或重新加載已更改的靜態文件而無需重新啟動整個應用程序。
為此，Spring Boot Dev Tools將劃分應用程序的類路徑並分配給兩個不同的類加載器：
>
>1. `基本類加載器（base classloader）`：包含一些不可變類或者幾乎不會被修改文件，例如Spring Boot JAR或第三方庫。
>2. `重新啟動類加載器（restart classloader）`：包含應用程序的文件，這些文件在項目開發過程中將頻繁更改。
>
>```
><!-- ADD SUPPORT FOR AUTOMATIC RELOADING -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
</dependency>
>```
>
>>* `自動重啟`
>>
>>spring-boot-devtools熱部署是對修改的類和配置文件進行重新加載，所以在重新加載的過程中會看到項目啟動的過程，其本質上只是對修改類和配置文件的重新加載，所以速度極快。
>>
>>原理：引入devtools之後，項目會用一個base類加載器來加載不改變的類，而會用restart類加載器來加載改變的類。當項目產生修改時，base類加載器不變化，而restart類會重建。類修改時，只對修改過的類重新加載，使得項目重新啟動時速度極快。
>>
>>* `緩存禁用`
>>
>>spring-boot-devtools 對於前端使用模板引擎的項目，能夠自動禁用緩存，在頁面修改後，只需要刷新瀏覽器頁面即可。
>>
>>原理：緩存可以提高性能，但在有模板引擎的開發中，模板引擎會緩存編譯過的模板，防止重複解析模板，這會導致修改頁面內容時，模板引擎不去重新解析模板，看不到修改過的內容，但devtools在開發環境中默認關閉模板引擎的緩存功能。devtools不會被打包進jar包或war包中，在生產環境中，模板引擎的緩存功能就可以正常使用了。

**SpringBoot Actuator**
>參考網址：https://kucw.github.io/blog/2020/7/spring-actuator/
>
>Actuator 是 SpringBoot 提供的監控功能，可以用來查看當前的 SpringBoot 程式運行的內部狀況，譬如知道自動化配置的資訊、創建的 Spring beans 和獲取當前的 properties 屬性值
>
>如果要使用 SpringBoot Actuator 提供的監控功能，需要先加入相關的 maven dependency
>```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
>```
>因為安全的因素，所以 Actuator 默認只會開放`/actuator/health`和`/actuator/info`這兩個 endpoint，如果要開放其他 endpoint 的話，需要額外在 application.properties 中做設置
>
>```
#例：
	// 自訂Info訊息內容
	info.app.name=My Super Cool App
	info.app.description=A crazy fun app, yoohoo!
	info.app.version=1.0.0
	// *在 YAML 中具有特殊含義，因此如果要包含（或排除）所有端點，請務必添加引號。
	management.endpoints.web.exposure.include=*
	management.endpoints.web.exposure.exclude=health,info
>```
>詳細可參考官方文檔：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator.endpoints

## Spring Boot REST API With Hibernate
>創建Spring Boot項目：https://start.spring.io/
>1. 選取Maven項目
>2. 選擇最新Spring Boot版本
>3. 輸入Project基本資訊以及Java版本
>4. 依照需求加入相關依賴資源
>
> 以往透過XML方式或是Java Config一不小心可能漏掉一些資訊或遺漏配置，透過Spring Boot在創建時給定所需依賴與資訊，自動為我們創建好專案，提高開發效率並減少出錯
>
>![image](https://user-images.githubusercontent.com/101872264/221398645-13522485-313d-4e03-9823-dd57a7dc3335.png)

### 此版本將演示透過EntityManager並利用Hibernate API使用在DAO中
**首先在application.properties中添加資料庫連接URL以及使用者名稱與密碼**
```
#
# JDBC properties
#
spring.datasource.url=jdbc:mysql://localhost:3306/employee_directory?useSSL=false&serverTimezone=UTC
spring.datasource.username=springstudent
spring.datasource.password=springstudent
```

**Employee.java程式碼，創建Employee實體類別，並添加相關屬性與資料庫中的字段相映射**
```
@Entity
@Table(name="employee")
public class Employee {

	// define fields
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="first_name")
	private String firstName;
	
	@Column(name="last_name")
	private String lastName;
	
	@Column(name="email")
	private String email;
	
	// define constructors
	
	public Employee() {
		
	}

	public Employee(String firstName, String lastName, String email) {
		this.firstName = firstName;
		this.lastName = lastName;
		this.email = email;
	}

	// define getter/setter
	以下省略...
}
```
**EmployeeDAO.java方法介面，以下列出基本CRUD操作方法**
```
public interface EmployeeDAO {

	public List<Employee> findAll ();
	
	public Employee findById(int theId);
	
	public void save(Employee theEmployee);
	
	public void deleteById(int theId);
}
```
**EmployeeDAOHibernateImpl.java程式碼，實現接口方法，並注入`EntityManager`，Spring會根據application.properties中的配置自動創建一個DataSource並託管，因此只需將其注入到應用程序中即可，並可透過`EntityManager`獲取當前Hibernate會話**
```
@Repository
public class EmployeeDAOHibernateImpl implements EmployeeDAO {

	// define field for entitymanager	
	private EntityManager entityManager;
	
	// set up constructor injection
	@Autowired
	public EmployeeDAOHibernateImpl(EntityManager theEntityManager) {
		entityManager = theEntityManager;
	}

	@Override
	public List<Employee> findAll() {
		
		// get the current hibernate session
		Session currentSession = entityManager.unwrap(Session.class);
		
		// create a query
		Query<Employee> theQuery =
				currentSession.createQuery("from Employee", Employee.class);
		
		// execute query and get result list
		List<Employee> employees = theQuery.getResultList();
		
		// return the results
		return employees;
	}

	@Override
	public Employee findById(int theId) {
		
		// get the current hibernate session
		Session currentSession = entityManager.unwrap(Session.class);
		
		// get the employee
		Employee theEmployee = 
				currentSession.get(Employee.class, theId);
		
		// return the employee
		return theEmployee;
	}

	@Override
	public void save(Employee theEmployee) {
		
		// get the current hibernate session
		Session currentSession = entityManager.unwrap(Session.class);
		
		// save employee
		// Remember if id=0 then save/insert else update
		currentSession.saveOrUpdate(theEmployee);
	}

	@Override
	public void deleteById(int theId) {
		
		// get the current hibernate session
		Session currentSession = entityManager.unwrap(Session.class);
		
		// delete object with primary key
		Query theQuery = 
				currentSession.createQuery("delete from Employee where id=:employeeId");
		theQuery.setParameter("employeeId", theId);
		
		theQuery.executeUpdate();
	}
}
```
**EmployeeService.java服務層方法介面**
```
public interface EmployeeService {

	public List<Employee> findAll();
	
	public Employee findById(int theId);
	
	public void save(Employee theEmployee);
	
	public void daleteById(int theId);
}
```
**EmployeeServiceImpl.java實現接口方法，實際上注入EmployeeDAO對象並委託調用EmployeeDAO方法，同時在方法中添加`@Transactional`註釋，自動為我們在方法執行前後啟閉會話**
```
@Service
public class EmployeeServiceImpl implements EmployeeService {
	
	private EmployeeDAO employeeDAO;
	
	@Autowired
	public EmployeeServiceImpl(EmployeeDAO theEmployeeDAO) {
		employeeDAO = theEmployeeDAO;
	}

	@Override
	@Transactional
	public List<Employee> findAll() {		
		return employeeDAO.findAll();
	}

	@Override
	@Transactional
	public Employee findById(int theId) {
		return employeeDAO.findById(theId);
	}

	@Override
	@Transactional
	public void save(Employee theEmployee) {
		employeeDAO.save(theEmployee);
	}

	@Override
	@Transactional
	public void daleteById(int theId) {
		employeeDAO.deleteById(theId);
	}
}
```
**EmployeeRestController.java完整程式碼，創建Controller控制器並添加CRUD映射配置與方法**
```
@RestController
@RequestMapping("/api")
public class EmployeeRestController {
	
	private EmployeeService employeeService;
	
	@Autowired
	public EmployeeRestController(EmployeeService theEmployeeService) {
		employeeService = theEmployeeService;
	}
	
	// expose "/employees" and return list of employees
	@GetMapping("/employees")
	public List<Employee> findAll(){
		return employeeService.findAll();
	}
	
	// add mapping for GET /employee/{employeeId}
	@GetMapping("/employees/{employeeId}")
	public Employee getEmployee(@PathVariable int employeeId) {
		Employee theEmployee = employeeService.findById(employeeId);
		
		if (theEmployee == null)
			throw new RuntimeException("Employee id not found - " + employeeId);
		
		return theEmployee;
	}
	
	// add mapping for POST /employees - add new employee
	@PostMapping("/employees")
	public Employee addEmployee(@RequestBody Employee theEmployee) {
		
		// also just in case they pass an id in JSON ... set id to 0
		// this is to force a save of new item ... instead of update
		
		theEmployee.setId(0);
		
		employeeService.save(theEmployee);
		
		return theEmployee;
	}
	
	// add mapping for PUT /employees - update existing employee
	@PutMapping("/employees")
	public Employee updateEmployee(@RequestBody Employee theEmployee) {
		
		employeeService.save(theEmployee);
		
		return theEmployee;
	}
	
	// add mapping for DELETE /employee/{employeeId} - delete employee
	@DeleteMapping("/employees/{employeeId}")
	public String deleteEmployee(@PathVariable int employeeId) {
		
		Employee tempEployee = employeeService.findById(employeeId);
		
		// throw exception if null
		if (tempEployee == null)
			throw new RuntimeException("Employee id not found - " + employeeId);
		
		employeeService.daleteById(employeeId);
		
		return "Deleted emploee id - " + employeeId;
		
	}
}
```
![image](https://user-images.githubusercontent.com/101872264/221403128-b3d894a4-0835-4a17-bbea-e32a1d6ef112.png)
