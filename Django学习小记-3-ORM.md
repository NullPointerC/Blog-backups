---
title: Django学习小记-3-ORM
categories: [Django]
tags:
  - Python
  - backend
  - Django
date: 2021-08-17 22:23:31
---

## 一、Django ORM

Django的ORM模块是框架特色功能之一，它把数据表与Python类对应、表字段与类属性对应、类实例与数据记录对应，并将对类实例的操作映射到数据库中。开发者不再需要写SQL代码，可以更加专注地完成业务逻辑，极大地提高了开发效率。

### 1.1 应用的Models定义

由于每一个数据表对应一个Model定义，每一个Model都是一个Python类，所以，Model之间是可以继承的。Django规定，所有的Model都必须继承自django.db.models.Model，可以是直接继承，也可以是间接继承。Model中的所有字段都是django.db.models.Field的子类，Django会根据Field的类型确定数据库表（与选择使用的数据库有关）的字段类型。Django内置了数十种Field字段类型，不同的类型支持的参数不一定相同，但是像名称、帮助文本、唯一性等参数都是通用的。

id字段是在Model定义中没有主动指定主键的情况下，Django自动加上去的。

### 1.2 应用完成数据库迁移

在执行迁移命令之前，需要把应用加载到项目中，如在INSTALLED_APPS的第一行加入post.apps.PostConfig：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210817225539.jpeg)

之后，对post应用执行makemigrations命令，在post/migrations包下面生成迁移文件：

```python
python manage.py makemigrations post
```

迁移文件也是Python文件，可以利用manage.py提供的sqlmigrate命令打印迁移文件执行的SQL语句：

```python
python manage.py sqlmigrate post 0001
```

sqlmigrate命令后面跟随应用名称和迁移文件的名称，执行后在控制台打印SQL语句。

sqlmigrate命令并不会实现数据库迁移，它只是将Django会执行的迁移SQL输出到控制台，以便让用户能够知道Django会怎么做，并从中发现可能的问题。当然，manage.py提供了更为简单的命令帮助用户检查项目中的问题，可以在manage.py所在的目录下执行：

```python
python manage.py check
```

如果没有问题，会看到控制台上打印输出：System check identified no issues(0silenced)。同样，check命令也不会影响数据库。

查看了SQL语句，也验证了项目的正确性，接下来就可以执行migrate命令将Models映射为数据库的表了：

```
python manage.py migrate
```

应用Django的规则，将表名定义为：应用名_小写类名。

### 1.3 Model类

Model是Django ORM的核心，它有许多特性（如继承模型、元数据），同时也要遵守一些规则，例如字段类型必须是Field类型。

#### 1.3.1Model的组成部分

每个Model都是一个Python类，且通常会包含四个部分：继承自django.db.models.Model、Model元数据声明（Meta内部类）、若干个Field类型的字段以及__str__方法。

1.django.db.models.Model

通过类之间的继承，Django主要对自定义的Model添加了两个属性。

（1）id：Django规定，每一个Model必须有且仅有一个Field字段的primary_key属性设置为True，即必须要有主键。通常，在自定义Model的时候，不需要关注主键字段，基类会自动添加一个auto-incrementing id作为主键。

（2）objects：它是Manager（django.db.models.Manager）类的实例，被称为查询管理器，是数据库查询的入口。每个Django Model都至少有一个Manager实例，可以通过自定义创建Manager以实现对数据库的定制访问，但通常是没有必要的，后面的内容会看到默认Manager的强大功能。

2.Meta内部类声明元数据

Meta是一个类容器，Django会将容器中的元数据选项定义附加到Model中。常见的元数据定义有：数据表名称、是否是抽象类、权限定义、索引定义等。Meta定义的元数据相当于Model的配置信息，可以直接在shell环境中打印出来，例如，对于Topic的定义，可以使用如下方法查看（为方便阅读，对结果进行了格式化）：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210817230206.jpeg)

Meta内部类是可选的，用户的Model如果没有需要完全可以不定义Meta，Django会自动应用默认的元数据到Model上。

3.定义数据表项的Field实例

Model中的每个字段都是Field（django.db.models.fields.Field）类型的实例，Django会根据Field的实际类型确定以下几个信息。

（1）数据库中的列类型，如CharField对应MySQL中的varchar。

（2）渲染表单时使用的默认HTML部件。

（3）管理后台与自动生成的表单中的数据验证。每个Field类型都定义了一些参数，这些参数大都会直接反映到数据表的Schema中，例如：unique设置为True反映这一列数据在表中是唯一的；primary_key设置为True会将这一列设置为表的主键等。需要注意的是，存在部分参数是通用的，即可以用在任何一种Field类型中。

4.__str__方法

__str__方法是Python中的“魔术”方法，它是为print这样的打印函数设计的。如果没有这个方法定义，打印对象会显示对象的内存地址，但是，这样的显示方式不够友好，且不利于调试。Python的这个特性同样对Django的Model实例生效，例如针对Topic的实例直接打印的话，会显示id和title的前20个字符。不仅如此，将来管理后台也可以看到__str__方法发挥的作用，它会将函数的返回值作为对象的显示值。

#### 1.3.2 Meta元数据类属性说明

Meta类用于定义Model的元数据，即不属于Model的字段，但是可以用来标识它的一些属性。

（1）abstract

一个布尔类型的变量，如果设置为True，则标识当前的Model是抽象基类，这个元选项不具有传递性，只对当前声明的类有效。例如，对于之前定义的BaseModel，用abstract声明为抽象基类，但是子类Topic和Comment不受影响。

（2）proxy

默认值是False，如果设置为True，则表示为基类的代理模型。

（3）db_table

这个字段用于指定数据表的名称。通常，如果没有特别的需要，默认会使用Django的表名生成规则，例如Topic会映射到post_topic表。如果想让Topic映射到topic表，定义db_table='topic'即可。db_table元选项对抽象基类是无效的，也不应该在抽象基类中去声明它。因为抽象基类可以被多个子类继承，如果数据表名也可以继承，那么，在数据库创建表的时候就会抛出错误。

（4）ordering

用于指定获取对象列表时的排序规则，它是一个字符串的列表或元组对象，其中的每一个字符串都是Model中定义的字段名，字符串前面可以加上“-”代表逆序，默认按照正序排序。

按照created_time正序排序，可以定义：

```python
ordering = ['create_time']
```

按照created_time逆序排序，可以定义：

```python
ordering = ['-create_time']
```

先按照created_time正序排序，再按照last_modified逆序排序，可以定义：

```python
ordering = ['create_time','-last_modified']
```

排序对于数据库查询是有代价的，所以，只有当所有的查询都需要按照特定的规则排序时才需要设定这个元选项，否则，可以在特定的查询中指定排序规则，不要做统一的定义。

（5）managed

它是一个布尔类型的变量，默认为True，代表Django会管理数据表的生命周期，即利用Django提供的工具可以完成创建和删除数据表。如果设置为False，唯一的不同之处就是Django不会管理这些Model所对应的数据表。这个元选项通常不需要考虑，除非用户在执行migrate命令之前已经在数据库中创建了数据表。此时就需要把它设置为False，由用户自己去做管理，但这样的声明需要注意以下问题。

