# PL/SQL

## 一、PL/SQL概述

### 1.1 PL/SQL特点

- 与SQL语言紧密集成。
- 减小网络流量，提高应用程序的运行性能。
- <img src="img/image-20221029210702977.png" alt="image-20221029210702977" style="zoom: 50%;" />
- 模块化的程序设计功能，提高了系统可靠性。
- 服务器端程序设计，可移植性好

### 1.2 PL/SQL执行过程

PL/SQL是先通过过程化语句执行器，再将块中的SQL语句给SQL执行器执行

SQL是直接将SQL语句给SQL执行器执行

## 二、PL/SQL基础

### 2.1 PL/SQL块的组成

PL/SQL程序的基本单元是语句块，所有的PL/SQL程序都是由语句块构成的 。

一个完整的PL/SQL语句块由3个部分组成。 

```SQL
declare
声明部分
主要用于声明变量、常量、数据类型、游标、异常处理名称以及本地（局部）子程序定义等。 
begin
可执行部分
该部分通过变量赋值、流程控制、数据查询、数据操纵、数据定义、事务控制、游标处理等实现块的功能。
exception
异常处理部分
异常处理部分用于处理该块执行过程中产生的异常。 
end;
```

- 执行部分是必须的，而声明部分和异常部分是可选的
- 可以在一个块的执行部分或异常处理部分嵌套其他的PL/SQL块；
- 所有的PL/SQL块都是以END结束。

```sql
DECLARE
  v_ename VARCHAR2(10);
BEGIN
  SELECT ename INTO v_ename FROM emp 
  WHERE empno=7844;
  DBMS_OUTPUT.PUT_LINE(v_ename);
EXCEPTION
  WHEN NO_DATA_FOUND THEN
     DBMS_OUTPUT.PUT_LINE('There is not such a  employee');
END;
```

### 2.2 注释

```sql
单行注释
--
多行注释
/*   */
```

### 2.3 数据类型

| 分类          | 数据类型                                |
| ------------- | --------------------------------------- |
| 数字类型      | NUMBER、BINARY_NUMBER  PLS_NUMBER       |
| 字符类型      | VARCHAR2、CHAR、LONG、  NCHAR、NVARCHAR |
| 日期/区间类型 | DATE、TIMESTAMP、INTERVAL               |
| 行标识类型    | ROWID、UROWID                           |
| 布尔类型      | BOOLEAN（TRUE、FALSE、NULL）            |
| 原始类型      | RAW、LONG  RAW                          |
| LOB类型       | CLOB、BLOB、NCLOB、BFILE                |
| 引用类型      | REF  CURSOR，REF  object_type。         |
| 记录类型      | RECORD                                  |
| 集合类型      | TABLE，VARRAY                           |

#### 2.3.1 数字类型 

NUMBER类型以十进制形式存储整数和浮点数，语法为NUMBER（p，s）。其中，p为精度，即所有有效数字位数；s为刻度范围，即小数位数。p的取值范围为1～38。
BINARY_INTEGER类型用于表示从-2147483647～+2147483647之间的整数，以二进制形式存储。当发生溢出时，将自动转换成NUMBER类型。
PLS_INTEGER类型表示范围与BINARY_INTEGER相同，但发生溢出时会产生错误。

#### 2.3.2 字符类型 

PL/SQL中的字符类型与Oracle数据库中的字符类型类似，但是允许字符串的长度有所不同。
VARCHAR2，CHAR主要用于存储来自本地数据库字符集的字符，而NCHAR，NVARCHAR2 用于存储来自国家字符集的字符串。 

| 类  型    | PL/SQL中最大字节数 | Oracle中最大字节数 |
| --------- | ------------------ | ------------------ |
| VARCHAR2  | 32767              | 4000               |
| NVARCHAR2 | 32767              | 4000               |
| CHAR      | 32767              | 2000               |
| NCHAR     | 32767              | 2000               |
| LONG      | 32760              | 2GB                |

#### 2.3.3 日期/区间类型 

- DATE：与数据库中的DATE类型相同，存储日期和时间信息，包括世纪、年、月、日、小时、分和秒，不包括秒的小数部分。

- TIMESTAMP：与DATE小数部分，如 timestamp(3)类型相似，但包括秒的

- 
  INTERVAL：用于存储两个时间戳之间的时间间隔，有下面两种形式。

  - INTERVAL YEAR [(p)]TO MONTH：两个时间戳相差的年数和月数。

  - INTERVAL DAY[(dp)] TO SECOND[(sp)]：两个时间戳相差的天数和秒数。

#### 2.3.4 行标识类型

- ROWID表示行的物理地址
- UROWID既可以表示行的物理地址，也可以表示行的逻辑地址。

#### 2.3.5 布尔类型（BOOLEAN）

只能在PL/SQL中使用，其取值为逻辑值，包括TRUE、FALSE、NULL

#### 2.3.6 LOB类型

- 包括BLOB，CLOB，NCLOB和BFILE四种类型。其中BLOB存放二进制数据，CLOB，NCLOB存放文本数据，而BFILE存放指向操作系统文件的指针。

- LOB类型变量可以存储4 GB的数据量。

#### 2.3.7引用类型

引用类型类似于其他高级语言中的指针类型。在PL/SQL中，引用类型包括游标的引用类型和对象的引用类型，即REF CURSOR和REF 

object_type。

#### 2.3.8 记录类型

记录类型是复合类型，类似于C语言中的结构体，是一个包含若干个成员分量的复合类型。

在使用记录类型时，需要先在声明部分定义记录类型和记录类型的变量，然后在执行部分引用该记录类型变量或其成员分量。

#### 2.3.9 集合类型

集合类型是复合类型，包括索引表类型、嵌套表类型和可变数组类型。

集合类型与记录类型的区别在于，记录类型中的成员分量可以是不同类型的，类似于结构体，而集合类型中所有的成员分量必须具有相同的数据类型，类似于数组。 

#### 2.3.10 %TYPE与%ROWTYPE

- 如果要定义一个类型与某个变量的数据类型或数据库表中某个列的数据类型一致（不知道该变量或列的数据类型）的变量，可以利用%TYPE来实现。
- 如果要定义一个与数据库中某个表结构一致的记录类型的变量，可以使用%ROWTYPE来实现。 

### 2.4 变量与常量的定义

#### 2.4.1 变量定义的一般格式

```SQL
变量名 [CONSTANT] 数据类型 [DEFAULT|:=表达式;
```

说明

- 变量或常量名称是一个PL/SQL标识符，应符合标识符命名规范；
- 每行只能定义一个变量；
- 如果加上关键字CONSTANT，则表示所定义的是一个常量，必须为它赋初值；
- 使用DEFAULT或“:=”运算符为变量初始化。

#### 2.4.2 变量的作用域

- 变量的作用域是指变量的有效作用范围，从变量声明开始，直到块结束。
- 如果PL/SQL块相互嵌套，则在内部块中声明的变量是局部的，只能在内部块中引用，而在外部块中声明的变量是全局的，既可以在外部块中引用，也可以在内部块中引用。
- 如果内部块与外部块中定义了同名变量，则在内部块中引用外部块的全局变量时需要使用外部块名进行标识。 

```sql
<<OUTER>>
DECLARE
  v_ename  CHAR(20);
  v_outer  NUMBER(5);
BEGIN
  v_outer :=10;
  DECLARE
    v_ename CHAR(20); 
    v_inner DATE;
  BEGIN
    v_inner:=sysdate;
    v_ename:='INNER V_ENAME';
    OUTER.v_ename:='OUTER V_ENAME';
  END;
  DBMS_OUTPUT.PUT_LINE(v_ename);
END; 
```

### 2.5 PL/SQL记录 

定义记录类型的语法为

```SQL
TYPE 记录名 IS RECORD(
    属性1 类型1 [NOT NULL][DEFAULT|:=expr1],
    属性2 类型2 [NOT NULL][DEFAULT|:=expr2],
    ……
);
```

注意：
相同记录类型的变量可以相互赋值；
不同记录类型的变量，即使成员完全相同也不能相互赋值；

