marsframe-springboot
#### 项目列表
- marsframe-core
- marsframe-jd
- marsframe-springboot-starter
- marsframe-timer
#### 使用说明：
- 配置application.properties
		
        #debug模式##
		debug=false

		#mars.frame 开启#################################################
		mars.frame.enabled=true
		mars.frame.jimdb.enabled=true
		mars.frame.jimdb.configPath=classpath:conf/${runZone:LF}/jimdb.properties
		################################################################
		
		##dal接入plugin###############################################
		#dal.configplugin.enabled=true 
		#dal.config.filePath=classpath:conf/dal.properties
		#############################################################

		#京东安全加固#
		server.servlet.jsp.class-name=com.jd.security.tomcat.JDJspServlet
		server.servlet.jsp.init-parameters.enableJsp=false
		server.servlet.jsp.init-parameters.xpoweredBy=false
		server.servlet.jsp.init-parameters.springboot=true
		
		### resources
		spring.mvc.servlet.load-on-startup=0
		spring.http.encoding.charset=UTF-8
		spring.http.encoding.enabled=true 
		spring.http.encoding.force=true 
		server.port=80
		server.context-path=/
		
		# log config
		logging.config=classpath:conf/logback.xml
		
		# 是否允许HttpServletRequest属性覆盖(隐藏)控制器生成的同名模型属性。
		spring.freemarker.settings.recognize_standard_file_extensions=true
		spring.freemarker.allow-request-override=true
		# 是否允许HttpSession属性覆盖(隐藏)控制器生成的同名模型属性。
		spring.freemarker.allow-session-override=true
		# 是否启用模板缓存。
		spring.freemarker.cache=false
		# 模板编码。
		spring.freemarker.charset=UTF-8
		# 是否检查模板位置是否存在。
		spring.freemarker.check-template-location=true
		# Content-Type value.
		spring.freemarker.content-type=text/html
		# 是否启用freemarker
		spring.freemarker.enabled=true
		# 设定所有request的属性在merge到模板的时候，是否要都添加到model中.
		spring.freemarker.expose-request-attributes=true
		# 是否在merge模板的时候，将HttpSession属性都添加到model中
		spring.freemarker.expose-session-attributes=true
		# 设定是否以springMacroRequestContext的形式暴露RequestContext给Spring’s macro library使用
		spring.freemarker.expose-spring-macro-helpers=true
		# 是否优先从文件系统加载template，以支持热加载，默认为true
		spring.freemarker.prefer-file-system-access=true
		# 设定模板的后缀.
		spring.freemarker.suffix=.ftl
		
		# 设定模板的加载路径，多个以逗号分隔，默认: 
		spring.freemarker.template-loader-path=classpath:/META-INF/resources/templates/
		# 设定FreeMarker keys.
		spring.freemarker.settings.template_update_delay=0
		spring.freemarker.settings.default_encoding=UTF-8
		spring.freemarker.settings.number_format=0.##########
		spring.freemarker.settings.date_format=yyyy-MM-dd
		spring.freemarker.settings.datetime_format=yyyy-MM-dd HH:mm:ss
		spring.freemarker.settings.locale=zh_CN
		spring.freemarker.settings.locale=zh_CN
		spring.freemarker.settings.classic_compatible=true
		spring.freemarker.settings.template_exception_handler=ignore

- sqlmap-config.xml的配置，可以使用Criteria构造复杂的sql
<!-- -->
	<typeAliases>
		<typeAlias alias="IbatisHsCriteriaUpdate" 	type="com.mars.kit.criterion.sql.UpdateCriteria" />  
		<typeAlias alias="IbatisHsCriteriaInsert" 	type="com.mars.kit.criterion.sql.InsertCriteria" />  
		<typeAlias alias="IbatisHsCriteria" 		type="com.mars.kit.criterion.sql.IbatisSelect" />
	</typeAliases>
	<mappers>
		<mapper resource="com/mars/kit/criterion/config/sqlmap-mars-criteria.xml" />
	</mappers>

- 数据归档功能配置

     data-archive.properties文件配置：