①如果Model中没有声明主键，那么即使将managed设置为False的情况下，Django仍然会自动添加一个名称为id的自增主键。由于已经设置为自己去管理数据表了，所以，最好是手动指定数据表中的所有数据列，避免造成混乱。

②如果Model中包含ManyToManyField类型的字段，且指向的Model也是自管理的（managed=False），那么，Django不会给这种关系创建中间表，需要主动创建中间表，并使用ManyToManyField.through指定关联关系。

（6）indexes它是一个列表类型的元选项，用来定义Model的索引，列表中的每一个元素都是Index（django.db.models.indexes.Index）类型的实例。Index的类定义如下：

```python
class Index(fields=[], name=None, db_tablespace=None)
```

fields：一个列表对象，用于指定索引的字段，是必填项，且至少包含一个字段。

name：用于标识索引的名称，是可选的，如果不提供，Django会根据不同的数据库生成合适的索引名。

db_tablespace：表空间，也是可选的，常见于PostgreSQL、Oracle数据库，用于优化数据库性能。MySQL数据库是不支持表空间的，Django也不会主动创建表空间，如果设置了这个字段，但是存储后端并不支持，则Django会自动忽略这个选项。

Topic使用indexes元选项的一个简单的示例：

```python
from django.db import models
indexes = [
	models.Index(fields=['title']),
	models.Index(fields=['title', 'is_online'], name='title_isonline_idx')
]
```

（7）unique_together

这是一个很常见的元组类型元选项，包含多个元组对象，标识联合唯一约束，在数据库层面表现为联合唯一索引。它的优点是将对数据取值的限制抽离出业务逻辑，放在了框架和数据库层面去处理，使整体的逻辑显得更为清晰。Topic使用unique_together元选项的简单示例：

```python
unique_together = (('title','is_online'),('title','content'))
```

特别地，如果unique_together只包含一个元素，可以直接写成：

```python
unique_together = ('title','is_online')
```

（8）verbose_name和verbose_name_plural

这两个元选项用于给Model类起一个方便阅读的名称，主要用在管理后台上的展示，其中verbose_name_plural是模型类的复数名。如果不设置的话，Django会使用小写的模型名作为默认值，且会将驼峰式的名称拆分为独立的单词，复数会参照verbose_name加上字母s。

（9）default_permissions

Django默认会给每一个定义的Model设置三个权限（权限是用户的一种属性，用户拥有这种属性，就具备了一些能力，关于权限后面会有详细的讲解分析，这里只需要知道这个概念）：('add','change','delete')。如果不需要这些默认的权限，或者只需要其中的一部分，可以根据自己的需要重新定义这个元选项。

（10）permissions

除了Django默认给Model添加的三个权限之外，还可以通过这个元选项给Model添加额外的权限。permissions是一个包含二元组的元组或者列表，元素的格式为：(权限代码,权限名称)。

如果想给Topic添加阅读和评论的权限，可以这样写：

```python
permissions = (
	("can_read_topic","可以阅读话题")
	("can_discuss_topic",可以讨论话题)
)
```

#### 1.3.3 Field的通用字段选项

Model中添加的字段都是Field类型的实例，且在Topic和Comment中已经看到过如default、unique等字段选项。不同的Field类型可能会支持不同的字段选项，但是，还是有很多字段选项是通用的，即可以用在任何一种Field类型中。

（1）blank

默认值是False，它是数据验证相关的字段，主要体现在管理后台录入数据的校验规则。对于任何一个属性，默认是不允许输入空值的，如果允许这种情况发生，需要设置blank=True。

（2）unique

默认值是False，它是一个数据库级别的选项，规定该字段在表中必须是唯一的，但是对ManyToManyField和OneToOneField关系类型是不起作用的。需要注意的是，数据库层面对待唯一性约束会创建唯一性索引，所以，如果一个字段设置了unique=True，就不需要对这个字段加上索引选项了。

（3）null

默认值是False，它是一个针对数据库的选项，影响表字段属性，规定这个字段的数据是否可以是空值。如果将其设置为True，则Django会在数据库中将空值存储为NULL。对于CharField和TextField这样的字符串类型，null字段应该总是设置为False，如果设置为True，对于“空数据”就会有两种概念：空字符串和NULL。有一个例外，是当CharField同时设置了unique=True和blank=True，那也需要设置null=True，这是为了防止在保存多个空白值时违反唯一性约束。

（4）db_index

默认值是False，如果设置为True，Django则会为该字段创建数据库索引。如果该字段经常作为查询的条件，那么就需要考虑设置db_index选项，以加快数据的检索速度。

（5）db_column

这个选项用于设置数据库表字段的名称。如果没有指定这个选项，Django会直接使用Model中字段的名称。需要注意的是，如果db_column设置的是数据库的保留字（如MySQL中的all）或者是包含了Python不能接受的变量名（如a-b），那么，Django会给数据列的名字加上引号。建议用户在设置列名之前，最好先查看自己所使用的数据库后端，不要与保留字发生冲突。

（6）default

用于给字段设置默认值。该选项可以设置为一个值或者是可调用对象，但是不能是可变对象，例如Model实例、列表、集合对象等。针对Topic中的is_online字段，设置默认值对象可以是：

```python
is_online = models.BooleanField(default=True, help_text=u'话题是否在线')
```

针对title字段，设置可调用对象可以是：

```python
def default_title():
	return 'default title'
title = models.CharField(max_length=255, unique=True, default=default_title)
```

同时，在使用default的时候，需要注意以下几点。

①lambda表达式不可以作为default的参数值，因为它不能被migrations命令序列化。

②对于ForeignKey这样的字段，默认值设置的应该是主键而不应该是Model对象实例。

（7）primary_key

默认值是False，如果某个字段设置该选项为True，则它会成为Model的主键字段，且不允许其他的字段再次将该选项设置为True。Model定义中如果没有设置该选项，那么Django会自动添加一个名称为id的AutoField类型的字段。通常情况下，Django的这种默认行为能够满足大部分的场景。同时，对于primary_key，需要注意它的两个特性。

①在数据库层面，primary_key=True就意味着对应的字段唯一且不能是NULL。

②主键字段是只读的，所以，如果用户修改了主键字段的值，并执行了保存动作，结果是创建了一条新的数据记录。

（8）choices

这个选项用于给字段设置可以选择的值。它是一个可迭代的对象，即列表或者元组，其中每一个元素都是一个二元组：（A，B）。A是用来选择的对象，即作为字段值使用；B是对A的描述信息。设置了choices的字段在管理后台的显示上会由文本框变成选择框，选择框中的可选值就是choices中的元组。

Django建议将choices定义在Model的内部，使代码整体看起来更加规整。例如，要定义一个Peop le模型，它有一个gender字段用来标识性别，那么，People的定义可以是这样：

```python
class Person(models.Model):
	MALE='m',
	FEMALE='f',
	GENER_CHOICES=(
		(MALE,'男性'),
		(FEMALE,'女性')
	)
	gener = models.CharField(max_length=1,choice=GENER_CHOICES, default=MALE)
```