```sql
-- 利用记录类型以及记录类型变量，保存员工信息。 
DECLARE
  TYPE t_emp IS RECORD(
     empno NUMBER(4), 
     ename CHAR(10),
     sal   NUMBER(6,2)
  );
  v_emp t_emp;
BEGIN
  SELECT empno,ename,sal INTO v_emp 
  FROM emp WHERE empno=7844;
  DBMS_OUTPUT.PUT_LINE(v_emp.ename||' '||v_emp.sal);
END;

DECLARE
  v_emp emp%ROWTYPE; 
BEGIN
  SELECT * INTO v_emp FROM emp
  WHERE empno=7844;
  DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '|| v_emp.ename||' '||v_emp.sal);  
END;
```

记录类型变量中分量的个数、顺序、类型应该与查询列表中列的个数、顺序、类型完全匹配。 

### 2.6 PL/SQL中SQL语句

由于PL/SQL执行采用早期绑定，即在编译阶段对变量进行绑定，识别程序中标识符的位置，检查用户权限、数据库对象等信息，因此在

PL/SQL中只允许出现: 

- SELECT 
- DML(UPDATE、DELETE、INSERT)
- 事务控制语句（COMMIT、ROLLBACK、SAVEPOINT）

注意：DDL语句不可以直接使用

通常，利用SQL语句对数据库进行操作时，各种相关量都在代码中以常量的形式指定，而在PL/SQL中可以通过变量动态指定各种相关量的

值，从而实现对数据库的动态操作。

```sql
DECLARE 
  v_empno NUMBER(4);
BEGIN
  v_empno:=&x;
  UPDATE emp SET sal=sal+100 WHERE empno=v_empno;
END
```

<img src="img\image-20221029215513164.png" alt="image-20221029215513164" style="zoom: 50%;" />

如果要查询当前DML语句操作的记录的信息，可以在DML语句末尾使用**RETURNING**语句返回该记录的信息。

```SQL
-- 返回修改之后的工资
DECLARE
   v_sal emp.sal%TYPE;
BEGIN
   UPDATE emp SET sal=sal+100 WHERE empno=7844 
                                            RETURNING sal INTO v_sal;
   DBMS_OUTPUT.PUT_LINE(v_sal);
END;
```

## 三、控制结构

### 3.1 选择结构

#### 3.1.1 if语句

```plsql
if 情况1 then 语句1;
[elsif 情况2 THEN 语句2;]
……
[else else_statements];
end if; 
```

例如，输入一个员工号，修改该员工的工资，如果该员工为10号部门，工资增加100；若为20号部门，工资增加90；若为30号部门，工资增加200；否则增加300。 

```plsql
DECLARE
  v_deptno emp.deptno%type;
  v_increment NUMBER(4);
  v_empno  emp.empno%type;
BEGIN
  v_empno:=&x;
  SELECT deptno INTO v_deptno FROM emp 
  WHERE empno=v_empno;
  IF v_deptno=10 THEN v_increment:=1000;
  ELSIF v_deptno=20 THEN v_increment:=900;
  ELSIF v_deptno=30 THEN v_increment:=2000;
  ELSE  v_increment:=3000;
  END IF;
  UPDATE emp SET sal=sal+v_increment 
  WHERE empno=v_empno;
END;
```

#### 3.1.2 CASE语句

```plsql
CASE
      WHEN condition1 THEN statements1;
      WHEN condition2 THEN statements2;
      ……
      WHEN conditionn THEN statementsn;
      [ELSE    else_statements;]
END CASE;
```

根据输入的员工号，修改该员工工资。如果该员工工资低于1000，则工资增加200；如果工资在1000～2000之间，则增加150；如果工资在2000～3000之间，则增加100；否则增加50。 

```plsql
DECLARE
v_sal emp.sal%type;
v_increment NUMBER(4);
v_empno  emp.empno%type;
BEGIN
v_empno:=&x;
SELECT sal INTO v_sal FROM emp 
      WHERE empno=v_empno;
CASE   
     WHEN v_sal<1000 THEN v_increment:=200;
     WHEN v_sal<2000 THEN v_increment:=150;
     WHEN v_sal<3000 THEN v_increment:=100;
     ELSE  v_increment:=50;
  END CASE;
UPDATE emp SET sal=sal+v_increment 
      WHERE empno=v_empno;
END; 
```

**等值比较的CASE语句基本语法**

```plsql
CASE test_value
    WHEN value1 THEN statements1;
    WHEN value2 THEN statements2;
    ……
    WHEN valuen THEN statementsn;
    [ELSE    else_ statements;]
END CASE; 
```

```plsql
DECLARE
   v_deptno emp.deptno%type;
   v_increment NUMBER(4);
   v_empno  emp.empno%type;
BEGIN
   v_empno:=&x;
  SELECT deptno INTO v_deptno FROM emp 
        WHERE empno=v_empno;
  CASE v_deptno  
    WHEN 10 THEN v_increment:=100;
    WHEN 20 THEN v_increment:=150;
    WHEN 30 THEN v_increment:=200;
    ELSE  v_increment:=300;
  END CASE;
    UPDATE emp SET sal=sal+v_increment 
        WHERE empno=v_empno; 
END; 
```

### 3.2 循环结构

#### 3.2.1 简单循环

```plsql
 LOOP
       循环语句;
       EXIT [WHEN condition] ;
END LOOP;
在循环体中一定要包含EXIT语句，否则程序进入死循环 
```

利用循环向temp_table表中插入50条记录。

```plsql
DECLARE
  v_counter BINARY_INTEGER := 1;
BEGIN
  LOOP
    INSERT INTO temp_table VALUES (v_counter);
    v_counter := v_counter + 1;
    EXIT WHEN v_counter > 50;
  END LOOP;
END; 
```

#### 3.1.2 WHILE循环

```plsql
WHILE condition LOOP 
        循环语句;
END LOOP; 
```

利用WHILE循环向temp_table表中插入50条记录。

```plsql
DECLARE
  v_counter BINARY_INTEGER :=1;
BEGIN
  WHILE v_counter <= 50 LOOP
     INSERT INTO temp_table VALUES (v_counter);
     v_counter := v_counter + 1;
  END LOOP;
END; 
```

#### 3.1.3 FOR循环

```plsql
FOR loop_counter IN [REVERSE] 最小值..最大值 LOOP
    循环语句;
END LOOP;
```

- 循环变量不需要显式定义，系统隐含地将它声明为BINARY_INTEGER变量；
- 系统默认时，循环变量从下界往上界递增计数，如果使用REVERSE关键字，则表示循环变量从上界向下界递减计数；
- 循环变量只能在循环体中使用，不能在循环体外使用

利用FOR循环向temp_table表中插入50条记录。

```plsql
BEGIN
  FOR v_counter IN 1..50 LOOP
    INSERT INTO temp_table VALUES (v_counter);
  END LOOP;
END;
```

### 3.3 跳转结构

```plsql
《标号》
…
GOTO 标号；
```

- 块内可以跳转，内层块可以跳到外层块，但外层块不能跳到内层。
- IF语句不能跳入。不能从循环体外跳入循环体内。不能从子程序外部跳到子程序中。
- 由于goto语句的缺点，建议尽量少用甚至不用goto语句。 

利用跳转结构向temp_table表中插入50条记录。

```plsql
DECLARE
   v_counter BINARY_INTEGER :=1;
BEGIN
   <<LABEL>>
   INSERT INTO temp_table VALUES (v_counter);
   v_counter := v_Counter + 1;
   IF v_counter<=50 THEN
         GOTO  LABEL;
   END IF;
END; 
```

## 四、游标

### 4.1 游标的概念

- 游标（CURSOR）是Oracle系统在内存中开辟的一个工作区，在其中存放SELECT语句返回的查询结果。
- 使用游标时，SELECT语句查询的结果可以是单条记录，多条记录，也可以是零条记录。
- 游标工作区中，存在着一个指针（POINTER）,在初始状态它指向查询结果的首记录。

### 4.2 游标的类型

- 显式游标：由用户定义、操作，用于处理返回多行数据的SELECT查询。
- 隐式游标：由系统自动进行操作，用于处理DML语句和返回单行数据的SELECT查询。        

