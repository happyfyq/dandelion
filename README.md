## Dandelion（浦公英） - 灵活轻量的数据库访问工具

##### 1、对象基本CRUD操作，可轻松选取或屏蔽部分属性，零配置转换对象及参数。
##### 2、ID可手动赋值，也可自动生成全局唯一ID，根据ID进行CRUD操作。
##### 3、可使用SQL生成器生成主体SQL，少量编码即可实现自定义查询。
##### 4、支持MySql数据库分页查询，分页默认从0条开始取100条。
##### 5、清晰有序的方法命名、完善的文档注释、使用原生SQL，极低的学习成本。
##### 6、该工具作为Spring jdbc的增强，可同时使用其中的JdbcTemplate，无后顾之忧。

-----

## 该工具遵循以下约定

##### 1、实体类ID属性名：实体类简单名+Id，例如User实体的ID属性为userId。
##### 2、积极的属性：非null的对象类型（包括包装类型），基本类型大于0或为true的值，反之则为消极的属性，系统默认初始化的值都是消极属性。
##### 3、积极或消极的属性可以作为属性名单配置，用于选取或屏蔽对象的部分属性，故不建议在实体类中使用默认值，也不建议使用基本类型。
##### 4、实体属性名默认与数据库表的列名相同，如需使用下划线风格，请将SqlGenerator.UNDERSCORE修改为true，如使用MySql不建议修改。
##### 5、主键ID为36进制数组成的字符串，21位可使用至8500年后，多机器部署ID永不重复、保持递增趋势、尾数分布均匀，可分库分表可移植数据库。

-----

## 相关依赖及使用说明

#### 有详细注释，可生成Java文档，使用案例可参考UserDaoTests和UserDaoImpl类。

##### 相关依赖：Spring framework、Spring jdbc，4.0.0以上的版本，建议使用4.2.0以上的版本。
##### 使用方式一：将com.ewing.dandelion包全量复制到项目中，并使Spring能够扫描到，再在项目中配置一个JdbcTemplate和NamedParameterJdbcTemplate即可。测试用例使用的是Spring boot自动配置。
##### 使用方式二：将该项目打包成jar包（或作为Maven依赖）引入到开发项目中，通过xml或java的方式配置JdbcTemplate和NamedParameterJdbcTemplate两个Bean并注入CommonBaseDao，GenericBaseDao只需保证继承的子类能被扫描到即可。

##### GenericDao接口及实现类GenericBaseDao，普通DAO类通过继承该接口，可以让该DAO类具有特定实体专用的CRUD方法。
##### CommonDao接口及实现类CommonBaseDao，该类可以通过传递class参数来对任意实体对象进行操作，不需要单独继承泛型接口。
##### pagination包下是对以上两个接口的分页支持，目前支持MySql，如使用其他数据库稍作修改即可。
##### SqlGenerator可以生成任意实体的查询SQL语句主体，可自由使用原生SQL追加条件和参数，灵活度非常高。

##### 注：可配合Spring Cache使用，使用注解声明式的本地缓存或Redis共享缓存，可很好地降低数据库访问次数。

------

## 相关测试结果（因机器而异）

##### 1、对Spring反射的测试，Spring反射机制是该工具的核心，Spring反射内部使用了缓存，经测试平均超过了1000万次/秒，最高可达亿/秒级，效率非常好。
##### 2、全局ID生成效率，同时使用10000个线程、每个线程获取100个ID，一共100万个，耗时2秒左右，100万线程也通过了测试，能满足任意单个节点新增数据的需求。
##### 3、SqlGenerator中无线程竞争，效率和Spring反射的效率在一个级别上，经测试平均也超过了1000万次/秒，整个工具类是非常轻量级的。