如果一个字段的可选值是可以枚举出来的，那么choices选项是个不错的选择。但是，需要注意的是，Django并不会把这一列设置为数据库中的枚举类型，它还是会遵循Field类型对应的数据表字段类型。例如，gender虽然设置了choices选项，但它在数据表中的类型仍然是varchar。

（9）help_text

这个选项用于在表单中显示字段的提示信息。例如在管理后台的编辑页面，对应在字段输入框的下方会显示该选项设定的值。由于表单通常提供给非技术人员，完善的提示信息将更加方便校验和录入字段数据，所以，对字段添加解释信息是很有必要的。

（10）verbose_name

这个选项用于给字段设置可读性更高的名称，通常也是用在表单的展示上。如果没有设置这个字段，Django将会直接展示字段名，且首字母大写。如果字段中存在下画线，Django会将它转换为空格。这个选项要与help_text区分开来，verbose_name可以认为是字段的别名，而help_text可以认为是对字段的描述。

#### 1.3.4 基础字段类型

Django内置的字段类型，按照是否与其他的Model存在关联可以划分为两类：基础字段类型和关系字段类型。关系字段类型只有三个，基础字段类型较多，又可以细分为：字符串、数字、时间和二进制。

1.django.db.models.FieldField

是所有字段类型的基类，不管是Django内置的字段类型还是自定义的字段类型都需要继承自它。通常对于Field，只需要关注三个方面：映射到数据表的列类型（db_type）、将Python对象映射到数据库（get_prep_value）、从数据库返回Python对象（from_db_value）。

（1）db_type(connection)它会根据所配置的数据库后端返回Field对应的数据库列类型。所以，如果对这个方法重载，那么应该需要考虑不同的数据库后端列类型兼容的问题。db_type方法只会在Django为Model生成创建表语句的时候调用一次，其他任何时候都不会被使用。所以，即使这个方法的实现较为复杂，也不会影响系统性能。

（2）get_prep_value(value)参数value是Model属性的当前值，通过该方法对value的操作，最终返回用作数据库查询的参数。注意，Model对象保存到数据库中的时候，也会将该方法的返回值作为该列数据保存。

（3）from_db_value(value,expression,connection)。与get_prep_value的作用正好相反，它会将从数据库中返回的数据转换为Python对象。因为数据库后端能够返回正确的Python类型，所以这个方法在大多数内置的字段中都没有用到。

2.常用的基础字段类型

（1）IntegerField整型字段，取值为-2147483648～2147483647，如果需要使用数字类型，且字段的取值范围符合要求，可以考虑使用该类型，例如Comment中的up和down。Django还提供了SmallIntegerField（小整数）、BigIntegerField（64位整数）和PositiveIntegerField（只允许存储大于等于0的整数）等字段类型用来满足存储整数的不同业务场景。

（2）AutoField一个根据ID自增的IntegerField。如果Model中没有定义主键，那么，Django会自动添加一个名称为id的该类型字段作为主键。如果觉得AutoField的取值范围不够用，可以考虑使用BigAutoField，它继承自AutoField，但是它使用的是8个字节的存储空间。

（3）CharField字符字段，是最常用的字段类型。它有一个必填的参数max_length，且取值只能是大于0的整数，将会在数据库中和表单验证的时候用到。

（4）TextField与CharField类似，也是用于存储字符类型的字段，但是它用于存储大文本。在字符型字段的选择上，如果需要限制它的最大长度，例如Topic的title（标题），那么就选择CharField类型；反之，就选择TextField，例如Topic的content（内容）。

（5）BooleanField布尔类型，在某个字段的取值只能是True或False的情况下选择使用该字段类型，例如Topic中的is_online。

（6）DateField和DateTimeField这两个字段类型是用来标识时间的，几乎在任何一个Model定义中都能看到会至少引用它们其中的一个。其中Date是日期，以Python中的datetime.date实例表示；DateTime是日期时间，以Python中的datetime.datetime实例表示。

它们都有两个特殊的参数选项可以设置。

①auto_now：这个选项应用在对象保存的时候，会自动设置为当前时间。

②auto_now_add：当首次创建对象的时候，会自动将字段设置为当前时间。

注意，auto_now和auto_now_add与default是互斥的，不应该将它们组合在一起使用。

（7）EmailField它继承自CharField，也用来存储字符串类型的数据。但是，就像它的名字一样，它是专门用来存储电子邮件地址的。其内部使用EmailValidator验证器对输入的字符串进行校验。一个经典的例子是Django用户系统的User定义，其中email字段使用的就是EmailField类型。

3.自定义一个字段类型

Django提供了数十种基础字段类型，除了之前介绍的几种常见的类型之外，还有GenericIPAddressField用于存储IP地址、URLField用于存储URL、FloatField用于存储浮点数的字段类型等。如果觉得这些都不能满足业务场景的需求，也可以选择自定义一个字段类型。通常，自定义的类型不需要继承自Field，因为这样会比较麻烦，而是可以选择继承自Django内置的基础字段类型。考虑这样一种场景：对于用户发表的每一个Topic，在把数据存储到title的时候，可以在title的前面加上一个签名（sign）。这需要自定义一个SignField，可以直接继承自CharField，并重写get_p rep_value(value)方法，它的实现可以是这样的：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210817232149.jpeg)

title的定义也需要修改为：

```python
title = SignField(max_length=255, unique=True, help_text='话题标题')
```

这样，在每次创建或修改Topic时，保存到数据库的title字段的前面就会有<my_bbs>签名了。

#### 1.3.5 三种关系字段类型

Django定义了三种关系类型用来描述数据库表的关联关系：多对一（ForeignKey）、一对一（OneToOneField）以及多对多（ManyToManyField）。

1.多对一关系类型

这种类型在数据库中的体现是外键关联关系，它可以和其他的Model建立关联，同时可以和自己建立关联，用来描述多对一的关系。

例如Topic和Comment的关系：一个Topic可以有多个Comment，但是一个Comment只能对应一个Topic。在数据库中的字段命名上，Django会将字段的名称添加“_id”作为列名，可以看到Comment表中topic_id一列。ForeignKey的定义如下：

```python
class django.db.models.Foreignkey(to, on_delete, **options)
```

它有两个必填的参数。

to：指定所关联的Model，它的取值可以是直接引用其他的Model，也可以是Model对应的字符串名称。如果要创建递归的关联关系，即Model自身存在多对一的关系，可以设置为字符串self。

on_delete：当删除关联表的数据时，Django将根据这个参数设定的值确定应该执行什么样的SQL约束。在django.db.models中定义了on_delete的可选值如下。

CASCADE：级联删除，它是大部分ForeignKey的定义应该选择的约束。它的表现是删除了“一”，则“多”会被自动删除。以Topic和Comment的关系举例：如果删除了Topic，那么所有与该Topic关联的Comment都会被删除。