### 4.3 显式游标  

#### 4.3.1 显式游标的操作

定义游标、打开游标、检索游标、关闭游标

<img src="img\image-20221029231007058.png" alt="image-20221029231007058" style="zoom:50%;" />

根据输入的部门号查询某个部门的员工信息，部门号在程序运行时指定。

```plsql
DECLARE
  v_deptno emp.deptno%TYPE;
  CURSOR c_emp IS SELECT * FROM emp WHERE deptno=v_deptno;
  v_emp c_emp%ROWTYPE;
BEGIN
  v_deptno:=&x;
  OPEN c_emp;
  LOOP
     FETCH c_emp INTO v_emp;
     EXIT WHEN c_emp%NOTFOUND;
     DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||
                                           v_emp.ename||' '|| 
                                           v_emp.sal ||'  '|| 
                                           v_deptno);                                   
   END LOOP; 
   CLOSE c_emp;
END; 
```

##### 定义游标

```plsql
CURSOR cursor_name IS select_statement ;
```

- 游标必须在PL/SQL块的声明部分进行定义；
- 游标定义时可以引用PL/SQL变量，但变量必须在游标定义之前定义；
- 定义游标时并没有生成数据，只是将定义信息保存到数据字典中；
- 游标定义后，可以使用cursor_name%ROWTYPE定义游标类型变量。

##### 打开游标

```plsql
OPEN cursor_name; 
```

- 检查变量的值
- 执行游标定义时对应的SELECT语句，将查询结果检索到工作区中。
- 游标指针指向第一个元组
- 一旦游标打开，就无法再次打开，除非先关闭
- 如果游标定义中的变量值发生变化，则只能在下次打开游标时才起作用。

##### 检索游标

```plsql
FETCH cursor_name INTO variable_list|record_variable; 
```

- 在使用FETCH语句之前必须先打开游标
- 对游标第一次使用FETCH语句时，游标指针指向第一条记录，因此操作的对象是第一条记录，使用后，游标指针指向下一条记录。
- 游标指针只能向下移动，不能回退
- INTO子句中的变量个数、顺序、数据类型必须与工作区中每行记录的字段数、顺序以及数据类型一一对应。

##### 关闭游标

```plsql
CLOSE cursor_name; 
```

游标所对应的内存工作区变为无效，释放与游标相关的系统资源。

#### 4.3.2 显式游标的属性 

- %ISOPEN：布尔型。如果游标已经打开，返回TRUE,否则为FALSE。
- %FOUND：布尔型，如果最近一次使用FETCH语句，有返回结果则为TRUE,否则为FALSE;
- %NOTFOUND：布尔型，如果最近一次使用FETCH语句,没有返回结果则为TRUE,否则为FALSE;
- %ROWCOUNT：数值型，返回到目前为止从游标缓冲区检索的元组数。
- %BULK_ROWCOUNT（i）：数值型，用于取得FORALL语句执行批绑定操作时第i个元素所影响的行数。

#### 4.3.3 参数化显式游标

```plsql
-- 参数化游标定义语法格式
CURSOR cursor_name(参数1 类型[,参数2 类型…]) IS select语句 
-- 打开参数化游标的方法
OPEN cursor_name(参数1 类型[,参数2 类型…])
```

- 定义参数化游标时，只能指定参数的类型，而不能指定参数的长度、精度、刻度；
- 打开带参数的游标时，实参的个数和数据类型等必须与游标定义时形参个数和数据类型等相匹配。

```plsql
DECLARE
  CURSOR c_emp(p_deptno emp.deptno%TYPE)IS 
  SELECT * FROM emp WHERE deptno=p_deptno;
  v_emp c_emp%ROWTYPE;

BEGIN
  OPEN c_emp(10);
  LOOP
    FETCH c_emp INTO v_emp;
    EXIT WHEN c_emp%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||v_emp.ename);    
  END LOOP;  
  CLOSE c_emp;
  OPEN c_emp(20);
  LOOP
    FETCH c_emp INTO v_emp;
    EXIT WHEN c_emp%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||v_emp.ename);    
  END LOOP;  
  CLOSE c_emp;
END;
```

#### 4.3.4 显式游标的检索

##### 利用简单循环检索游标

```plsql
DECLARE
   CURSOR cursor_name IS SELECT语句;//定义游标
BEGIN
   OPEN cursor_name;//打开游标
   LOOP
      FETCH…INTO…;//检索游标
   EXIT WHEN cursor_name%NOTFOUND;
……
   END LOOP;
   CLOSE cursor_name;//关闭游标
END; 
```

**EXIT WHEN子句应该是FETCH…INTO语句的下一条语句。** 

利用简单循环统计并输出各个部门的平均工资。

```plsql
DECLARE
  CURSOR c_dept_stat IS SELECT deptno,avg(sal) avgsal 
          FROM emp GROUP BY deptno;//定义游标
  v_dept c_dept_stat%ROWTYPE;
BEGIN
OPEN c_dept_stat;//打开游标
LOOP
    FETCH c_dept_stat INTO v_dept;//检索游标
    EXIT WHEN c_dept_stat%NOTFOUND;
    DBMS_OUTPUT.PUT_LINE(v_dept.deptno||' '||v_dept.avgsal);
  END LOOP;
  CLOSE c_dept_stat;//关闭游标
END;
```

##### 利用WHILE循环检索游标

```plsql
DECLARE
CURSOR cursor_name IS SELECT…;
BEGIN
OPEN cursor_name;
FETCH…INTO…;
WHILE cursor_name%FOUND LOOP
FETCH…INTO…;
……
END LOOP;
CLOSE cursor;
END;
注意：在循环体外进行一次FETCH操作，作为第一次循环的条件。
```

利用WHILE循环统计并输出各个部门的平均工资。

```plsql
DECLARE
  CURSOR c_dept_stat IS SELECT deptno,avg(sal) avgsal 
          FROM emp GROUP BY deptno;//定义游标
  v_dept c_dept_stat%ROWTYPE;
BEGIN
  OPEN c_dept_stat;//打开游标
  FETCH c_dept_stat INTO v_dept; 
  WHILE c_dept_stat%FOUND LOOP
    DBMS_OUTPUT.PUT_LINE(v_dept.deptno||' '||  
                                           v_dept.avgsal);
    FETCH c_dept_stat INTO v_dept;//检索游标
  END LOOP;
  CLOSE c_dept_stat;//关闭游标
END; 

```

##### 利用FOR循环检索游标

```plsql
DECLARE
	CURSOR cursor_name IS SELECT…;
BEGIN
    FOR loop_variable IN cursor_name LOOP
      ……
    END LOOP;
END; 
```

利用FOR循环统计并输出各个部门的平均工资。

```plsql
DECLARE
  CURSOR c_dept_stat IS SELECT deptno,avg(sal) 
              avgsal FROM emp GROUP BY deptno;
BEGIN
  FOR v_dept IN c_dept_stat LOOP
    DBMS_OUTPUT.PUT_LINE(v_dept.deptno||' '||
                                          v_dept.avgsal);
  END LOOP; 
END; 
```

#### 4.3.5利用游标更新或删除数据

```plsql
CURSOR cursor_name IS
SELECT select_list_item FROM table  FOR UPDATE
[OF column_reference] [NOWAIT]; 
```

注意

> 打开游标时对相应的表加锁（通常SELECT操作不在数据上设置任何锁），其他用户不能对该表进行DML操作；
> 若数据对象已经被其他会话加锁，则当前会话挂起等待（默认状态），若指定了NOWAIT子句，则不等待，返回ORACLE错误。
> 对于多表查询时，可以通过OF子句指定某个要加锁的表的列的形式，对特定的表加锁，而其他表不加锁；否则所有表都加锁。 
> 当用户执行COMMIT或ROLLBACK操作时，数据上的锁会自动被释放。

更新或修改数据的语法为

```plsql
UPDATE|DELETE…
WHERE CURRENT OF cursor_name 
```

> 注意
> 如果游标定义时没有使用FOR UPDATE子句，则不能利用该游标修改或删除数据库中的数据。