<!-- -->
	#邮件全局配置#
	data.archiver.mail.host=smtp.jd.local
	data.archiver.mail.port=25
	data.archiver.mail.username=notifying
	data.archiver.mail.password=xxx
	data.archiver.mail.sendername=notifying
	data.archiver.mail.sender=notifying@jd.com
	#数据源beanname配置#
	data.archiver.dataSoruce.name=mysql_dataSoruce_main
    spring中引入：
	<import resource="classpath:archiver/spring/spring-data-archiver.xml" />

	就可以使用
	@Resource
    private ArchiverEngineExecutor dataArchiveEngineExecutor;
    实现数据归档：
    dataArchiveEngineExecutor.archive(config);
- ArchiveConfig的介绍
<!-- -->
	/** where条件*/
    private String where="1=1";

    /** 每次limit取行数据归档处理）*/
    private Integer limit;

    /** 设置一个事务提交一次的数量. 单条插入和单条删除，bulk=false时生效*/
    private Integer txnSize;

    /** 是否删除source数据库的相关匹配记录*/
    private boolean purge = Boolean.TRUE;

    /** progressSize每次归档限制总条数）*/
    private Integer progressSize = Integer.MAX_VALUE;
    
    /**失败尝试次数**/
    private Integer retries = 2;

    /** 分批归档了limit个行记录后的休眠1秒（单位为毫秒）*/
    private Long sleep = 5*1000L;
    
    /**归档的表名***/
    private String srcTblName;
    
    /**归档目标表名**/
    private String desTblName;
    
    /**是否批次执行**/
    private boolean bulk = true;
    
    /**是否发送邮件**/
    private boolean sendEmail;
    
    /**邮件接收人,逗号分隔**/
    private String recipients;
    
- 增加基于 spring的RestTemplate的封装MarsRestTemplate
<!-- --> 
	MarsRestTemplate restTemplate = RestTemplateBuilder.newBuilder().setConnectTimeout(1000)
  	.setReadTimeout(1000).setWriteTimeout(2000).build();
    JSONObject result = restTemplate.postRequest(url,uriVariables,JSONObject.class);
#### 1.0.0-SNAPSHOT版本 （只支持JDK1.8以上）
- 本地缓存切换为高性能Caffeine
- 优化RefreshableLocalCache，自动刷新缓存信息
- 增加UMP切面UmpAspect
- 增加单机限流LimitRater
- 增加hystrix容错机制
- 增加guava-Retryer重试机制

#### 2.0.1-SNAPSHOT版本
- 增加springboot Validator
- 优化RefreshableLocalCache，增加list获取
- 增加本地缓存刷新
#### springboot Validator
- @AssertFalse
<!-- -->
	Boolean, boolean	Checks that the annotated element is false.
- @AssertTrue
<!-- -->
	Boolean, boolean	Checks that the annotated element is true.
- @DecimalMax	
<!-- -->	
	BigDecimal, BigInteger, String, byte, short, int, long and the respective wrappers of the primitive types. 
	Additionally supported by HV: any sub-type of Number.	
	被标注的值必须不大于约束中指定的最大值. 这个约束的参数是一个通过BigDecimal定义的最大值的字符串表示.	
- @DecimalMin
<!-- -->
	BigDecimal, BigInteger, String, byte, short, int, long and the respective wrappers of the primitive types.
	 Additionally supported by HV: any sub-type of Number.	
	被标注的值必须不小于约束中指定的最小值. 这个约束的参数是一个通过BigDecimal定义的最小值的字符串表示
- @Digits(integer=, fraction=)	
<!-- -->	
	BigDecimal, BigInteger, String, byte, short, int, long and the respective wrappers of the primitive types. 
	Additionally supported by HV: any sub-type of Number.	
	Checks whether the annoted value is a number having up to integer digits and fraction fractional digits.	
	对应的数据库表字段会被设置精度(precision)和准度(scale).
- @Future	
<!-- -->
	java.util.Date, java.util.Calendar; Additionally supported by HV, 
	if the Joda Time date/time API is on the class path: any implementations of ReadablePartial and ReadableInstant.
	检查给定的日期是否比现在晚.