PROTECT：删除被引用对象时，将会抛出ProtectedError异常。以Topic和Comment的关系举例：当一个Topic对象被一个或多个Comment对象关联时，删除这个Topic就会触发异常。当然，如果一个Topic还没有被Comment关联，是可以被删除的。

SET_NULL：设置删除对象所关联的外键字段为null，但前提是设置了选项null为True，否则会抛出异常。以Topic和Comment的关系举例：删除了Topic，与之相关联的Comment的topic字段会被设置为null。

SET_DEFAULT：将外键字段设置为默认值，但前提是设置了default选项，且指向的对象是存在的。以Topic和Comment的关系举例：id是1的Topic有2个Comment关联，且Comment的外键字段设置了default=2，那么，删除了id是1的Topic之后，与之关联的Comment的外键字段就会被置为2了。

SET(value)：删除被引用对象时，设置外键字段为value。value如果是一个可调用对象，那么就会被设置为调用后的结果。以Topic和Comment的关系举例：Comment的外键字段设置了on_delete=models.SET(1)，删除id是2的Topic，与之相关联的Comment的外键字段就会被设置为1。

DO_NOTHING：不做任何处理。但是，由于数据表之间存在引用关系，删除关联数据，会造成数据库抛出异常。

除了必填的参数之外，ForeignKey还有一些常用的可选参数需要关注。

to_field：关联对象的字段名称。默认情况下，Django使用关联对象的主键（大部分情况下是id），如果需要修改成其他字段，可以设置这个参数。但是，需要注意，能够关联的字段必须有unique=True的约束。

db_constraint：默认值是True，它会在数据库中创建外键约束，维护数据完整性。通常情况下，这符合大部分场景的需求。但是，如果数据库中存在一些历史遗留的无效数据，则可以将其设置为False，这时就需要自己去维护关联关系的正确性了。

related_name：这个字段设置的值用于反向查询，默认不需要设置，Django会设置其为“小写模型名_set”。如果不想创建反向关联关系，可以将它设置为“+”或者以“+”结尾。

related_query_name：这个名称用于反向过滤。如果设置了related_name，那么将用它作为默认值，否则Django会把模型的名称作为默认值。

2.一对一关系类型

OneToOneField继承自ForeignKey，在概念上，它类似unique=True的ForeignKey。它与ForeignKey最显著的差别体现在反向查询上：ForeignKey反向查询返回的是一个对象实例列表，而OneToOneField反向查询返回的是一个对象实例。OneToOneField的定义如下：

```python
class django.db.models.OneToOneField(to, on_deleted,parent_link=False, **options)
```

根据定义可以看出，它与ForeignKey的参数几乎是一样的，只是多了一个可选参数parent_link：当其设置为True并在继承自另一个非抽象的Model中使用时，那么该字段就会变成指向父类实例的应用，而不是用于扩展父类并继承父类的属性。

通常一对一关系类型用在对已有Model的扩展上。例如通过对内置的User进行扩展，添加类似爱好、签名等字段时，就可以新建一个Model，并定义一个字段与User进行一对一的关联。具体实现如下：

```python
from django.db import models
from django.contrib.auth.models import User
class customUser(models.Model):
	user = models.OneToOneField (to=User,on_delete=models.CASCADE)
    sign = models.CharField (max_length=255,help_text='用户签名')
```

3.多对多关系类型

多对多关系也是比较常见的，例如一个作者（Author）可以写很多本书（Book），一本书也可以由多个作者共同完成，那么，Author与Book之间就是多对多的关系。Django通过中间表来维护Model与Model之间的多对多关系。这个中间表可以是自己提供的，也可以直接使用Django默认生成的。ManyToManyField的定义如下：

```python
class django.db.models.ManyToManyField(to,**options)
```

它有一个必填的参数to，与ForeignKey和OneToOneField在概念上是一致的，都是用来指定与当前的Model关联的Model。

另外，ManyToManyField还有一些重要的可选参数需要关注。

related_name：与ForeignKey中的related_name作用相同，都是用于反向查询。

db_table：用于指定中间表的名称。如果没有提供，Django会使用多对多字段的名称和包含这张表的Model的名称组合起来构成中间表的名称（仍然会有App的前缀）。

through：用于指定中间表。这个参数通常不需要设置，因为Django会默认生成隐式的through Model。由于Django有自己的中间表生成策略，因此如果用户想自己去控制表之间的关联关系或者增加一些额外的信息，就可能会使用这个参数，例如，在关联表中增加描述性字段。

针对Author与Book之间的多对多关系，可以这样定义：

```python
from django.db import models
class Book (models.Model) :
	title = models.CharField (max_length=64, help_text='书名')
class Author(models.Model):
	name = models.CharField (max_length=32，help_text='作者姓名')
    books = models.ManyToManyField(to=Book)
```

#### 1.3.6Model的继承模型

Django Model的继承与Python类的继承是一样的，只是Django要求所有自定义的Model都必须继承自django.db.models.Model。

在Django中Model之间有三种继承模型，它们分别是抽象基类、多表继承和代理模型。

1.抽象基类

抽象Model专门设计为被其他的子类继承，它将子Model中通用的元素聚合到一起，以便子Model不用多次重复定义这些通用的部分，且对于修改也只需要操作基类，节省了很多工作量。

像BaseModel一样，定义Meta内部类，并将abstract设置为True，就把当前的Model定义为抽象基类了。这类Model不能被实例化，Django不会为它创建数据表，相应地，也就没有查询管理器。关于Model的元数据继承关系，遵循以下几个规则。

（1）抽象基类中定义的元数据，子类中没有定义，子类会继承基类中的元数据。

（2）抽象基类中定义的元数据，子类也定义了，子类优先级更高。

（3）子类可以定义自己的元数据，即不出现在抽象基类中的元数据。

在定义抽象基类时，需要注意，如果定义了ForeignKey或ManyToManyField类型的字段，并且设置了related_name或者related_query_name参数，由于继承关系，子类也会拥有同样的字段，所以，就需要想办法让子类中的反向名称和查询名称是唯一的。

Django提供了特殊的语法解决这个问题。

'%(class)s'：会替换为子类的小写名称。

'%(app_label)s'：会替换为子类所属App的小写名称。

由于Django中每个App的名称都应该是不同的，且每个App中所有Model的名称都应该是不同的，所以，组合在一起的名称就一定是唯一的。

如果在抽象基类中定义了与其他Model存在关联关系的字段，也可以不去设置related_name或related_query_name参数，使用Django默认生成的名称，且保证子类中不要定义这个默认名称的同名字段。

2.多表继承

这种继承方式的效果是父Model和子Model都会有数据库表，且Django会自动给子Model添加一个OneToOneField类型的字段指向父Model，而这个字段会成为子Model数据表的主键。

3.代理模型

代理模型的使用场景是需要给原始的Model添加一些方法或者修改它的Meta选项，但是不需要修改原始Model的字段定义。为原始的Model创建一个代理，那么对代理模型的增删改的操作都会被保存到原始的Model中。

创建代理模型比较简单，只需要将Meta中的proxy选项设置为True即可。假如，需要给Topic添加title的校验方法且查询记录按照id的顺序返回，那么，可以定义ProxyTopic：