修改员工的工资，如果员工的部门号为10，则工资提高100；如果部门号为20，则工资提高150；如果部门号为30，则工资提高200；否则工资提高250。 

```plsql
DECLARE
  CURSOR c_emp IS SELECT * FROM emp FOR UPDATE;
  v_increment NUMBER;
BEGIN
  FOR v_emp IN c_emp LOOP
    CASE v_emp.deptno 
      WHEN 10 THEN v_increment:=100;
      WHEN 20 THEN v_increment:=150;
      WHEN 30 THEN v_increment:=200;
      ELSE         v_increment:=250;
    END CASE;
    UPDATE emp SET sal=sal+v_increment
                        WHERE CURRENT OF c_emp;
  END LOOP;
COMMIT;
END; 
```

### 4.4 隐式游标

**概念**

- 所有的SQL语句都有一个执行的缓冲区，隐式游标就是指向该缓冲区的指针，由系统隐含地打开、处理和关闭。隐式游标又称为SQL游标。
- 隐式游标主要用于处理INSERT、UPDATE，DELETE以及单行的SELECT…INTO语句，没有OPEN，FETCH，CLOSE等操作命令。

- **属性**
  SQL%ISOPEN：布尔型值，判断隐式游标是否已经打开。对用户而言，该属性值始终为FALSE，因为操作时系统自动打开，操作完后立即自动关闭。
- SQL%FOUND：布尔型值，判断当前的操作是否会对数据库产生影响。如果有数据的插入、删除、修改或查询到数据，则返回TRUE，否则返回FALSE。
- SQL%NOTFOUND：布尔型值，判断当前的操作是否对数据库产生影响。如果没有数据的插入、删除、修改或没有查询到数据，则返回TRUE，否则返回FALSE。
- SQL%ROWCOUNT：数值型，返回当前操作所涉及的数据库中的行数。

修改员工号为1000的员工工资，将其工资增加100。如果该员工不存在，则向emp表中插入一个员工号为1000，工资为900的员工。

```plsql
BEGIN
    UPDATE emp SET sal=sal+100 WHERE empno=1000;
    IF SQL%NOTFOUND THEN
       INSERT INTO emp(empno,sal) VALUES(1000,900);
    END IF;
END;
或
BEGIN
    UPDATE emp SET sal=sal+100 WHERE empno=1000;
    IF SQL%ROWCOUNT=0 THEN
       INSERT INTO emp(empno,sal) VALUES(1000,900);
    END IF;
END;
```

### 4.5 游标变量

游标变量是一个指向多行查询结果集的指针，不与特定的查询绑定，因此具有非常大的灵活性，可以在打开游标变量时定义查询，可以返

回不同结构的结果集 

 **定义游标引用类型及游标变量** 

```plsql
TYPE ref_cursor_type_name IS REF CURSOR [RETURN return_type]
```

RETURN子句用于指定定义的游标类型返回结果集的类型，**该类型必须是记录类型**。如果定义游标引用类型时带有RETURN子句，则用其定义的变量称为强游标变量，否则称为弱游标变量。

在Oracle 中，系统预定义了一个游标引用类型，称为SYS_REFCURSOR，可以直接使用它定义游标变量。 

```plsql
ref_cursor_type_name variable_name;
例如
TYPE emp_cursor_type IS REF CURSOR 
               RETURN emp%ROWTYPE;
TYPE general_cursor_type IS REF CURSOR;
v_emp emp_cursor_type;
v_general general_cursor_type;
my_cursor SYS_REFCURSOR;
```

**打开游标变量**

```plsql
OPEN cursor_variable FOR select_statement;
注意
如果打开的游标变量是强游标变量，则查询语句的返回类型必须与游标引用类型定义中RETURN子句指定的返回类型相匹配。
例如
OPEN v_emp FOR SELECT * FROM emp;
OPEN v_general FOR SELECT 
                     empno,ename,sal,deptno FROM emp;
OPEN my_cursor FOR SELECT * FROM dept;
```

**检索游标变量**

```plsql
LOOP
FETCH cursor_variable INTO variable1, variable2, …;
EXIT WHEN cursor_variable%NOTFOUND;
……
END LOOP; 
注意
检索游标变量时只能使用简单循环或WHILE循环，不能采用FOR循环。
```

**关闭游标变量**

```plsql
CLOSE cursor_variable; 
```

要求根据输入的不同表名进行不同处理，若表名为emp，则显示高于10号部门平均工资的员工信息；若表名为dept，则显示各个部门的人数。 

```plsql
DECLARE
  v_table CHAR(20);
  TYPE type_cursor IS REF CURSOR;
  v_cursor type_cursor;
  v_emp emp%ROWTYPE;
  v_deptno emp.deptno%TYPE;
  v_num NUMBER;
BEGIN
  v_table:='&table_name';
  IF v_table = 'emp' THEN
    OPEN v_cursor FOR SELECT * FROM emp WHERE sal>(SELECT AVG(sal) FROM emp WHERE deptno=10);
  ELSIF v_table = 'dept' THEN
    OPEN v_cursor FOR SELECT deptno,count(*) num 
              FROM emp GROUP BY deptno;
  ELSE
    RAISE_APPLICATION_ERROR(-20000,'Input must be “emp” or “dept”');
 END IF; 
 LOOP
    IF v_table = 'emp' THEN
      FETCH v_cursor INTO v_emp;
      EXIT WHEN v_cursor%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||  
                           v_emp.ename||' '|| 
                           v_emp.sal||' '||
                           v_emp.deptno);
    ELSE
      FETCH v_cursor INTO v_deptno,v_num;
      EXIT WHEN v_cursor%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE(v_deptno||' '||v_num);
    END IF;
  END LOOP;
  CLOSE v_cursor;
END; 
```

修改7844员工的工资，保证修改后工资不超过6000。

```plsql
DECLARE
  e_highlimit EXCEPTION;
  v_sal emp.sal%TYPE;
BEGIN
  UPDATE emp SET sal=sal+100 WHERE empno=7844 RETURNING sal INTO v_sal;
  IF v_sal>6000 THEN 
     RAISE e_highlimit;
  END IF;
EXCEPTION
  WHEN e_highlimit THEN
    DBMS_OUTPUT.PUT_LINE('The salary is too large!');
    ROLLBACK;
END; 
```

## 五、异常处理

### 5.1 Oracle错误处理机制

Oracle中对运行时错误的处理采用了异常处理机制。

一个错误对应一个异常，当错误产生时抛出相应的异常，并被异常处理器捕获，程序控制权传递给异常处理器，由异常处理器来处理运行时错误。 

### 5.2 异常的类型

#### 5.2.1 预定义的Oracle异常

当Oracle错误产生时，与错误对应的预定义异常被自动抛出，通过捕获该异常可以对错误进行处理。

常用预定义异常包括：

| 异常情况名              | 错误代码  | 描述                                                       |
| ----------------------- | --------- | ---------------------------------------------------------- |
| CURSOR_ALREADY_OPEN     | ORA-06511 | 尝试打开已经打开的游标                                     |
| INVALID_CURSOR          | ORA-01001 | 不合法的游标操作（如要打开已经关闭的游标）                 |
| NO_DATA_FOUND           | ORA-01403 | 没有发现数据                                               |
| TOO_MANY_ROWS           | ORA-01422 | 一个SELECT INTO语句匹配多个数据行                          |
| INVALID_NUMBER          | ORA-01722 | 转换成数字失败  （‘X’）                                    |
| VALUE_ERROR             | ORA-06502 | 截断、算法或转换错误，通常出现在赋值错误                   |
| ZERO_DIVIDE             | ORA-01476 | 除数为0                                                    |
| ROWTYPE_MISMATCH        | ORA-06504 | 主机游标变量与PL/SQL游标变量类型不匹配                     |
| DUP_VAL_ON_INDEX        | ORA-00001 | 违反唯一性约束或主键约束                                   |
| SYS_INVALID_ROWID       | ORA-01410 | 转换成ROWID失败                                            |
| TIMEOUT_ON_RESOURCE     | ORA-00051 | 在等待资源中出现超时                                       |
| LOGIN_DENIED            | ORA-01017 | 无效用户名/密码                                            |
| CASE_NOT_FOUND          | ORA-06592 | 没有匹配的WHEN子句                                         |
| NOT_LOGGED_ON           | ORA-01012 | 没有与数据库建立连接                                       |
| STORAGE_ERROR           | ORA-06500 | PL/SQL内部错误                                             |
| PROGRAM_ERROR           | ORA-06501 | PL/SQL内部错误                                             |
| ACCESS_INTO_NULL        | ORA-06530 | 给空对象属性赋值                                           |
| COLLECTION_IS_NULL      | ORA-06531 | 对某NULL  PL/SQL表或可变数组试图应用集合方法，而不是EXISTS |
| SELF_IS_NULL            | ORA-30625 | 调用空对象实例的方法                                       |
| SUBSCRIPT_BEYOND_COUNT  | ORA-06533 | 对嵌套表或数组索引引用时超出集合中元素的数量               |
| SUBSCRIPT_OUTSIDE_LIMIT | ORA-06532 | 对嵌套表或可变数组索引的引用超出声明的范围                 |