- @Max	
<!-- -->	
	BigDecimal, BigInteger, byte, short, int, long and the respective wrappers of the primitive types. 
	Additionally supported by HV: String (the numeric value represented by a String is evaluated), any sub-type of Number.	
	检查该值是否小于或等于约束条件中指定的最大值.	会给对应的数据库表字段添加一个check的约束条件.
- @Min	
<!-- -->
	BigDecimal, BigInteger, byte, short, int, long and the respective wrappers of the primitive types. 
	Additionally supported by HV: String (the numeric value represented by a String is evaluated), any sub-type of Number.
	检查该值是否大于或等于约束条件中规定的最小值.	会给对应的数据库表字段添加一个check的约束条件.
- @NotNull	
<!-- -->
	Any type	Checks that the annotated value is not null.	
	对应的表字段不允许为null.
- @Null	
<!-- -->
	Any type	Checks that the annotated value is null.
- @Past	
<!-- -->
	java.util.Date, java.util.Calendar; Additionally supported by HV, 
	if the Joda Time date/time API is on the class path: any implementations of ReadablePartial and ReadableInstant.	
	检查标注对象中的值表示的日期比当前早.
- @Pattern(regex=, flag=)	
<!-- -->	
	String	检查该字符串是否能够在match指定的情况下被regex定义的正则表达式匹配.
- @Size(min=, max=)	
<!-- -->
	String, Collection, Map and arrays	Checks if the annotated element's size is between min and max (inclusive).	
	对应的数据库表字段的长度会被设置成约束中定义的最大值.
- @Valid	
<!-- -->
	Any non-primitive type	递归的对关联对象进行校验, 如果关联对象是个集合或者数组,
	 那么对其中的元素进行递归校验,如果是一个map,则对其中的值部分进行校验.	
- @CreditCardNumber	
<!-- -->
	String	Checks that the annotated string passes the Luhn checksum test. Note, 
	this validation aims to check for user mistakes, not credit card validity! 
	See also Anatomy of Credit Card Numbers.	
- @Email
<!-- -->	
	String	Checks whether the specified string is a valid email address.
- @Length(min=, max=)
<!-- -->
	String	Validates that the annotated string is between min and max included.	
	对应的数据库表字段的长度会被设置成约束中定义的最大值.
- @NotBlank	
<!-- -->
	String	Checks that the annotated string is not null and the trimmed length is greater than 0. 
	The difference to @NotEmpty is that this constraint can only be applied on strings and that trailing whitespaces are ignored.
- @NotEmpty	
<!-- -->
	String, Collection, Map and arrays	Checks whether the annotated element is not null nor empty.
- @Range(min=, max=)	
<!-- -->
	BigDecimal, BigInteger, String, byte, short, int, long and the respective wrappers of the primitive types	
	Checks whether the annotated value lies between (inclusive) the specified minimum and maximum.
- @SafeHtml(whitelistType=, additionalTags=)	
<!-- -->
	CharSequence	Checks whether the annotated value contains potentially malicious fragments such as <script/>.
	 In order to use this constraint, the jsoup library must be part of the class path.
	 With the whitelistType attribute predefined whitelist types can be chosen.
	 You can also specify additional html tags for the whitelist with the additionalTags attribute.
- @ScriptAssert(lang=, script=, alias=)	
<!-- -->
	Any type	要使用这个约束条件,必须先要保证Java Scripting API 即JSR 223 ("Scripting for the JavaTM Platform")
	的实现在类路径当中. 如果使用的时Java 6的话,则不是问题, 如果是老版本的话,
	 那么需要把 JSR 223的实现添加进类路径. 这个约束条件中的表达式可以使用任何兼容JSR 223的脚本来编写. (更多信息请参考javadoc)
- @URL(protocol=, host=, port=, regexp=, flags=)	
<!-- -->	
	String	Checks if the annotated string is a valid URL according to RFC2396. 
	If any of the optional parameters protocol, host or port are specified, 
	the corresponding URL fragments must match the specified values. 
	The optional parameters regexp and flags allow to specify an additional regular expression 
	(including regular expression flags) which the URL must match.
#### 2.0.2-SNAPSHOT版本
- 入参解析去掉fastjson，换成jackson，减少安全漏洞