```python
class ProxyTopic(Topic):
	"""
	代理topic
	"""
	class Meta:
		ordering = ['id']
		proxy = True
	def is_topic_valid(self):
		return 'django' in self.title
```

Django不会为ProxyTopic创建新的数据表，它和Topic共用post_topic表，只是在查询数据的返回上有不同的排序逻辑，且ProxyTopic实例多了一个is_topic_valid方法用来校验title是否包含django。

最后，需要注意，代理模型只能继承自一个非抽象的基类，并且不能同时继承多个非抽象基类。

### 1.4 Model的查询操作API

在应用中创建的每一个Model类，Django都会自动添加一个名为objects的Manager（django.db.models.Manager）对象，它是Model与数据库实现交互的入口，也被称作查询管理器。Django应用的每一个Model都至少会有一个管理器，且支持自定义管理器，但是鉴于默认的Manager功能非常强大，因此通常不必自己定义。

#### 1.4.1 创建Model实例对象

Django创建Model实例有两种方法，一种是直接调用Model的save方法，另一种是通过Manager的create方法。两种方法并没有本质的区别，都能够创建新的实例，但是，对于save方法，它还兼具了update实例的功能。

1.使用save方法创建Model实例

创建Model实例就是填充它的各个字段（有默认值或者允许为null的字段可以不填），之后调用它的save方法。

2.使用create方法创建Model实例

通常的业务场景中，根据给定的条件查询数据库记录是很普遍的。Manager提供了查询Model实例的接口，这些接口通常会返回三种类型：单实例、QuerySet和RawQuerySet。

#### 1.4.2 返回单实例的查询方法

查询单实例，Manager提供了get和get_or_create方法，它们的区别是：如果查询的实例不存在，get_or_create方法会创建新的实例对象。

1.使用get查询

假如，需要查询title是first topic的Topic对象，可以这样：

```python
Topic.objects.get(title='first topic')
```

当然，如果有多个查询条件，可以同时列出：

```python
Topic.objects.get(id=1, title='first topic')
```

get方法非常简单，它会按照给定的查询条件检索数据记录并返回单实例对象。

但需要注意的是，get方法会抛出两类异常。

DoesNotExist：给定的查询条件找不到对应的数据记录。

MultipleObjectsReturned：给定的查询条件匹配了多条数据记录。

所以，使用get方法要确保数据记录存在且只存在一条。通常，为了保证在get的时候不抛出异常，可以这样做：

```python
try:
    topic = Topic.objects.get(id=1, title='first topic')
except Topic.DoesNotExist:
    # do something
except Topic.MultipleObjectsReturned:
	# do something
```

特别地，Django支持在查询的时候使用pk来代替主键名称。这对于自定义了主键且主键名称不是id的Model来说，是非常方便而且不易出错的。例如，查询主键是1的Topic对象，可以这样写：

```python
Topic.objects.get(pk=1)
```

2.使用get_or_create

查询这个方法的查询过程与get类似，都需要传递查询参数，但是与get不同的是，它返回的是一个tuple对象，即(object,created)。其中第一个元素是实例对象，第二个元素是布尔值，标识返回的实例对象是否是新创建的。

例如，可以根据id和title查询Topic：

```python
Topic.objects.get_or_create(id=1,title='first topic')
```

需要注意的是，如果查询条件能够匹配多条数据记录，get_or_create方法也会抛出MultipleObjectsReturned异常。Manager提供的方法中不仅有get和get_or_create方法会返回单个Model实例，类似的还有first、last等方法，完整的接口定义可以查阅官方文档或阅读源码。

#### 1.4.3 返回QuerySet的查询方法

当需要返回多条数据记录时，就需要用到QuerySet对象。可以简单地把QuerySet理解为Model集合，它可以包含一个、多个或者零个Model实例。Manager提供了很多接口可以返回QuerySet对象，常用的有all、filter、exclude、reverse、order_by等方法。

QuerySet有个很重要的特性是惰性加载，即在真正使用它的时候才会去访问数据库检索记录。这样做的原因是QuerySet支持链式查询，如果每一次都去查询数据库，则很容易成为性能瓶颈。

1.使用all方法获取所有的数据记录

Manager提供的all方法可以获取Model的所有数据实例，例如，获取所有的Topic可以这样写：

```python
Topic.objects.all()
```

执行这条语句会打印所有的Topic记录，即会访问数据库获取结果。但是假如把这条语句赋值给一个变量，就像下面这样：

```python
topic_qs = Topic.objects.all()
```

2.使用reverse方法获取逆序数据记录

reverse方法也会获取全量的数据记录，这一点与all方法是相同的。但是，它返回的数据记录的顺序与all方法相反，即逆序查询。

3.使用order_by方法自定义排序规则

无论是all方法还是reverse方法，它们返回数据结果的排序规则都会受到BaseModel的影响，假如需要自定义排序规则，就需要使用order_by方法了。

order_by中可以指定多个排序字段，例如，对所有的Topic对象先按照title逆序排列，再按照created_time正序排列：

```python
Topic.objects.order_by('-title', 'create_time')
```

order_by方法在执行之前，会清除它之前的所有排序。对于传递给它的参数有两种情况比较特殊。

（1）不传递任何参数，那么查询时将不会有任何排序规则，默认的规则也不会有。

（2）传递问号：order_by('?')，这代表随机排序，这种查询可能会很慢且通常没有意义，所以很少使用。

4.使用filter方法过滤数据记录

通常对数据表的检索都只会获取全量数据的一个子集，即使用WHERE子句过滤不符合条件的记录。

filter方法完成的就是这样的功能，它会将传递的参数转换成WHERE子句实现过滤查询。在不传递任何参数的情况下，查询效果和all方法是一样的。

常用的查询关键字及含义如下,使用方法就是字段名后加上双下画线再加上关键字。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818152144.jpeg)

对于contains、startswith、endswith关键字，也都有对应的忽略大小写的查询版本，只需要在关键字之前加上字母i就可以了，例如icontains。

5.使用exclude方法反向过滤

exclude方法与filter方法实现的功能是很类似的，都用来对提供的条件进行过滤。实际上，它们确实都是调用了同一个方法，只是用一个布尔值标记自己是正向过滤（filter）还是反向过滤（exclude）。

exclude方法相当于在filter方法的前面加上一个NOT，即过滤出来的结果是不满足给定条件的数据记录。例如，想要获取up不小于29的Comment对象：

```python
Comment.objects.exclude(up__lt=29)
```

6.链式查询

由于filter、exclude这样的方法返回的结果是QuerySet，所以，在它们的后面可以继续调用filter、exclude等方法，这样就形成了链式查询。

例如，需要content中包含good、down不大于20且up大于20的Comment对象，可以使用链式查询：

```python
Comment.objects.filter(
	content__contains='good'
).exclude(
	down__gt=20	
).filter(
	up__gt=20
)
```

7.使用values方法获取字典结果