#### 5.2.2 非预定义异常

有一些Oracle错误没有预定义异常与其关联，需要在语句块的声明部分声明一个异常名称，然后通过编译指示PRAGMA EXCEPTION_INIT

将该异常名称与一个Oracle错误相关联。此后，当执行过程出现该错误时将自动抛出该异常。

```plsql
声明一个异常名称
e_integrity EXCEPTION;
将异常与一个Oracle错误号相绑定
PRAGMA EXCEPTION-INIT(e_integrity.-2291)
示例
DECLARE
  e_deptno_fk EXCEPTION;
  PRAGMA EXCEPTION_INIT(e_deptno_fk,-2292);
BEGIN
……
EXCEPTION
……
END;
```

#### 5.2.3用户自定义的异常

- 用户定义错误是指，有些操作并不会产生Oracle错误，但是从业务规则角度考虑，认为是一种错误。
- 用户自定义异常必须在声明部分进行声明。
- 当异常发生时，系统不能自动触发，需要用户使用RAISE语句。
- 在异常处理部分捕捉并处理异常。

### 5.3 异常处理过程

异常处理分3个步骤进行：

- 在声明部分为错误定义异常，包括非预定义异常和用户定义异常。
- 在执行过程中当错误产生时抛出与错误对应的异常。
- 在异常处理部分通过异常处理器捕获异常，并进行异常处理。

#### 5.3.1 异常的定义

Oracle中的3种异常，其中预定义异常由系统定义，而其他两种异常则需要用户定义。

- 定义异常方法：

  - ```plsql
    e_exception EXCEPTION;
    ```

- 如果是非预定义的异常，需要将异常与一个Oracle错误相关联，其语法为：

  - ```plsql
    PRAGMA EXCEPTION_INIT(e_exception, -#####);
    ```

> 注意：Oracle内部错误号用一个负的5位数表示，如-02292。其中  -20999～-20000为用户定义错误的保留号。

#### 5.3.2 异常的抛出

由于系统可以自动识别Oracle内部错误，因此当错误产生时系统会自动抛出与之对应的预定义异常或非预定义异常。但是，系统无法识别用户定义错误，因此当用户定义错误产生时，需要用户手动抛出与之对应的异常。
用户定义异常的抛出语法为

```plsql
RAISE user_define_exception； 
```

#### 5.3.3 异常的捕获与处理

异常处理器的基本形式为

```plsql
EXCEPTION
WHEN exception1[OR excetpion2…]THEN 
   sequence_of_statements1;
WHEN exception3[OR exception4…]THEN 
   sequence_of_statements2;
……
WHEN OTHERS THEN 
   sequence_of_statementsn;
END;
```

> 一个异常处理器可以捕获多个异常，只需在WHEN子句中用OR连接即可；
> 一个异常只能被一个异常处理器捕获，并进行处理。 

查询名为SMITH的员工工资，如果该员工不存在，则输出“There is not such an employee!”；如果存在多个同名的员工，则输出其员工号和工资。

```plsql
DECLARE
  v_sal emp.sal%type;
BEGIN
  SELECT sal INTO v_sal FROM emp WHERE ename='SMITH';
  DBMS_OUTPUT.PUT_LINE(v_sal);
EXCEPTION
  WHEN NO_DATA_FOUND THEN 
    DBMS_OUTPUT.PUT_LINE('There is not such an emplyee!');
  WHEN TOO_MANY_ROWS THEN
    FOR v_emp IN (SELECT * FROM emp WHERE ename='SMITH')    
    LOOP
       DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||v_emp.sal);
    END LOOP;
END;
```

删除dept表中部门号为10的部门信息，如果不能删除则输出“There are subrecords in emp table!”。

```plsql
DECLARE
  e_deptno_fk EXCEPTION;
  PRAGMA EXCEPTION_INIT(e_deptno_fk,-2292);
BEGIN
  DELETE FROM dept WHERE deptno=10;
EXCEPTION
  WHEN e_deptno_fk THEN
     DBMS_OUTPUT.PUT_LINE(' There are subrecords in emp table!');
END;
```

修改7844员工的工资，保证修改后工资不超过6000。

```plsql
DECLARE
  e_highlimit EXCEPTION;
  v_sal emp.sal%TYPE;
BEGIN
  UPDATE emp SET sal=sal+100 WHERE empno=7844 RETURNING sal INTO v_sal;
  IF v_sal>6000 THEN 
     RAISE e_highlimit;
  END IF;
EXCEPTION
  WHEN e_highlimit THEN
    DBMS_OUTPUT.PUT_LINE('The salary is too large!');
    ROLLBACK;
END; 
```

#### 5.3.4 OTHERS异常处理器

- OTHERS异常处理器是一个特殊的异常处理器，可以捕获所有的异常。
- 通常，OTHERS异常处理器总是作为异常处理部分的最后一个异常处理器，负责处理那些没有被其他异常处理器捕获的异常。 

```plsql
DECLARE
  v_sal emp.sal%TYPE;
  e_highlimit EXCEPTION;
BEGIN
  SELECT sal INTO v_sal FROM emp WHERE ename='JOAN';
  UPDATE emp SET sal=sal+100 WHERE empno=7900;
  IF v_sal>6000 THEN 
    RAISE e_highlimit;
  END IF;
EXCEPTION
  WHEN e_highlimit THEN
    DBMS_OUTPUT.PUT_LINE('The salary is too large!');
   ROLLBACK;
  WHEN OTHERS THEN
   DBMS_OUTPUT.PUT_LINE('There is some wrong in selecting!');
END; 
```

可以通过两个函数来获取错误相关信息。

SQLCODE：返回当前错误代码。

- 如果是用户定义错误返回值为1；
- 如果是ORA-1403：NO DATA FOUND错误，返回值为100
- 其他Oracle内部错误返回相应的错误号。 

SQLERRM：返回当前错误的消息文本。

- 如果是Oracle内部错误，返回系统内部的错误描述；
- 如果是用户定义错误，则返回信息文本为“User-defined Exception”。 

```plsql
DECLARE
  v_sal emp.sal%TYPE;
  e_highlimit EXCEPTION;
  v_code NUMBER(6);
  v_text VARCHAR2(200);
BEGIN
  SELECT sal INTO v_sal FROM emp WHERE ename='JOAN';
  UPDATE emp SET sal=sal+100 WHERE empno=7900;
  IF v_sal>6000 THEN 
    RAISE e_highlimit;
  END IF;
EXCEPTION
  WHEN e_highlimit THEN
    DBMS_OUTPUT.PUT_LINE('The salary is too large!');
    ROLLBACK;
  WHEN OTHERS THEN
    v_code:=SQLCODE;
    v_text:=SQLERRM;   
    DBMS_OUTPUT.PUT_LINE(v_code||' '||v_text);
END;
```

### 5.4 异常的传播

#### 5.4.1 执行部分异常的传播