values方法同样会返回QuerySet，但是它不像all、filter等方法那样通过迭代可以获取Model的实例对象，它会返回字典，字典中的键对应Model的字段名。可以给values方法传递参数，用于限制SELECT的查询范围。如果没有指定，那么，查询结果将包含Model的所有字段。

例如，下面的查询语句只会查询Comment表的id和up字段：

```python
Comment.objects.values('up','down')
```

对于有些场景只需要表的部分字段，且不依赖Model实例的其他功能时，这种查询方法是非常高效的。特别是表中某一列或几列数据量很大，但又不会经常用到的情况下，values方法会是个不错的选择。

8.使用values_list方法获取元组结果

values_list方法与values方法非常相似，只是对返回结果迭代得到的是元组而不是字典。

同样，它也会按照传递的字段名限制SELECT的查询范围，且返回的元组元素顺序与传递的字段顺序相同。例如，通过values_list查询Comment表的id和up字段：

```python
Comment.objects.values_list('up','down')
```

如果没有给values_list传递任何字段参数，那么，将会返回Model中的所有字段，顺序为字段在模型中定义的顺序。

特别地，如果只传递了一个参数，可以配合使用flat参数，将它设置为True，那么迭代返回结果得到的将是字段值，例如，只查询Comment表的id字段：

```python
Comment.objects.values_list('id',flat=True)
```

这样的特性在只需要处理某个表的单列数据时会很方便。注意，如果不设置flat，其默认是False，则返回的结果就是只有单个元素的元组。

9.对QuerySet进行切片

Python可以通过切片操作获取序列类型对象的部分元素，用来限制返回的数据结果。这种语法同样适用于QuerySet，它在SQL上表现为LIMIT和OFFSET。例如，返回Comment表的前两个对象，可以这样写：

```python
Comment.objects.all()[:2]
```

在对QuerySet进行切片的时候需要注意以下几点。

（1）QuerySet不支持从末尾切片，即索引值不能是负数。

（2）对一个QuerySet执行切片操作会返回另一个QuerySet，并不会触发数据库的查询。但是，如果使用step切片语法，则Django会触发数据库查询，并且返回Model实例列表，例如：

```python
Comment.objects.all()[1:3:2]
```

（3）切片后返回的QuerySet不能再执行过滤或排序操作，Django认为这样的需求没有实际的意义。

对于Manager而言，除了all、filter、exclude等常用的方法会返回QuerySet之外，还有一些不常用的方法也会返回QuerySet对象，例如distinct、only、defer等方法，它们的使用场景不多。

#### 1.4.4 返回RawQuerySet的查询方法

Django的ORM非常强大，大部分业务场景都可以使用ORM语句完成。

但是，对于复杂的查询来说，ORM的表达能力可能达不到要求或者是书写起来不够灵活，那么可以使用SQL语句实现查询。

Manager提供了raw方法允许使用SQL语句实现对Model的查询，raw方法返回的是一个RawQuerySet对象，同样可以对它迭代得到Model实例对象。

但是，与QuerySet不同的是，不能在RawQuerySet上执行filter、exclude等方法。

1.简单的SQL查询

想要查询Model的所有实例对象，可以直接使用all方法。那么，如果想使用raw方法配合SQL语句实现同样的功能，可以这样写：

```python
for comment in Comment.objects.raw('SELECT * FROM post_comment'):
	print('%d: %s' % (comment.id, comment.content))
```

raw方法会自动将数据表字段映射到Model对应的字段上，但是需要注意，Django不会对传递给raw方法的SQL语句进行检查。

Django期望它会从数据库中返回一组行数据，但是并不做强制要求。如果查询的结果不是行数据，则会产生一个错误。

2.不要拼接SQL语句

很多场景下需要使用参数化查询，例如根据传递的title查询Topic对象。

这个时候，不能手动填充SQL字符串，因为这样存在SQL注入攻击的风险。

raw方法充分考虑到了这一点，提供了params参数来解决这个问题。

对于参数化查询的SQL来说，只需要在语句中加上%s或%(key)s之类的占位符，例如：

```python
for topic in Topic.objects.raw('SELECT id FROM post_topic WHERE title=%s', ['first topic']):
    print("%s: %s" % (topic.id, topic.title))
```

raw方法可以接受一个列表或字典类型（SQLite后端不支持）的参数，并将SQL语句中的占位符替换，最终完成查询。

传递给raw方法的SQL语句只是获取id这一个字段，但是仍然可以打印Topic的title字段。

这也是raw查询的一个特性，如果访问那些没有出现在SQL中的字段，Django会自动触发新的查询，获取对应的字段值。因此，对应于上面的print过程，其实是触发了两次数据库查询。

3.RawQuerySet支持索引和切片

虽然RawQuerySet不支持很多QuerySet的常见操作，但也可以对它进行索引和切片，例如：

```python
Comment.objects.raw('SELECT * FROM post_comment')[0]
Comment.objects.raw('SELECT * FROM post_comment')[1:2]
```

不过，这种操作其实是比较低效的。

raw方法仍然只是执行传递给它的SQL语句，将所有的数据从数据库中取出，之后在Python代码的层面上做索引和切片操作。

虽然Django提供了raw SQL的查询方法，但是它很少被用到，因为这需要考虑不同的数据库后端的不同特性。Django官方建议尽量使用常规的ORM完成查询。

#### 1.4.5 返回其他类型的查询方法

除了之前介绍的会返回单个Model实例和多个Model实例（通过对QuerySet或RawQuerySet迭代）的查询方法之外，Django还提供了一些非常有用的方法，它们会返回整数或者布尔类型的值

1.返回QuerySet的对象数量

通过all、filter、exclude方法获取了QuerySet对象之后，如果想要知道匹配的对象数量，可以通过len方法获取，例如：

```python
len(Comment.objects.filter(id__gt=1))
```

虽然能够得到结果，但是这样做的效率比较低，Django会从数据库中查询所有的数据记录，再去计算这个可迭代对象的长度。为了解决这个问题，QuerySet提供了count方法：

```python
Comment.objects.filter(id__gt=1).count()
```

count方法会在数据库上执行SELECT COUNT(*)操作并返回数字类型。所以，如果想获取实例对象的数量应该总是使用count方法。

2.判断QuerySet是否包含对象

有许多场景只需要根据当前给定的条件返回是否存在匹配实例对象的布尔结果，这也比较简单，只需要判断count方法的返回值是否为0就可以了。但是，这也需要两步操作才能完成，QuerySet提供了更加简单的方法，例如：

```python
Comment.objects.filter(id__gt=1).exists()
Comment.objects.filter(id__gt=10).exists()
```

如果QuerySet中包含Model对象，exists返回True；否则，它会返回False。应该总是使用exists方法校验给定的条件是否能够匹配到数据，因为，这个方法会将查询的代价最小化，在SQL上的表现为LIMIT 1。

3.使用update方法更新Model实例

在创建Model实例对象的时候介绍过可以使用save方法更新Model实例，即先查询出Model对象，之后更新字段值（更新主键会创建新的实例），再执行save方法就可以完成实例对象的更新，例如：

```python
comment = Comment.objects.get(id=1)
comment.up = 90
comment.save()
```

需要注意的是，虽然只是给up字段重新设定了值，但是save执行会更新Comment表的所有列。

如果只需要更新特定的列，那么可以使用QuerySet提供的update方法，例如针对up和down字段的修改利用update方法可以这样实现：

```python
Comment.objects.filter(id=1).update(up=90,down=33)
```

update方法可以一次性更新多个对象，并返回一个整数，标识受影响的记录条数。针对上面的例子，由于id为1的记录只有一条，所以，更新数据记录之后返回数字1。

4.使用delete方法删除Model实例

对于Model实例自身而言，可以调用它的delete方法完成删除操作，这样会删除一个Model实例。

同时，QuerySet也提供了delete方法，用来批量删除实例对象。其实，这两种删除数据的方式最终都会调用同样的方法，所以，它们的返回值也是一样的。

例如，需要删除id是1的Comment实例：

```python
comment = Comment.objects.get(id=1)
comment.delete()
```

需要删除id小于等于2的Topic实例：

```python
Topic.objects.filter(id__lte=2).delete()
```

delete方法返回一个二元组：第一个元素标识删除的实例个数，第二个元素是字典类型，记录每一个Model类型删除的实例个数。

对于第一个删除操作，只会删除Comment对象自身，所以，删除的实例个数就是1。

对于第二个删除操作，会删除两个Topic对象，但是由于Comment与Topic存在关联关系，且设置了级联删除，所以，一共会删除4个Model对象。

需要注意，对于第二个删除操作，由于存在外键的约束，因此在删除的顺序上是先删除Comment对象，再删除Topic对象。

#### 1.4.6 存在关联关系的查询

Django中存在三种关系模型用来维护表与表之间的关联，同时，Django也为此提供了非常强大的关联关系查询，自动在后台处理包含JOIN的SQL语句。

1.Model的反向查询

Django中的每一种关联关系都可以实现反向查询，例如Comment中定义了ForeignKey指向Topic，那么对于每一个Topic的实例对象都自动地会有一个管理器可以用来查询与它关联的Comment实例对象。

默认情况下，管理器的名称为“小写模型名_set”，对于Topic而言就是comment_set。之前介绍过可以通过related_name参数覆盖，但是通常不需要这样做。

```python
topic = Topic.objects.get(id=1)
topic.comment_set.all()
topic.comment_set.filter(content='very good!')
topic.comment_set.exclude(content__contains='good')
```

对于ManyToManyField和OneToOneField关系类型也可以实现类似的反向查询，但是对于OneToOneField类型的反向查询比较特殊。它的管理器代表的是一个单一的对象，而不是对象集合，且名称变成了小写的Model名。

2.跨关联关系查询

在查询Topic的时候可能会考虑Comment的情况，这是很普遍的场景，也被称作跨关联查询。

Django将这种查询形式设计得非常简单直接，只需要使用双下画线与关联Model的字段名称组合在一起，并给出合适的条件就可以完成查询。

例如，想要查询所有Topic的title字段包含first的Comment对象：

```python
Comment.objects.filter(topic__title__contains='first')
```

这里需要清楚，查询的是Comment对象，而不是Topic对象。这种查询是没有深度限制的，例如继续查询Topic关联的User：

```python
Comment.objects.filter(topic__user__username='admin')
```

Django也支持反向的关联查询，只需要使用关联Model的小写名称即可。例如要查询所有Com ment的up值大于等于30的Topic对象：

```python
Topic.objects.filter(comment__up__gte=30)
```

3.跨关联关系多值查询

之前看到的关联关系查询都是针对单个条件的，这种情况比较简单。如果关键字的参数有多个，且都是跨越外键或者多对多的情况，查询就会比较复杂。为了更好地描述这种查询，先给id是1的Topic对象添加一个Comment：

```python
Comment.objects.create(content='general', up=60, down=40, topic=topic)
```

执行第一个查询：

```python
Topic.objects.filter(comment__content__contains='very', comment__up__lte=60)
```

这个查询对应的是Comment中的content字段值包含very且up字段值小于等于60的所有的Topic对象，很显然，这样的Topic并不存在。

再执行第二个查询：

```python
Topic.objects.filter(comment__content__contains='very').filter(comment__up__lte=60)
```

对比第一个查询，可以发现第二个查询的条件是相同的，只是将两个条件分开，利用了两个filter。

这样的操作得到了不一样的结果。出现这样的效果的原因是：第一个filter过滤得到的QuerySet包含id是1的Topic对象；第二个filter在上一个结果QuerySet中过滤出up小于等于60的Comment的Topic对象，所以，最终的结果就是id是1的Topic。

从这两个例子中可以看出，跨关联关系的多值查询是很复杂的，不同的查询构造方式会出现不一样的结果。不仅仅是filter查询，exclude的多值查询也会表现得比较“奇怪”，在实际使用时需要小心谨慎。

#### 1.4.7 F对象和Q对象查询

Django提供了两个非常有用的工具：F对象和Q对象，方便了在一些特殊场景下的查询过程。

1.F对象查询

F对象用于操作数据库中某一列的值，它可以在没有实际访问数据库获取数据值的情况下对字段的值进行引用。

Django支持对F对象引用的字段使用加减乘除、取模、幂计算等算术操作，且运算符两边可以是具体的数值或者是另一个F对象引用的字段。

这在一些场景下非常有用，例如，Model中不同字段的比较作为过滤条件的情况、对Model的单个字段值进行更新，且这种更新建立在原字段值的基础上。

如果要查询up小于等于down的Comment，使用F对象查询可以这样实现：

```python
Comment.objects.filter(up__lte=F('down'))
```

可以看到，使用F对象将这种查询过程变得非常简单，否则，就需要将所有的Comment对象取出，迭代比较获取结果或者是使用raw SQL的方式。

如果要查询所有up值大于down值2倍的Comment对象，可以这样实现：

```python
Comment.objects.filter(up__gt=F('down') * 2)
```

F对象功能非常强大，除了基础字段值的引用之外，它也支持跨关联关系查询。例如，需要查询所有的Topic的content字段值包含title字段值的Comment对象：

```python
Comment.objects.filter(topic__content__contains=F('topic__title'))
```

假如一个Model对象的某一个字段值的更新需要依赖这个字段值，F对象也是非常有用的。

举一个例子说明这种情况：需要给id为1的Comment对象的up值加1，那么，更新的up值就会依赖原值，如果没有F对象，可能会使用下面的方法实现：

```python
comment = Comment.objects.get(id=1)
comment.up += 1
comment.save()
```

这个更新过程可能会出现竞争条件，导致结果不符合预期。

例如，有两个线程同时获取了同一个Comment对象，第一个线程对它的up字段值加1并保存，这不会出现问题。之后，第二个线程也对up字段值加1并保存，那么，第一个线程的工作就丢失了。

为了避免上述情况的发生，最好的办法是让数据库负责更新过程。这需要借助F对象提供的能力，实现过程如下：

```python
comment = Comment.objects.get(id=1)
comment.up = F('up')+1
comment.save()
```

这种更新过程与之前的方式都可以实现对字段值的更新，但是实际上F对象描述的是SQL表达式，而不是Python的运算。