- 如果当前语句块有该异常的处理器，则执行之，并且成功完成该语句块。然后，控制权传递到外层语句块。 
- 如果当前语句块没有该异常的处理器，则通过在外层语句块中产生该异常来传播该异常。然后，执行对外层语句块执行步骤1。如果没有外层语句块，则该异常将传播到调用环境。 

```plsql
DECLARE
  v_sal emp.sal%TYPE;
BEGIN
  BEGIN  
    SELECT sal INTO v_sal FROM emp WHERE ename='JOAN';
  EXCEPTION
    WHEN NO_DATA_FOUND THEN 
      DBMS_OUTPUT.PUT_LINE('There is not such an employee!');
  END;
  DBMS_OUTPUT.PUT_LINE('Now this is outputted by outer 
                                          block!');
END;
输出
There is not such an employee!
Now this is outputted by outer block! 

DECLARE
  v_sal emp.sal%TYPE;
BEGIN
  BEGIN  
    SELECT sal INTO v_sal FROM emp WHERE deptno=10;
  EXCEPTION
    WHEN NO_DATA_FOUND THEN 
      DBMS_OUTPUT.PUT_LINE('There is not such an employee!');
  END;
  DBMS_OUTPUT.PUT_LINE('Now this is outputted by outer 
                                           block!');
EXCEPTION
  WHEN TOO_MANY_ROWS THEN
    DBMS_OUTPUT.PUT_LINE('There are more than one employee!');
END;
输出
There are more than one employee!
```

#### 5.4.2 声明部分和异常处理部分的异常

声明部分和异常处理部分的异常会立刻传播到外层语句块的异常处理部分，即使当前语句块有该异常的异常处理器。 

```plsql
BEGIN
  DECLARE
v_number NUMBER(6) :='ABC';
BEGIN
    v_number:=10;
  EXCEPTION
    WHEN OTHERS THEN
       DBMS_OUTPUT.PUT_LINE('This is outputted by inner         
                                                 block!');
  END;
EXCEPTION
WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('This is outputted by outer 
                                              block!');
END;
输出
This is outputted by outer block! 
```

- 无论是执行部分的异常，还是声明部分或异常处理部分的异常，如果在本块中没有处理，最终都将向外层块中传播。
- 通常在程序最外层块的异常处理部分放置OTHERS异常处理器，以保证没有错误被漏掉检测，否则错误将传递到调用环境。

## 六、存储子程序

- 存储子程序是被命名的PL/SQL块，以编译的形式存储在数据库服务器中，可以在应用程序中进行调用，是PL/SQL程序模块化的一种体现。
- PL/SQL中的存储子程序包括存储过程和（存储）函数两种。 
- 存储子程序是以独立对象的形式存储在数据库服务器中，因此是一种全局结构，与之对应的是局部子程序，即嵌套在PL/SQL块中的局部过程和函数，其存储位置取决于其所在的父块的位置。 

### 6.1 存储过程

#### 6.1.1 存储过程的创建

```plsql
CREATE [OR REPLACE] PROCEDURE procedure_name
(
    parameter1_name [mode] datatype [DEFAULT|:=value]
	[, parameter2_name [mode] datatype [DEFAULT|:=value],…]
)
AS|IS
   /*Declarative section is here */
BEGIN
   /*Executable section is here*/ 
EXCEPTION
   /*Exception section is here*/ 
END[procedure_name]; 
```

创建一个存储过程，以部门号为参数，查询该部门的平均工资，并输出该部门中比平均工资高的员工号、员工名。

```plsql
CREATE OR REPLACE PROCEDURE show_emp(
	p_deptno emp.deptno%TYPE
)
AS
  	v_sal emp.sal%TYPE;
BEGIN
	SELECT avg(sal) INTO v_sal FROM emp 
    WHERE deptno=p_deptno;
	DBMS_OUTPUT.PUT_LINE(p_deptno||' '||'average salary is:'|| v_sal);
	FOR v_emp IN (SELECT * FROM emp WHERE deptno=p_deptno AND sal>v_sal) LOOP
    		DBMS_OUTPUT.PUT_LINE(v_emp.empno||' '||v_emp.ename);
	END LOOP;
EXCEPTION
 	WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('The department doesn’’t exists!');
END show_emp;
```

> 通常，存储过程不需要返回值，如果需要返回一个值可以通过函数调用实现。但是，如果希望返回多个值，可以使用OUT参数来实现。

创建一个存储过程，以部门号为参数，返回该部门的人数和最高工资。

```plsql
CREATE OR REPLACE PROCEDURE return_deptinfo(
p_deptno emp.deptno%TYPE,
p_avgsal OUT emp.sal%TYPE
p_count  OUT emp.sal%TYPE)
AS
BEGIN
     SELECT avg(sal),count(*) INTO p_avgsal,p_count 
     FROM emp 
     WHERE deptno=p_deptno;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('The department don’’t exists!');
END return_deptinfo; 
```

#### 6.1.2 存储过程的调用

在SQL*PLUS中调用

```plsql
EXEC  procedure_name(parameter_list)
```

在PL/SQL块中调用

```plsql
BEGIN
      procedure_name(parameter_list);
END；
注意
在PL/SQL程序中，存储过程可以作为一个独立的表达式被调用。 
```

#### 6.1.3 存储过程的管理

```plsql
修改存储过程 
CREATE OR REPLACE PROCEDURE procedure_name
查看存储过程及其源代码 
查询数据字典视图USER_SOURCE
SELECT name,text FROM user_source 
WHERE type='PROCEDURE'; 
重新编译存储过程 
ALTER PROCEDURE…COMPILE 
ALTER PROCEDURE show_emp COMPILE; 
删除存储过程 
DROP PROCEDURE
DROP PROCEDURE show_emp;
```

### 6.2 函数

#### 6.2.1 函数的创建

```plsql
CREATE [OR REPLACE] FUNCTION function_name (
	parameter1_name [mode] datatype [DEFAULT|:=value]
	[, parameter2_name [mode] datatype [DEFAULT|:=value],…]
)RETURN return_datatype 
AS|IS
     /*Declarative section is here */
BEGIN
    /*Executable section is here*/ 
EXCEPTION
    /*Exception section is here*/ 
END [function_name]; 
```

> 在函数定义的头部，参数列表之后，必须包含一个RETURN语句来指明函数返回值的类型，但不能约束返回值的长度、精度、刻度等。如果使用%TYPE，则可以隐含地包括长度、精度、刻度等约束信息；
> 在函数体的定义中，必须至少包含一个RETURN 语句，来指明函数返回值。也可以有多个RETURN语句，但最终只有一个RETURN语句被执行

创建一个以部门号为参数，返回该部门最高工资的函数。

```plsql
CREATE OR REPLACE FUNCTION return_maxsal(
	p_deptno emp.deptno%TYPE
)RETURN emp.sal%TYPE
AS
    v_maxsal emp.sal%TYPE;
BEGIN
   SELECT max(sal) INTO v_maxsal FROM emp 
   WHERE deptno=p_deptno;
   RETURN v_maxsal;
EXCEPTION 
   WHEN NO_DATA_FOUND THEN
       DBMS_OUTPUT.PUT_LINE('The deptno is invalid!');
END return_maxsal;
```

如果需要函数返回多个值，可以使用OUT或IN OUT模式参数。

创建一个函数，以部门号为参数，返回部门名、部门人数及部门平均工资。

```plsql
CREATE OR REPLACE FUNCTION ret_deptinfo(
  p_deptno dept.deptno%TYPE,
  p_num OUT NUMBER,
  p_avg OUT NUMBER) RETURN dept.dname%TYPE
AS
  v_dname dept.dname%TYPE;
BEGIN
  SELECT dname INTO v_dname FROM dept 
  WHERE deptno=p_deptno;
  SELECT count(*),avg(sal) INTO p_num,p_avg 
  FROM emp WHERE deptno=p_deptno;
  RETURN v_dname;
END ret_maxsal; 
```

#### 6.2.2 函数的调用

**在SQL语句中调用函数**

函数可以在SQL语句的以下部分调用：