2.Q对象查询

Q对象用于复杂查询，它把关键字参数封装在一起，并传递给filter、exclude、get等查询方法。多个Q对象可以使用“&”（与）、“|”（或）运算符组合，产生一个新的Q对象。可以使用“～”（非）运算符取反，即实现NOT查询。

最简单的使用Q对象查询的例子是单个关键字参数，例如，查询title中包含topic的所有Topic对象：

```python
Topic.objects.filter(Q(title__contains='topic'))
```

如果想要查询up大于60或down大于60的所有Comment对象，之前的查询方法不容易实现。但是，使用Q对象会使这一过程变得非常简单：

```python
Comment.objects.filter(Q(up__gt=60)|Q(down__gt=60))
```

可以看到，这里将两个Q对象使用运算符“|”组合起来，形成“或”的关系，再传递给filter方法实现查询。当然，也可以使用与、非的组合实现更加复杂的查询。

Q对象也可以与关键字参数组合在一起使用，但是在这种情况下，Django规定，Q对象必须放在前面。

#### 1.4.8 聚合查询和分组查询

聚合和分组都是用来生成统计值的过程：聚合是指对QuerySet整体（可以理解为Model对象的集合）生成一个统计值；分组是为QuerySet中每一个对象都生成一个统计值。

1.聚合查询

对QuerySet计算统计值，需要使用aggregate方法，提供的参数可以是一个或多个聚合函数。

aggregate是QuerySet的一个终止子句，它返回的是字典类型，键是聚合的标识，值是聚合的结果。

Django提供了一系列的聚合函数，其中Avg（平均值）、Count（计数）、Max（最大值）、Min（最小值）、Sum（加和）最为常用。

```python
Comment.objects.filter(topic=1).aggregate(Sum('up'))
```

首先得到id为1的Topic的Comment对象，之后，计算up值的加和。可以看到，字典结果的键名称是up__sum，这是Django根据字段名和聚合函数的名称自动拼接得到的。当然，也可以给聚合值指定一个名称，例如：

```python
Comment.objects.filter(topic=1).aggregate(up_s=Sum('up'))
```

也可以给aggregate传递多个聚合函数：

```python
Comment.objects.aggregate(l=Min('up'), h=Max('up'), m=Avg('up'))
```

2.分组查询

第二类统计是对QuerySet中的每一个Model对象都生成一个统计值，这可以通过annotate方法完成。annotate并不是结束子句，它返回的是一个QuerySet对象，所以，可以对结果进行“再加工”。

annotate方法的使用过程与aggregate是类似的，都需要传递聚合函数，来描述统计过程。

统计每一个Topic对应的Comment的数量，利用annotate可以这样实现：

```python
for topic in Topic.objects.annotate(Count('comment')):
    print('%d: %d' % (topic.id, topic.comment__count))
```

### 1.5 ORM原理

#### 1.5.1 Python元类

通常，类是用来生成对象的，在Python中也是成立的。但是，Python中的类又比较特殊，类同样是对象。当使用class关键字定义类的时候，Python解释器就自动创建了这个对象。既然类是对象，那么，它又是什么类型呢？Python中查看一个对象的类型可以使用type方法，例如：

可以看到，类A的类型是type，也就是说type创建了类A。在Python代码中使用type确定对象的类型是比较常见的，实际上，type还有一个重要的功能，就是创建类。

type接收三个参数用来创建类。

（1）类的名称，例如A。

（2）父类的元组，由于Python是支持多继承的，所以，如果只有一个父类，则应注意单元素元组的写法。

（3）属性字典。

使用type动态地创建类A，可以像下面这样：

```python
A = type('A', (), {})
print(A)
```

type的第一个参数传递了字符A作为类的名称；

第二个参数是一个空的元组，代表不继承任何类（Python3中所有的类都继承自object）；

第三个参数是一个空的字典，标识没有自定义属性。

type函数返回的就是类A，即可以用它去创建实例对象了。

Python在创建类对象（不是类实例对象）的时候，首先会从当前的类定义中查询是否指定了元类，如果没有，则继续在父类中寻找指定的元类。

如果在任何父类中都找不到元类的声明，就会上升到模块层次去查询。最终，如果还没有找到元类的声明，Python就会使用内置的type来创建这个类。

#### 1.5.2 Python描述符

简单地说，描述符就是实现了描述符协议的对象，描述符协议包含三个方法：__get__、__set__和__delete__。只要实现了这三个方法中的任意一个，这个类对象就被称作描述符，且这个类对象的实例就有了一些特殊的特性。

这三个方法的声明如下。

__get__(self,instance,owner)：用于访问属性，返回属性的值。

__set__(self,instance,value)：设置属性的值，不返回任何内容。

__delete__(self,instance)：用于删除属性，不返回任何内容。

只实现了__get__方法的对象称为非数据描述符，这类描述符只能读取对象属性；同时实现了__get__和__set__方法的对象是数据描述符，这类描述符可以实现对属性的读写。

#### 1.5.3 继承models.Model

Model元数据的集成与加工等都会由models.Model来完成（实际上是它的元类）。这样，Model就可以通过ORM API对数据库表实现增删改查了。

1.自动添加的自增主键

自定义的Model在没有设置主键的情况下，会由models.Model的元类自动添加名称为id的自增主键字段。

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164601.jpeg)

从源码中可以看到，元类的__new__方法在返回之前调用了_prepare()，它是在ModelBase中定义的方法。下面看一看这个方法做了些什么：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164623.jpeg)

继续看Options的_prepare（位于django/db/models/options.py文件中）方法：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164652.jpeg)

如果自定义的Model没有设定主键字段且不继承自非抽象的基类Model，那么，pk和parents字段都不会被设定（如Topic），会直接进入else条件。

首先，定义一个AutoField对象，然后调用ModelBase的add_to_class方法将它添加到类定义中，且属性名称为id。

所以，自定义的Model不需要显式地指定主键，元类在创建类对象的时候会动态检查并决定是否添加。

2.自动添加的查询管理器

Django会为每一个Model自动添加一个名称为objects的查询管理器对象，它的实现也是定义在ModelBase的_prepare方法中：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164746.jpeg)

从源码中可以看到，使用了add_to_class方法将Manager对象添加到类定义中。下面看一看add_to_class的实现：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164802.jpeg)

继续看Manager的contribute_to_class（位于django/db/models/manager.py文件中）方法：

![img](https://gitee.com/cao_ziqiang/img/raw/master/20210818164818.jpeg)

到这里，终于可以知道，Model.objects并不是一个Manager实例，而是一个ManagerDescriptor实例。

ManagerDescriptor其实是一个描述符，是对Manager访问的一种保护，其主要目的就是实现对Model实例和抽象Model的拒绝访问。

这样，继承自models.Model的类就自动拥有了主键（id）和查询管理器（objects），这些已经能够满足大多数场景的需要了。

Django通过元类实现它们，在创建自定义Model对象的时候这些就已经完成了，这是非常优雅的做法，也是非常值得学习和借鉴的。