- SELECT语句的目标列；
- WHERE和HAVING子句；
- CONNECT BY，START WITH，ORDER BY，GROUP BY子句；
- INSERT语句的VALUES子句中；
- UPDATE语句的SET子句中。 

**在PL/SQL中调用函数**

通过return_maxsal函数的调用，输出各个部门的最高工资；通过ret_deptinfo函数调用，输出各个部门名、部门人数及平均工资。

```plsql
DECLARE
  v_maxsal emp.sal%TYPE;
  v_avgsal emp.sal%TYPE;
  v_num    NUMBER;
  v_dname  dept.dname%TYPE;
BEGIN
  FOR v_dept IN (SELECT DISTINCT deptno FROM emp) LOOP
      v_maxsal:=ret_maxsal(v_dept.deptno);
      v_dname:=ret_deptinfo(v_dept.deptno,v_num,v_avgsal);
      DBMS_OUTPUT.PUT_LINE(v_dname||' '||v_maxsal||' '||  
                                             v_avgsal||' '||v_num);
  END LOOP;
END;
```

注意：函数只能作为表达式的一部分被调用。

#### 6.2.3函数的管理

```plsql
函数的修改
CREATE OR REPLACE FUNCTION function_name
查看函数及其源代码
查询数据字典视图USER_SOURCE
SELECT name,text FROM user_source 
WHERE type='FUNCTION';
函数重编译
ALTER FUNCTION…COMPILE
ALTER FUNCTION ret_maxsal COMPILE;
删除函数
DROP FUNCTION
DROP FUNCTION ret_maxsal;
```

## 七、包

### 7.1 包概述

包是包含一个或多个子程序单元（过程、函数等）的容器。
包是一种全局结构 。
包类型

- 数据库内置包 
- 用户创建的包

包构成

- 包规范：包规范声明了软件包中所有内容，如过程、函数、游标、类型、异常和变量等，其中过程和函数只包括原型信息，不包含任何子程序代码。  
- 包体：包体中包含了在包头中的过程和函数的实现代码。包体中还可以包括在规范中没有声明的变量、游标、类型、异常、过程和函数，但是它们是私有元素，只能由同一包体中其他过程和函数使用。

### 7.2 包的创建

#### 7.2.1 创建包规范

```plsql
CREATE OR REPLACE PACKAGE package_name 
IS|AS
[PRAGMA SERIALLY_RESUABLE]
  type_definition|variable_declaration|
  exception_declaration|cursor_declaration| 
  procedure_ declaration|function_ declaration
END [package_name];

```

- 元素声明的顺序可以是任意的，但必须先声明后使用；
- 所有元素是可选的；
- 过程和函数的声明只包括原型，不包括具体实现。

创建一个软件包，包括2个变量、2个过程和1个异常。

```plsql
CREATE OR REPLACE PACKAGE pkg_emp
AS
  minsal   NUMBER;
  maxsal   NUMBER;
  e_beyondbound  EXCEPTION;
  PROCEDURE update_sal(
           p_empno NUMBER, p_sal NUMBER);
  PROCEDURE add_employee(
           p_empno NUMBER,p_sal NUMBER);
END pkg_emp;
```

#### 7.2.2 创建包体

```plsql
CREATE OR REPLACE PACKAGE BODY package_name 
IS|AS
[PRAGMA SERIALLY_RESUABLE]
  type_definition|variable_declaration|
  exception_declaration|
  cursor_declaration| 
  procedure_definition |
  function_definition
END [package_name]; 
```

- 包体中函数和过程的原型必须与包规范中的声明完全一致；
- 只有在包规范已经创建的条件下，才可以创建包体；
- 如果包规范中不包含任何函数或过程，则可以不创建包体。 

```plsql
CREATE OR REPLACE PACKAGE BODY pkg_emp
AS
    PROCEDURE update_sal(p_empno NUMBER, p_sal NUMBER)
    AS
    BEGIN
      SELECT min(sal), max(sal) INTO minsal,maxsal FROM emp;
      IF p_sal BETWEEN minsal AND maxsal THEN
        UPDATE emp SET sal=p_sal WHERE empno=p_empno;
        IF SQL%NOTFOUND THEN
          RAISE_APPLICATION_ERROR(-20000,'The employee doesn''t exist');
        END IF;
      ELSE
        RAISE e_beyondbound;
      END IF;
 	 EXCEPTION
       WHEN e_beyondbound THEN
         DBMS_OUTPUT.PUT_LINE('The salary is beyond bound! ');
     END update_sal; 
     
	 PROCEDURE add_employee(p_empno NUMBER,p_sal NUMBER)
     AS
 	 BEGIN
       SELECT min(sal), max(sal) INTO minsal,maxsal FROM emp;
       IF p_sal BETWEEN minsal AND maxsal THEN
         INSERT INTO emp(empno,sal) VALUES(p_empno,p_sal);
       ELSE
         RAISE e_beyondbound;
       END IF;
     EXCEPTION
       WHEN e_beyondbound THEN
         DBMS_OUTPUT.PUT_LINE('The salary is beyond bound! ');
     END add_employee;
END pkg_emp;
```

#### 7.2.3 包的调用

在包规范声明的任何元素是公有的，在包外都是可见的

- 包外：通过package.element形式调用；
- 包内：直接通过元素名进行调用。 

在包体中定义而没有在包头中声明的元素是私有的，只能在包体中引用 

调用软件包pkg_emp中的过程update_sal，修改7844员工工资为3000。调用add_employee添加一个员工号为1357，工资为4000的员工。

```plsql
BEGIN
  pkg_emp.update_sal(7844,3000);
  pkg_emp.add_employee(1357,4000);
END;
```

#### 7.2.4 包重载

重载子程序必须同名不同参，即名称相同，参数不同。参数不同体现为参数的个数、顺序、类型等不同。

如果两个子程序参数只是名称和模式不同，则不能重载。

- PROCEDURE overloadme(parameter1 IN NUMBER);
- PROCEDURE overloadme(parameter2 OUT NUMBER); 

不能根据两个函数返回类型不同而对它们进行重载。

- FUNCTION overloadme RETURN DATE;
- FUNCTION overloadme RETURN NUMBER;

重载子程序参数必须在类型系列方面有所不同。

- PROCEDURE overloadchar(parameter IN CHAR);
- PROCEDURE overloadchar(parameter IN VARCHAR2);

### 7.3 包的管理

```plsql
包的修改
CREATE OR REPLACE PACKAGE package_name
查看包及其源代码
查询数据字典视图USER_SOURCE
SELECT name,text FROM user_source WHERE type='PACKAGE';
SELECT name,text FROM user_source WHERE type='PACKAGE BODY';
重新编译包
ALTER PACKAGE…COMPILE（包规范和包体）
ALTER PACKAGE…COMPILE SPECIFICATION（包规范）
ALTER PACKAGE… COMPILE BODY（包体）   
ALTER PACKAGE pkg_emp COMPILE;
ALTER PACKAGE pkg_emp COMPILE SPECIFICATION;
ALTER PACKAGE pkg_emp COMPILE BODY;
删除包
DROP PACKAGE （包规范和包体）
DROP PACKAGE BODY （包体） 
DROP PACKAGE BODY pkg_emp;
DROP PACKAGE pkg_emp;
```

## 八、触发器

### 8.1 触发器概述

#### 8.1.1 触发器的概念和作用

- 触发器是一种特殊类型的存储过程，编译后存储在数据库服务器中。
- 当特定事件发生时，由系统自动调用执行，而不能由应用程序显式地调用执行。
- 触发器不接受任何参数。
- 触发器主要用于维护那些通过创建表时的声明约束不可能实现的复杂的完整性约束，并对数据库中特定事件进行监控和响应。

#### 8.1.2 触发器的类型

- DML触发器：建立在基本表上的触发器，响应基本表的INSERT，UPDATE，DELETE操作。
- INSTEAD OF触发器：建立在视图上的触发器，响应视图上的INSERT，UPDATE，DELETE操作。
- 系统触发器：建立在系统或模式上的触发器，响应系统事件和DDL（CREATE，ALTER，DROP）操作。 

#### 8.1.3 触发器的组成

触发器由触发器头部和触发器体两个部分组成，主要包括：

- 作用对象：触发器作用的对象包括表、视图、数据库和模式。
- 触发事件：激发触发器执行的事件。如DML、DDL、数据库系统事件等。
- 触发时间：用于指定触发器在触发事件完成之前还是之后执行。如果指定为AFTER，则表示先执行触发事件，然后再执行触发器；如果指定为BEFORE，则表示先执行触发器，然后再执行触发事件。
- 触发级别：触发级别用于指定触发器响应触发事件的方式。默认为语句级触发器，即触发事件发生后，触发器只执行一次。如果指定为FOR EACH ROW，即为行级触发器，则触发事件每作用于一个记录，触发器就会执行一次。
- 触发条件：由WHEN子句指定一个逻辑表达式，当触发事件发生，而且WHEN条件为TRUE时，触发器才会执行。
- 触发操作：触发器执行时所进行的操作。

### 8.2 DML触发器

**DML触发器的种类**

- 语句级前触发器
- 语句级后触发器
- 行级前触发器
- 行级后触发器

**DML触发器的执行顺序**

- 如果存在，则执行语句级前触发器。
- 对于受触发事件影响的每一个记录：
  - 如果存在，则执行行级前触发器；
  - 执行当前记录的DML操作（触发事件）；
  - 如果存在，则执行行级后触发器。
- 如果存在，则执行语句级后触发器。 

#### 8.2.1 创建DML触发器

```plsql
CREATE [OR REPLACE] TRIGGER trigger_name
BEFORE|AFTER triggering_event [OF column_name]
ON table_name]
[FOR EACH ROW]
[WHEN trigger_condition]
DECLARE
     /*Declarative section is here */
BEGIN
     /*Exccutable section si here*/ 
EXCEPTION
     /*Exception section is here*/ 
END [trigger_name];
```

##### **语句级触发器** 

在默认情况下创建的DML触发器为语句级触发器，即触发事件发生后，触发器只执行一次

创建一个触发器，禁止在休息日改变雇员信息

```plsql
CREATE OR REPLACE TRIGGER trg_emp_weekend
BEFORE INSERT OR UPDATE OR DELETE ON emp
BEGIN
   IF TO_CHAR(SYSDATE, 'DY', 'nls_date_language= american') IN('SAT', 'SUN')
   	THEN raise_application_error(-20000, 'Can''t operate in weekend. ');
   END IF;
END trg_emp_weekend;
```

如果触发器响应多个DML事件，而且需要根据事件的不同进行不同的操作，则可以在触发器体中使用3个条件谓词。

| 谓词      | 行为                                          |
| --------- | --------------------------------------------- |
| INSERTING | 如果触发语句是INSERT，则为TRUE；  否则为FALSE |
| UPDATING  | 如果触发语句是UPDATE，则为TRUE；  否则为FALSE |
| DELETING  | 如果触发语句是DELETE，则为TRUE；  否则为FALSE |

为emp表创建一个触发器，当执行插入操作时，统计操作后员工人数；当执行更新工资操作时，统计更新后员工平均工资；当执行删除操作时，统计删除后各个部门的人数。

```plsql
CREATE OR REPLACE TRIGGER trg_emp_dml
AFTER INSERT OR UPDATE OR DELETE ON emp
DECLARE
  v_count NUMBER;
  v_sal   NUMBER(6,2);
BEGIN
  IF INSERTING THEN 
    SELECT count(*) INTO v_count FROM emp;
    DBMS_OUTPUT.PUT_LINE(v_count);
  ELSIF UPDATING THEN
    SELECT avg(sal) INTO v_sal FROM emp;
    DBMS_OUTPUT.PUT_LINE(v_sal);
  ELSE
    FOR v_dept IN (SELECT deptno,count(*) num FROM emp GROUP BY deptno) LOOP
      DBMS_OUTPUT.PUT_LINE(v_dept.deptno||' '||v_dept.num);
    END LOOP;
  END IF;
END trg_emp_dml; 
```

##### **行级触发器**

- 行级触发器是指执行DML操作时，每操作一个记录，触发器就执行一次，一个DML操作涉及多少个记录，触发器就执行多少次。
- 在行级触发器中可以使用WHEN条件，进一步控制触发器的执行。
- 在行级触发器中引入了:old和:new 两个标识符，来访问和操作当前被处理记录中的数据。 

**标识符**
:old和:new作为triggering_table%ROWTYPE类型的两个变量

在不同触发事件中，:old和:new的意义不同

| 触发事件 | :old                     | :new                       |
| -------- | ------------------------ | -------------------------- |
| INSERT   | 未定义，所有字段都为NULL | 当语句完成时，被插入的记录 |
| UPDATE   | 更新前原始记录           | 当语句完成时，更新后的记录 |
| DELETE   | 记录被删除前的原始值     | 未定义，所有字段都为NULL   |

引用方式：
      :old.field和:new.field （执行部分）
       old.field 和new.field   (WHEN条件中)
注意事项：

- 是伪记录，不能作为整个记录进行赋值或引用 
- 不能传递给带triggering_table%ROWTYPE参数的过程和函数 
- 如果触发器是建立在嵌套表上，:old和:new都执行嵌套表的行，:parent指向父表中的当前行。

为emp表创建一个触发器，当插入新员工时显示新员工的员工号、员工名；当更新员工工资时，显示修改前后员工工资；当删除员工时，显示被删除的员工号、员工名。 

```plsql
CREATE OR REPLACE TRIGGER trg_emp_dml_row
BEFORE INSERT OR UPDATE OR DELETE ON emp
FOR EACH ROW
BEGIN
   IF INSERTING THEN 
         DBMS_OUTPUT.PUT_LINE(:new.empno||' '||:new.ename);
   ELSIF UPDATING THEN
         DBMS_OUTPUT.PUT_LINE(:old.sal||' '||:new.sal);
   ELSE
         DBMS_OUTPUT.PUT_LINE(:old.empno||' '||:old.ename);
   END IF;
END trg_emp_dml_row;
```

在行级触发器中，可以使用WHEN子句进一步控制触发器的执行。

例如，修改员工工资时，保证修改后的工资高于修改前的工资。

```plsql
CREATE OR REPLACE TRIGGER trg_emp_update_row
BEFORE UPDATE OF sal ON emp
FOR EACH ROW
WHEN(new.sal<=old.sal)
BEGIN
    RAISE_APPLICATION_ERROR(20001,'The salary is lower!');
END trg_emp_update_row; 
```

##### 变异表触发器

变异表是指激发触发器的DML语句所操作的表，即触发器为之定义的表，或者由于DELETE CASCADE操作而需要修改的表，即当前表的子表。

约束表是指由于引用完整性约束而需要从中读取或修改数据的表，即当前表的父表。

当对一个表创建行级触发器，或创建由DELETE CASCADE操作而激发的语句级触发器时，有下列两条限制：

- 不能读取或修改任何触发语句的变异表；
- 不能读取或修改触发表的一个约束表的PRIMARY KEY，UNIQUE 或FOREIGN KEY关键字的列, 但可以修改其他列。

注意

- 如果INSERT…VALUES语句只影响一行，那么该语句的行级前触发器不会把触发表当做变异表对待。
- INSERT INTO table SELECT…等语句总是把触发表当做变异表，即使子查询仅仅返回一条记录。 

#### 8.2.2管理触发器

```plsql
激活或禁用触发器
可以激活或禁用某个触发器。语法为
ALTER TRIGGER triggername ENABLE|DISABLE;
激活或禁用某个表对象上的所有触发器。语法为
ALTER TABLE table_name ENABLE|DISABLE ALL 
  TRIGGERS;
修改触发器
CREATE OR REPLACE TRIGGER trigger_name
重新编译触发器
ALTER TRIGGER trigger_name COMPILE; 
查看触发器及其源代码
查询数据字典视图USER_TRIGGERS
SELECT trigger_name,trigger_type,
table_name,trigger_body FROM user_triggers;
删除触发器
当触发器不再需要时，可以使用DROP TRIGGER语句删除触发器。
DROP TRIGGER trigger_name; 
```

