# SQL (Structured Query Language)

> 관계형 데이터베이스 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어

-

### What is SQL?
- 구조화 된 쿼리 언어
- SQL을 사용하면 데이터베이스에 액세스하고 조작 할 수 있습니다.
- SQL은 ANSI (American National Standards Institute) 표준입니다.

### What Can SQL do?
- 데이터베이스에 대해 **쿼리**를 실행할 수 있습니다.
- 데이터베이스에서 데이터를 **검색** 할 수 있습니다.
- 데이터베이스에 레코드를 **삽입** 할 수 있습니다.
- 데이터베이스의 레코드를 **업데이트** 할 수 있습니다.
- 데이터베이스에서 레코드를 **삭제**할 수 있습니다.
- **새로운 데이터베이스를 생성** 할 수 있습니다.
- 데이터베이스에 **새로운 테이블을 생성** 할 수 있습니다.
- 데이터베이스에 **저장 프로 시저**를 생성 할 수 있습니다.
- 데이터베이스에 **뷰**를 생성 할 수 있습니다.
- 테이블, 프로 시저 및 뷰에 대한 **사용 권한을 설정**할 수 있습니다.

### RDBMS
>관계형 데이터베이스 관리  
>
>SQL과 MS SQL Server, IBM DB2, Oracle, MySQL 및 Microsoft Access와 같은 모든 최신 데이터베이스 시스템의 기초
>
>RDBMS의 데이터는 테이블이라는 데이터베이스 오브젝트에 저장됩니다.
>
테이블은 관련 데이터 항목의 모음이며 열과 행으로 구성됩니다.

-
## SQL Syntax

### Database Tables
> 데이터베이스는 대개 하나 이상의 테이블을 포함합니다.  
>  각 테이블은 이름 (예 : '고객'또는 '주문')으로 식별됩니다.  
>  테이블은 데이터가있는 레코드 (행)를 포함합니다.

CustomerID|ContactName|Address
---|---|---
1|maria|inchun
2|michael|buchun
3|jordon|seoul
4|dave|busan

### SQL Statements
>데이터베이스에서 수행해야하는 대부분의 조치는 SQL 문으로 완료됩니다.  
>
ex) `SELECT * FROM Customers;`    
Customers 테이블의 모든 레코드를 선택

- SQL 키워드는 대소 문자를 구분하지 않습니다.
- 세미콜론(;)은 데이터베이스 시스템에서 각 SQL 문을 분리하여 서버에 대한 동일한 호출에서 둘 이상의 SQL 문을 실행할 수 있도록하는 표준 방법입니다.

### Some of The Most Important SQL Commands

- SELECT - 데이터베이스에서 데이터를 추출
- UPDATE - 데이터베이스에 데이터를 업데이트
- DELETE - 데이터베이스에서 데이터를 삭제
- INSERT INTO - 데이타베이스에 새로운 데이터를 삽입
- CREATE DATABASE - 새 데이터베이스를 만듭니다
- ALTER DATABASE - 데이터베이스를 수정
- CREATE TABLE - 새로운 테이블을 생성
- ALTER TABLE - 테이블을 수정
- DROP TABLE - 테이블을 삭제합니다
- CREATE INDEX - 인덱스 (검색 키)를 생성
- DROP INDEX - 인덱스를 삭제

-

## SQL SELECT Statement
>SELECT 문은 데이터베이스에서 **데이터를 선택**하는 데 사용됩니다.  
결과는 결과 세트라고하는 결과 테이블에 저장됩니다.

### SELECT Syntax

```
SELECT column_name,column_name
FROM table_name;
```
```
SELECT * FROM table_name;
```

##### Example
`SELECT CustomerName,City FROM Customers;`  
> "Customers"테이블에서 "CustomerName"및 "City"열을 선택
 
`SELECT * FROM Customers;`  
> Customers 테이블에서 모든 컬럼을 선택


## SQL SELECT DISTINCT Statement
>테이블에서 열은 많은 중복 값을 포함 할 수 있습니다. 때로는 서로 다른 (뚜렷한) 값만 나열하려고합니다.
>
DISTINCT 키워드는 **고유 한 (다른) 값만 리턴**하는 데 사용될 수 있습니다.

### SELECT DISTINCT Syntax

```
SELECT DISTINCT column_name,column_name
FROM table_name;
```
##### Example
`SELECT DISTINCT City FROM Customers;`  
> "Customers"테이블의 "City"열에있는 고유 한 값만 선택


## SQL WHERE Clause
> 지정된 기준을 충족하는 레코드 만 추출하는 데 사용됩니다. (필터링)

### WHERE Syntax
```
SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;
```
##### Example
###### Text Fields vs. Numeric Fields
```
SELECT * FROM Customers
WHERE Country='Mexico';
```
>"Customers"테이블에서 "Mexico"국가의 모든 고객을 선택

```
SELECT * FROM Customers
WHERE CustomerID=1;
```
>SQL은 텍스트 값에 대해 작은 따옴표를 사용해야합니다 .  
>(대부분의 데이터베이스 시스템은 큰 따옴표도 허용합니다.)  


## SQL AND & OR Operators
>
AND 연산자는 첫 번째 조건과 두 번째 조건이 모두 참인 경우 레코드를 표시합니다.
>
OR 연산자는 첫 번째 조건 또는 두 번째 조건이 참이면 레코드를 표시합니다.

##### AND Operator Example
```
SELECT * FROM Customers
WHERE Country='Germany'
AND City='Berlin';
```
>"Customers"테이블에서 "Germany"국가와 "Berlin"도시의 모든 고객을 선택

##### OR Operator Example
```
SELECT * FROM Customers
WHERE City='Berlin'
OR City='München';
```
>"Customers"테이블에서 도시 "Berlin"또는 "München"의 모든 고객을 선택

##### Combining AND & OR
```
SELECT * FROM Customers
WHERE Country='Germany'
AND (City='Berlin' OR City='München');
```
>
"Customers"테이블에서 "Germany"국가의 모든 고객을 선택하고 도시는 "Berlin"또는 "München"과 같아야합니다.


## SQL ORDER BY Keyword
>
ORDER BY 키워드는 하나 이상의 열을 기준으로 결과 집합을 정렬하는 데 사용됩니다.
>
ORDER BY 키워드는 기본적으로 레코드를 오름차순으로 정렬합니다. 내림차순으로 레코드를 정렬하려면 DESC 키워드를 사용할 수 있습니다.

### ORDER BY Syntax
```
SELECT column_name, column_name
FROM table_name
ORDER BY column_name ASC|DESC, column_name ASC|DESC;
```

##### ORDER BY Example
```
SELECT * FROM Customers
ORDER BY Country;
```
>"고객"테이블의 모든 고객을 "국가"열로 정렬하여 선택

##### ORDER BY DESC Example
```
SELECT * FROM Customers
ORDER BY Country DESC;
```
>'고객'테이블의 모든 고객을 '국가'열에 의해 DESCENDING로 정렬하여 선택

##### ORDER BY Several Columns Example
```
SELECT * FROM Customers
ORDER BY Country, CustomerName;
```
>"Customers"테이블의 모든 고객을 "Country"및 "CustomerName"열로 정렬

##### ORDER BY Several Columns Example 2
```
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```
>"Customers"테이블의 모든 고객을 "국가"에 따라 오름차순으로 정렬하고 "CustomerName"열로 내림차순으로 정렬


## SQL INSERT INTO Statement
>INSERT INTO 문은 테이블에 새 레코드를 삽입하는 데 사용됩니다.

### INSERT INTO Syntax

- 첫 번째 형식은 데이터가 삽입 될 열 이름을 지정하지 않고 값만 지정합니다.

```
INSERT INTO table_name
VALUES (value1,value2,value3,...);
```
- 두 번째 형식은 열 이름과 삽입 할 값을 모두 지정합니다.

```
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

##### INSERT INTO Example
```
INSERT INTO Customers (CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal','Tom B. Erichsen','Skagen 21','Stavanger','4006','Norway');
```
>"Customers"테이블에 새 행을 삽입하려고한다고 가정합니다.

##### Insert Data Only in Specified Columns
>특정 열에 만 데이터를 삽입 할 수도 있습니다.
>
다음 SQL 문은 새 행을 삽입하지만 "CustomerName", "City"및 "Country"열에 만 데이터를 삽입합니다 (물론 CustomerID 필드도 자동으로 업데이트됩니다).

```
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```

## SQL UPDATE Statement

##### Example

```
UPDATE Customers
SET City='Hamburg'
WHERE CustomerID=1;
```
>"Customers"테이블에서 레코드의 "도시"열 값을 변경

### SQL UPDATE Statement
>UPDATE 문은 테이블의 기존 레코드를 업데이트하는 데 사용

### Syntax
```
UPDATE table_name
SET column1=value1,column2=value2,...
WHERE some_column=some_value;
```
>WHERE 절은 갱신해야하는 레코드를 지정합니다.  
>**WHERE 절을 생략하면 모든 레코드가 업데이트**됩니다!

### UPDATE Multiple Columns
```
UPDATE Customers
SET ContactName='Alfred Schmidt', City='Frankfurt'
WHERE CustomerID=1;
```
>둘 이상의 열을 업데이트하려면 쉼표를 구분 기호로 사용

### UPDATE Multiple Records
> 갱신 명령문에서 갱신 될 레코드 수를 결정하는 것은 WHERE 절입니다.

```
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```
### Update Warning!
>WHERE 절을 생략하면 모든 레코드가 업데이트됩니다.

```
UPDATE Customers
SET ContactName='Juan';
```
## SQL DELETE Statement
> DELETE 문은 테이블의 행을 삭제하는 데 사용됩니다.

### DELETE Syntax
```
DELETE FROM table_name
WHERE some_column=some_value;
```
##### DELETE Example
>"고객"테이블에서 고객 "Alfreds Futterkiste"를 삭제

```
DELETE FROM Customers
WHERE CustomerName='Alfreds Futterkiste' AND ContactName='Maria Anders';
```
#### Delete All Data
```
DELETE FROM table_name;

or

DELETE * FROM table_name;
```
**레코드를 삭제할 때 매우주의해야합니다. 이 문장은 실행 취소 할 수 없습니다!**


## SQL Injection
>SQL Injection은 데이터베이스를 파괴 할 수 있습니다.  
>
>SQL injection은 악의적 인 사용자가 웹 페이지 입력을 통해 SQL 명령문에 SQL 명령을 주입 할 수있는 기술입니다.  
>삽입 된 SQL 명령은 SQL 문을 변경하고 웹 응용 프로그램의 보안을 손상시킬 수 있습니다.

### SQL Injection Based on 1=1 is Always True
ID : ` 105 or 1=1 `  
Server Result : `SELECT * FROM Users WHERE UserId = 105 or 1=1;`

> 위의 SQL이 유효합니다. 이후는, 테이블 사용자에서 모든 행을 반환합니다
Users 테이블에 이름과 암호가 포함되어 있다면  

`SELECT UserId, Name, Password FROM Users WHERE UserId = 105 or 1=1;`  
>105 또는 1 = 1을 입력 상자에 삽입하기 만하면 데이터베이스의 모든 사용자 이름과 암호에 액세스 할 수 있습니다.

### SQL Injection Based on ""="" is Always True

##### 일반적인 구조  
>id : abcd / pw : myPass  
>Server Code :  
>
>```
>uName = getRequestString("UserName");
>uPass = getRequestString("UserPass");
>
sql = 'SELECT * FROM Users WHERE Name ="' + uName + '" AND Pass ="' + uPass + '"'
>```
>Result:
`SELECT * FROM Users WHERE Name ="abcd" AND Pass ="myPass"`

##### 해커는 사용자 이름이나 암호 텍스트 상자에 "or" "="를 삽입하여 데이터베이스의 사용자 이름과 암호에 액세스한다.
>id: " or ""=" / pw: " or ""="
>Result:
>`SELECT * FROM Users WHERE Name ="" or ""="" AND Pass ="" or ""=""`
>이후는, 테이블 사용자에서 모든 행을 반환합니다.

### SQL Injection Based on Batched SQL Statements 
>대부분의 데이터베이스는 일괄 처리 된 SQL 문을 세미콜론으로 구분하여 지원합니다.  
>`SELECT * FROM Users; DROP TABLE Suppliers`  
>위의 SQL은 Users 테이블의 모든 행을 반환 한 다음 Suppliers라는 테이블을 삭제  
>
>우리가 다음 서버 코드를 가지고 있다면
>Server Code :
>
>```
>txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```
>id : `105; DROP TABLE Suppliers`  
>Result : `SELECT * FROM Users WHERE UserId = 105; DROP TABLE Suppliers`

### Parameters for Protection
> SQL 주입 공격을 막기 위해 SQL 입력에서 검색 할 단어 나 문자의 "블랙리스트"를 사용은 합당하지 않습니다.  
> (실제로 데이터베이스 필드에 SQL 문을 입력하는 것이 완벽하게 합법적이어야합니다.)  
> SQL 주입 공격으로부터 웹 사이트를 보호하는 유일한 입증 된 방법은 **SQL 매개 변수**를 사용하는 것입니다.  
> SQL 매개 변수는 실행 시간에 제어 된 방식으로 SQL 쿼리에 추가되는 값입니다.  
> 
> ##### ASP.NET Razor 예제
> ```
> txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = @0";
db.Execute(txtSQL,txtUserId);
```
>매개 변수는 SQL 문에 @ 마커로 표시됩니다.
>
SQL 엔진은 각 매개 변수가 해당 열에 대해 올바른지 확인하고 문자 그대로 처리되며 실행될 SQL의 일부가 아닌지 확인합니다.
>
>##### Another Example  
>```
>txtNam = getRequestString("CustomerName");
txtAdd = getRequestString("Address");
txtCit = getRequestString("City");
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
db.Execute(txtSQL,txtNam,txtAdd,txtCit);
```

#### Examples
>
##### SELECT STATEMENT IN ASP.NET:
```
txtUserId = getRequestString("UserId");
sql = "SELECT * FROM Customers WHERE CustomerId = @0";
command = new SqlCommand(sql);
command.Parameters.AddWithValue("@0",txtUserID);
command.ExecuteReader();
```
##### INSERT INTO STATEMENT IN ASP.NET:
```
txtNam = getRequestString("CustomerName");
txtAdd = getRequestString("Address");
txtCit = getRequestString("City");
txtSQL = "INSERT INTO Customers (CustomerName,Address,City) Values(@0,@1,@2)";
command = new SqlCommand(txtSQL);
command.Parameters.AddWithValue("@0",txtNam);
command.Parameters.AddWithValue("@1",txtAdd);
command.Parameters.AddWithValue("@2",txtCit);
command.ExecuteNonQuery();
```
##### INSERT INTO STATEMENT IN PHP:
```
$stmt = $dbh->prepare("INSERT INTO Customers (CustomerName,Address,City) 
VALUES (:nam, :add, :cit)");
$stmt->bindParam(':nam', $txtNam);
$stmt->bindParam(':add', $txtAdd);
$stmt->bindParam(':cit', $txtCit);
$stmt->execute();
```

## SQL SELECT TOP Clause

>SELECT TOP 절은 리턴 할 레코드 수를 지정하는 데 사용됩니다.
>
SELECT TOP 절은 수천 개의 레코드가있는 큰 테이블에서 매우 유용 할 수 있습니다. 많은 수의 레코드를 반환하면 성능에 영향을 줄 수 있습니다.  

### SQL Server / MS Access Syntax
```
SELECT TOP number|percent column_name(s)
FROM table_name;
```
### SQL SELECT TOP Equivalent in MySQL and Oracle
>
##### MySQL Syntax
```
SELECT column_name(s)
FROM table_name
LIMIT number;
```
##### Example
```
SELECT *
FROM Persons
LIMIT 5;
```
##### Oracle Syntax
```
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```
##### Example
```
SELECT *
FROM Persons
WHERE ROWNUM <=5;
```  

#### SELECT TOP Example
>
```SELECT TOP 2 * FROM Customers;```
"Customers"테이블에서 두 개의 첫 번째 레코드를 선택

#### SELECT TOP PERCENT Example
>
```SELECT TOP 50 PERCENT * FROM Customers;```
"Customers"테이블에서 레코드의 처음 50 %를 선택

## SQL LIKE Operator
>열의 지정된 패턴을 검색하는 데 사용됩니다.

### SQL LIKE Syntax
```
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;
```
### Example
>
```
SELECT * FROM Customers
WHERE City LIKE 's%';
```
> 도시가 문자 "s"로 시작하는 모든 고객을 선택  
> 
```
SELECT * FROM Customers
WHERE City LIKE '%s';
```
> 도시가 문자 "s"로 끝나는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE Country LIKE '%land%';
```
>"land"패턴이 포함 된 국가의 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE Country NOT LIKE '%land%';
```
> "land"패턴이 들어있는 국가가 아닌 모든 고객을 선택
> NOT 키워드를 사용하면 패턴과 일치하지 않는 레코드를 선택할 수 있습니다.
>
- "%"기호는 이전과 패턴 후 모두 와일드 카드 (누락 된 문자)를 정의하는 데 사용됩니다.

## SQL Wildcards
>
SQL에서 와일드 카드 문자는 SQL LIKE 연산자와 함께 사용됩니다.
>
SQL 와일드 카드는 테이블 내의 데이터를 검색하는 데 사용됩니다. 

##### % Wildcard
>
```
SELECT * FROM Customers
WHERE City LIKE 'ber%';
```
City가 "ber"로 시작하는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE City LIKE '%es%';
```
"es"패턴이 포함 된 City가있는 모든 고객을 선택

##### _ Wildcard
>
```
SELECT * FROM Customers
WHERE City LIKE '_erlin';
```
임의의 _자로 시작하여 "erlin"다음에 City가있는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE City LIKE '_erlin';
```
City가 "L"로 시작하고 모든 문자, 그 다음에 "n"다음에 문자가오고 "on"다음에 오는 모든 고객을 선택

##### [charlist] Wildcard
>
```
SELECT * FROM Customers
WHERE City LIKE '[bsp]%';
```
City가 "b", "s"또는 "p"로 시작하는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE City LIKE '[a-c]%';
```
City가 "b", "s"또는 "p"로 시작하는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE City LIKE '[!bsp]%';
```
City가 "a", "b"또는 "c"로 시작하는 모든 고객을 선택
>
```
SELECT * FROM Customers
WHERE City NOT LIKE '[bsp]%';
```
"b", "s"또는 "p"로 시작하는 도시가 아닌 모든 고객을 선택

## SQL IN Operator
>IN 연산자를 사용하여 WHERE 절에 여러 값을 지정할 수 있습니다.

### SQL IN Syntax

```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);
```
##### Example
>
```
SELECT * FROM Customers
WHERE City IN ('Paris','London');
```
> City가 "Paris"또는 "London"인 모든 고객을 선택

## SQL BETWEEN Operator
>
BETWEEN 연산자는 범위 내의 값을 선택합니다. 값은 숫자, 텍스트 또는 날짜 일 수 있습니다.

### SQL BETWEEN Syntax

```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
#### BETWEEN Operator Example

```
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;
```
>가격이 10과 20 사이 인 모든 제품을 선택

#### NOT BETWEEN Operator Example
```
SELECT * FROM Products
WHERE Price NOT BETWEEN 10 AND 20;
```
#### BETWEEN Operator with IN Example
```
SELECT * FROM Products
WHERE (Price BETWEEN 10 AND 20)
AND NOT CategoryID IN (1,2,3);
```
>가격이 10과 20 사이 인 모든 제품을 선택하지만 CategoryID가 1,2 또는 3 인 제품은 표시하지 않아야합니다.

#### BETWEEN Operator with Text Value Example
```
SELECT * FROM Products
WHERE ProductName BETWEEN 'C' AND 'M';
```
>ProductName이 BETWEEN 'C'및 'M'문자로 시작하는 모든 제품을 선택

#### NOT BETWEEN Operator with Text Value Example
```
SELECT * FROM Products
WHERE ProductName NOT BETWEEN 'C' AND 'M';
```
>NOT BETWEEN 'C'및 'M'문자로 시작하는 ProductName이있는 모든 제품을 선택

#### BETWEEN Operator with Date Value Example
```
SELECT * FROM Orders
WHERE OrderDate BETWEEN #07/04/1996# AND #07/09/1996#;
```
>OrderDate BETWEEN '04 -July-1996 '및 '09-Junly-1996'이있는 모든 주문을 선택

**BETWEEN 연산자는 다른 데이터베이스에서 다른 결과를 생성 할 수 있습니다.**

## SQL Aliases
>
SQL 별칭은 데이터베이스 테이블이나 테이블의 열에 임시 이름을 제공하는 데 사용됩니다.
>
기본적으로 앨리어스 (alias)가 생성되어 컬럼 이름을보다 쉽게 ​​읽을 수 있습니다.

### SQL Alias Syntax for Columns
```
SELECT column_name AS alias_name
FROM table_name;
```
### SQL Alias Syntax for Tables
```
SELECT column_name(s)
FROM table_name AS alias_name;
```
#### Alias Example for Table Columns
>
```
SELECT CustomerName AS Customer, ContactName AS [Contact Person]
FROM Customers;
```
>CustomerName 열과 ContactName 열의 두 가지 별칭을 지정합니다. 팁 : 열 이름에 공백이 포함 된 경우 큰 따옴표 또는 대괄호가 필요합니다
>
```
SELECT CustomerName, Address+', '+City+', '+PostalCode+', '+Country AS Address
FROM Customers;
```
>네 개의 열 (Address, City, PostalCode 및 Country)을 결합하고 "Address"라는 별칭을 만듭니다.


#### Alias Example for Tables
>
```
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;
```
>CustomerID = 4 (Around the Horn) 인 고객의 모든 주.을 선택합니다. "Customers"및 "Orders"테이블을 사용하고 각각 "c"및 "o"테이블 별명을 부여합니다 (여기서 별칭을 사용하여 SQL을 더 짧게 만듭니다).
>
```
SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM Customers, Orders
WHERE Customers.CustomerName="Around the Horn" AND Customers.CustomerID=Orders.CustomerID;
```
>별칭이없는 동일한 SQL 문

**별칭은 다음과 같은 경우에 유용합니다.**

- 쿼리에 두 개 이상의 테이블이 관련되어 있습니다.
- 함수는 쿼리에서 사용됩니다.
- 열 이름이 크거나 읽기가 쉽지 않습니다.
- 두 개 이상의 열이 결합됨

## SQL Joins
>두 개 이상의 테이블에서 행을 결합하는 데 사용됩니다.

#### Example
```
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID=Customers.CustomerID; #공통컬럼 일치시키기
```

## SQL INNER JOIN Keyword
>
두 테이블의 열이 일치하는 한 두 테이블의 모든 행을 선택

### SQL INNER JOIN Syntax

```
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;
```

<center>![](http://www.w3schools.com/sql/img_innerjoin.gif)</center>

#### Example
```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
INNER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```
>일치 하지 않는 값은 출력이 되지 않음.

## SQL LEFT JOIN Keyword
>왼쪽 테이블 (table1)의 모든 행과 오른쪽 테이블 (table2)의 일치하는 행을 반환합니다. 결과가 일치하지 않으면 오른쪽에 NULL입니다.

### SQL LEFT JOIN Syntax
```
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
```

<center>![](http://www.w3schools.com/sql/img_leftjoin.gif)</center>

#### Example
```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

## SQL RIGHT JOIN Keyword
>왼쪽 테이블 (table1)에 일치하는 행과 함께 오른쪽 테이블 (table2)의 모든 행을 리턴합니다. 결과가 일치하지 않으면 왼쪽에 NULL입니다.

### SQL RIGHT JOIN Syntax
```
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```
<center>![](http://www.w3schools.com/sql/img_rightjoin.gif)</center>
#### Example
```
SELECT Orders.OrderID, Employees.FirstName
FROM Orders
RIGHT JOIN Employees
ON Orders.EmployeeID=Employees.EmployeeID
ORDER BY Orders.OrderID;
```
## SQL FULL OUTER JOIN Keyword
>왼쪽 테이블 (table1)과 오른쪽 테이블 (table2)의 모든 행을 리턴

### SQL FULL OUTER JOIN Syntax
```
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;
```
<center>![](http://www.w3schools.com/sql/img_fulljoin.gif)</center>
#### Example
```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

## SQL UNION Operator
>
두 개 이상의 SELECT 문의 결과 집합을 결합  
각 SELECT 문은 동일한 수의 열을 가져야합니다.  
열은 유사한 데이터 유형을 가져야합니다.  
또한 각 SELECT.의 열은 동일한 순서 여야합니다.

### SQL UNION Syntax
```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```
>UNION는 기본적으로 별개의 값을 선택합니다. 중복 값을 허용하려면 UNION에 ALL 사용

### SQL UNION ALL Syntax
```
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```
#### UNION Example
```
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers
ORDER BY City;
```
>UNION은 고유한 값만 선택함. 중복값은 한번만 나열됨
>중복된 값까지 나열하려면 UNION ALL으로 처리 해야함.
#### UNION ALL Example
```
SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers
ORDER BY City;
```
#### UNION ALL With WHERE
```
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION ALL
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City;
```

## SQL SELECT INTO Statement
>SELECT INTO 문은 한 테이블의 데이터를 선택하여 새 테이블에 삽입합니다.

### SQL SELECT INTO Syntax
```
SELECT *
INTO newtable [IN externaldb]
FROM table1;
```
>모든 열을 새 테이블에 복사 할 수 있습니다.

```
SELECT column_name(s)
INTO newtable [IN externaldb]
FROM table1;
```
> 원하는 열만 새 테이블에 복사 할 수 있습니다.
>
>새 테이블은 SELECT 문에 정의 된대로 열 이름과 유형으로 작성됩니다. **AS**을 사용하여 새 이름을 적용 할 수 있습니다.

#### Examples
>
고객의 백업 복사본을 만듭니다.

```
SELECT *
INTO CustomersBackup2013
FROM Customers;
```
>
IN 절을 사용하여 테이블을 다른 데이터베이스로 복사하십시오.

```
SELECT *
INTO CustomersBackup2013 IN 'Backup.mdb'
FROM Customers;
```
>
새 테이블에 몇 개의 열만 복사하십시오.

```
SELECT CustomerName, ContactName
INTO CustomersBackup2013
FROM Customers;
```
>
독일 고객 만 새 테이블에 복사하십시오.

```
SELECT *
INTO CustomersBackup2013
FROM Customers
WHERE Country='Germany';
```
>
둘 이상의 테이블에서 새 테이블로 데이터 복사 :
```
SELECT Customers.CustomerName, Orders.OrderID
INTO CustomersOrderBackup2013
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID=Orders.CustomerID;
```
>
팁 : 문에 SELECT는 다른 사람의 스키마를 사용하여 비어있는 새 테이블을 만들 수 있습니다. 쿼리가 데이터를 반환하지 않게하는 WHERE 절을 추가하기 만하면됩니다.

```
SELECT *
INTO newtable
FROM table1
WHERE 1=0;
```
## SQL INSERT INTO SELECT Statement
>한 테이블에서 데이터를 선택하여 기존 테이블에 삽입

### SQL INSERT INTO SELECT Syntax
>
한 테이블에서 다른 테이블 (기존 테이블)으로 모든 열을 복사 할 수 있습니다.

```
INSERT INTO table2
SELECT * FROM table1;
```
>
또는 원하는 기존 열만 다른 기존 테이블로 복사 할 수 있습니다.

```
INSERT INTO table2 (column_name(s))
SELECT column_name(s)
FROM table1;
```



## SQL CREATE DATABASE Statement
>데이터베이스를 만드는 데 사용

### SQL CREATE DATABASE Syntax
```
CREATE DATABASE dbname;
```
## SQL CREATE TABLE Statement
>
데이터베이스에 테이블을 작성하는 데 사용됩니다.  
테이블은 행과 열로 구성됩니다.  
각 테이블에는 이름이 있어야합니다.

### SQL CREATE TABLE Syntax
```
CREATE TABLE table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
....
);
```
#### Example
```
CREATE TABLE Persons
(
PersonID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```
>빈 테이블이 문 INTO 삽입하여 데이터로 채워질 수있다.

## SQL Constraints
>테이블의 데이터에 대한 규칙을 지정하는 데 사용
>제한 조건과 데이터 조치 사이에 위반이 있으면, 제한 조건에 의해 조치가 중단

### SQL CREATE TABLE + CONSTRAINT Syntax
```
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

- NOT NULL : 열이 NULL 값을 저장할 수 없음을 나타냅니다
- UNIQUE : 열의 각 행은 고유 한 값을 가져야한다는 보장
- PRIMARY KEY : NULL과 UNIQUE NOT의 조합. 열 (또는 두 개 이상의 열 조합)이 테이블의 특정 레코드를보다 쉽고 빠르게 찾는 데 도움이되는 고유 한 ID를 갖도록 보장합니다.
- FOREIGN KEY : 다른 테이블의 값과 일치하는 하나의 테이블에서 데이터의 참조 무결성을 보장합니다
- CHECK : 컬럼의 값이 특정 조건을 충족 보장
- DEFAULT : 열에 대한 디폴트 값을 지정합니다

>
>-
### SQL NOT NULL Constraint
>기본적으로 테이블 열은 NULL 값을 포함 할 수 있음  
>NOT NULL은 열이 NULL 값을 허용하지 않도록합니다.  
>NOT NULL은 필드가 항상 값을 포함하도록합니다.  
>즉, 새 레코드를 삽입하거나이 필드에 값을 추가하지 않고 레코드를 업데이트 할 수 없습니다.
>
#### Example
```
CREATE TABLE PersonsNotNull
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
-
### SQL UNIQUE Constraint
>UNIQUE은 데이터베이스 테이블의 각 레코드를 고유하게 식별합니다.  
UNIQUE 및 PRIMARY KEY은 둘 다 열 또는 열 집합의 고유성을 보장합니다.  
PRIMARY KEY에는 자동으로 UNIQUE이 정의되어 있습니다.  
테이블 당 UNIQUE을 많이 가질 수 있지만 테이블 당 하나의 PRIMARY KEY만 가질 수 있습니다.
#### SQL UNIQUE Constraint on CREATE TABLE
```
CREATE TABLE Persons
(
P_Id int NOT NULL UNIQUE,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
UNIQUE의 이름 지정을 허용하고 여러 열에 UNIQUE을 정의하려면 다음 SQL 구문을 사용합니다.
```
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
)
```
#### SQL UNIQUE Constraint on ALTER TABLE
```
ALTER TABLE Persons
ADD UNIQUE (P_Id)
```
UNIQUE 제약 조건의 이름 지정을 허용하고 여러 열에 UNIQUE 제약 조건을 정의하려면 다음 SQL 구문을 사용합니다.
>
```
ALTER TABLE Persons
ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
```
#### To DROP a UNIQUE Constraint
```
ALTER TABLE Persons
DROP CONSTRAINT uc_PersonID
```
-
### SQL PRIMARY KEY Constraint
PRIMARY KEY은 데이터베이스 테이블의 각 레코드를 고유하게 식별합니다.  
기본 키에는 UNIQUE 값이 있어야합니다.  
기본 키 열은 NULL 값을 포함 할 수 없습니다.  
대부분의 테이블에는 기본 키가 있어야하며 각 테이블에는 기본 키가 하나만있을 수 있습니다.
>
#### SQL PRIMARY KEY Constraint on CREATE TABLE
```
CREATE TABLE Persons
(
P_Id int NOT NULL PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
PRIMARY KEY의 이름 지정 및 여러 열의 PRIMARY KEY를 허용하려면 다음 SQL 구문을 사용하십시오.
>
```
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
)
```
#### SQL PRIMARY KEY Constraint on ALTER TABLE
```
ALTER TABLE Persons
ADD PRIMARY KEY (P_Id)
```
PRIMARY KEY의 이름 지정 및 여러 열의 PRIMARY KEY 정의를 허용하려면 다음 SQL 구문을 사용하십시오.
```
ALTER TABLE Persons
ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
```
기본 키를 추가하려면 ALTER TABLE 문을 사용하는 경우, 기본 키 열 (들) 이미 (테이블이 처음 생성 될 때) NULL 값을 포함하지 않는 선언되어 있어야합니다.
#### To DROP a PRIMARY KEY Constraint
```
ALTER TABLE Persons
DROP CONSTRAINT pk_PersonID
```
-
### SQL FOREIGN KEY Constraint
한 테이블의 외부 키는 다른 테이블의 PRIMARY KEY를 가리 킵니다.
FOREIGN KEY은 테이블 간의 연결을 파괴하는 작업을 방지하는 데 사용.
또한 FOREIGN KEY은 잘못된 데이터가 가리키는 테이블에 포함 된 값 중 하나 여야하므로 잘못된 데이터가 외부 키 열에 삽입되는 것을 방지.
#### SQL FOREIGN KEY Constraint on CREATE TABLE
```
CREATE TABLE Orders
(
O_Id int NOT NULL PRIMARY KEY,
OrderNo int NOT NULL,
P_Id int FOREIGN KEY REFERENCES Persons(P_Id)
)
```
FOREIGN KEY 제약 조건의 이름 지정을 허용하고 여러 열에 FOREIGN KEY 제약 조건을 정의하려면 다음 SQL 구문을 사용하십시오.
```
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
PRIMARY KEY (O_Id),
CONSTRAINT fk_PerOrders FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
)
```
#### SQL FOREIGN KEY Constraint on ALTER TABLE
```
ALTER TABLE Orders
ADD FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
```
FOREIGN KEY 제약 조건의 이름 지정을 허용하고 여러 열에 FOREIGN KEY 제약 조건을 정의하려면 다음 SQL 구문을 사용하십시오.
```
ALTER TABLE Orders
ADD CONSTRAINT fk_PerOrders
FOREIGN KEY (P_Id)
REFERENCES Persons(P_Id)
```
#### To DROP a FOREIGN KEY Constraint
```
ALTER TABLE Orders
DROP CONSTRAINT fk_PerOrders
```
-
### SQL CHECK Constraint
CHECK은 열에 배치 할 수있는 값 범위를 제한하는 데 사용됩니다.  
단일 열에 CHECK을 정의하면이 열에 대해 특정 값만 허용됩니다.  
표에 CHECK을 정의하면 행의 다른 컬럼의 값에 따라 특정 열의 값을 제한 할 수 있습니다.
#### SQL CHECK Constraint on CREATE TABLE
```
CREATE TABLE Persons
(
P_Id int NOT NULL CHECK (P_Id>0),
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
CHECK 제약 조건의 이름 지정을 허용하고 여러 열에 CHECK 제약 조건을 정의하려면 다음 SQL 구문을 사용합니다.
```
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
)
```
#### SQL CHECK Constraint on ALTER TABLE
```
ALTER TABLE Persons
ADD CHECK (P_Id>0)
```
CHECK의 이름 지정을 허용하고 여러 열에 CHECK을 정의하려면 다음 SQL 구문을 사용합니다.
```
ALTER TABLE Persons
ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```
#### To DROP a CHECK Constraint
```
ALTER TABLE Persons
DROP CONSTRAINT chk_Person
```
-
### SQL DEFAULT Constraint
DEFAULT은 열에 기본값을 삽입하는 데 사용됩니다.
다른 값을 지정하지 않으면 기본값이 모든 새 레코드에 추가됩니다.
#### SQL DEFAULT Constraint on CREATE TABLE
```
CREATE TABLE Persons
(
P_Id int NOT NULL,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255) DEFAULT 'Sandnes'
)
```
GETDATE ()와 같은 함수를 사용하여 DEFAULT을 사용하여 시스템 값을 삽입 할 수도 있습니다.
>
```
CREATE TABLE Orders
(
O_Id int NOT NULL,
OrderNo int NOT NULL,
P_Id int,
OrderDate date DEFAULT GETDATE()
)
```
#### SQL DEFAULT Constraint on ALTER TABLE
```
ALTER TABLE Persons
ALTER COLUMN City SET DEFAULT 'SANDNES'
```
#### To DROP a DEFAULT Constraint
```
ALTER TABLE Persons
ALTER COLUMN City DROP DEFAULT
```

## SQL CREATE INDEX Statement
>인덱스를 사용하면 데이터베이스 응용 프로그램에서 데이터를 빠르게 찾을 수 있습니다.  
>사용자는 색인을 볼 수 없으며 검색 / 쿼리 속도를 높이기 위해 사용  
>인덱스 테이블을 업데이트없이 테이블을 업데이트하는 것보다 더 많은 시간이 걸립니다.  
>(인덱스도 업데이트를해야하기 때문에). 따라서 자주 검색 할 열 (및 테이블)에 대해서만 인덱스를 만들어야합니다.
### SQL CREATE INDEX Syntax
```
CREATE INDEX index_name
ON table_name (column_name)
```
### SQL CREATE UNIQUE INDEX Syntax
```
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```
#### Example
```
CREATE INDEX PIndex
ON Persons (LastName, FirstName)
```

## SQL DROP INDEX, DROP TABLE, and DROP DATABASE
>인덱스, 테이블 및 데이터베이스는 DROP 문을 사용하여 쉽게 삭제 / 제거 할 수 있습니다.
### The DROP INDEX Syntax
```
DROP INDEX table_name.index_name
```
### The DROP TABLE Statement
```
DROP TABLE table_name
```
### The DROP DATABASE Statement
```
DROP DATABASE database_name
```
### The TRUNCATE TABLE Statement
```
TRUNCATE TABLE table_name
```

## SQL ALTER TABLE Statement
>ALTER TABLE은 기존 테이블의 열을 추가, 삭제 또는 수정하는 데 사용
### SQL ALTER TABLE Syntax
```
ALTER TABLE table_name
ADD column_name datatype
```
테이블의 열을 삭제하려면 다음 구문을 사용하십시오. 일부 데이터베이스 시스템에서는 열 삭제가 허용되지 않습니다.
```
ALTER TABLE table_name
DROP COLUMN column_name
```
테이블의 열의 데이터 형식을 변경하려면 다음 구문을 사용합니다.
```
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

## SQL AUTO INCREMENT Field
>새 레코드가 테이블에 삽입 될 때 고유 번호가 생성되도록합니다.

### Syntax
```
CREATE TABLE Persons
(
ID int IDENTITY(1,1) PRIMARY KEY,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255)
)
```
## SQL Views
>SQL에서 뷰는 SQL 문의 결과 세트를 기반으로하는 가상 테이블입니다.  
뷰에는 실제 테이블과 마찬가지로 행과 열이 포함됩니다.  
SQL 함수, WHERE 및 JOIN 문을 뷰에 추가하고 데이터가 하나의 단일 테이블에서 오는 것처럼 데이터를 표시 할 수 있습니다.
### SQL CREATE VIEW Syntax
```
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```
### SQL Updating a View
```
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```
이제 "현재 제품 목록"보기에 "범주"열을 추가하려고합니다. 다음 SQL로 뷰를 업데이트합니다.
>
```
CREATE OR REPLACE VIEW [Current Product List] AS
SELECT ProductID,ProductName,Category
FROM Products
WHERE Discontinued=No
```
### SQL Dropping a View
```
DROP VIEW view_name
```

## SQL Date Functions
>**날짜 작업시 가장 어려운 부분은 삽입하려는 날짜의 형식이 데이터베이스의 날짜 열 형식과 일치하는지 확인하는 것입니다.**
>데이터에 날짜 부분 만 포함되어 있으면 쿼리가 예상대로 작동합니다.  
>그러나 시간 부분이 복잡하면 복잡해집니다.
>
Function	|Description
---|---
GETDATE()|	Returns the current date and time
DATEPART()|	Returns a single part of a date/time
DATEADD()|	Adds or subtracts a specified time interval from a date
DATEDIFF()|	Returns the time between two dates
CONVERT()|	Displays date/time data in different formats
### SQL Date Data Types
- DATE - format YYYY-MM-DD
- DATETIME - format: YYYY-MM-DD HH:MI:SS
- SMALLDATETIME - format: YYYY-MM-DD HH:MI:SS
- TIMESTAMP - format: a unique number

## SQL NULL Values
>NULL 값은 알 수없는 데이터가 누락되었음을 나타냅니다.  
>table 열이 옵션의 경우이 열에 값을 추가하지 않고 새로운 레코드를 삽입하거나 기존 레코드를 업데이트 할 수 있습니다. 이것은 필드가 NULL 값으로 저장되는 것을 의미합니다.

## SQL ISNULL(), NVL(), IFNULL() and COALESCE() Functions
```
SELECT ProductName,UnitPrice*(UnitsInStock+ISNULL(UnitsOnOrder,0))
FROM Products
```
## SQL General Data Types
>데이터베이스 테이블의 각 열에는 이름과 데이터 유형이 있어야합니다.  
SQL 개발자는 SQL 테이블을 작성할 때 각 테이블 열에 저장된 데이터 유형을 결정해야합니다.  
데이터 유형은 각 컬럼 내부에서 예상되는 데이터 유형을 이해하기위한 SQL의 지침 및 레이블이며 SQL이 저장된 데이터와 상호 작용하는 방법을 식별합니다.


Data type | Description
---|---
CHARACTER(n)|	Character string. Fixed-length n
VARCHAR(n) or CHARACTER VARYING(n)|	Character string. Variable length. Maximum length n
BINARY(n)|	Binary string. Fixed-length n 
BOOLEAN|	Stores TRUE or FALSE values
VARBINARY(n) or BINARY VARYING(n)|	Binary string. Variable length. Maximum length n
INTEGER(p)|	Integer numerical (no decimal). Precision p
SMALLINT|	Integer numerical (no decimal). Precision 5
INTEGER|	Integer numerical (no decimal). Precision 10
BIGINT|	Integer numerical (no decimal). Precision 19
DECIMAL(p,s)|	Exact numerical, precision p, scale s. Example: decimal(5,2)| is a number that has 3 digits before the decimal and 2 digits after the decimal
NUMERIC(p,s)|	Exact numerical, precision p, scale s. (Same as DECIMAL)
FLOAT(p)|	Approximate numerical, mantissa precision p. A floating number in base 10 exponential notation. The size argument for this type consists of a single number specifying the minimum precision
REAL|	Approximate numerical, mantissa precision 7
FLOAT|	Approximate numerical, mantissa precision 16
DOUBLE PRECISION|	Approximate numerical, mantissa precision 16
DATE|	Stores year, month, and day values
TIME|	Stores hour, minute, and second values
TIMESTAMP|	Stores year, month, day, hour, minute, and second values
INTERVAL|	Composed of a number of integer fields, representing a period of time, depending on the type of interval
ARRAY|	A set-length and ordered collection of elements
MULTISET|	A variable-length and unordered collection of elements
XML| Stores XML data

### SQL Data Type Quick Reference

Data type|Access|SQLServer|Oracle|MySQL|
---|---|---|---|---
boolean|Yes/No|Bit|Byte|N/A|Boolean
integer|Number(integer)|Int|Number|Int Integer
float|	Number (single)|	Float Real|Number|Float
currency|Currency|Money|N/A|N/A
string (fixed)|	N/A|	Char|	Char|	Char
string (variable)|Text (<256) Memo (65k+)|Varchar|Varchar Varchar2|Varchar
binary object|OLE Object Memo|Binary (fixed up to 8K) Varbinary (<8K) Image (<2GB)|	Long Raw|Blob Text

## SQL Data Types for Various DBs
>
#### String types:
Data type|Description|Storage
---|---|---
char(n)|	Fixed width character string. Maximum 8,000 characters|	Defined width
varchar(n)|	Variable width character string. Maximum 8,000 characters|	2 bytes + number of chars
varchar(max)|	Variable width character string. Maximum 1,073,741,824 characters|	2 bytes + number of chars
text|	Variable width character string. Maximum 2GB of text data|	4 bytes + number of chars
nchar|	Fixed width Unicode string. Maximum 4,000 characters|	Defined width x 2
nvarchar|	Variable width Unicode string. Maximum 4,000 characters	 
nvarchar(max)|	Variable width Unicode string. Maximum 536,870,912 characters	 
ntext|	Variable width Unicode string. Maximum 2GB of text data	 
bit|	Allows 0, 1, or NULL	 
binary(n)|	Fixed width binary string. Maximum 8,000 bytes	 
varbinary|	Variable width binary string. Maximum 8,000 bytes	 
varbinary(max)|	Variable width binary string. Maximum 2GB	 
image|	Variable width binary string. Maximum 2GB
#### Number types:
Data type|	Description|	Storage
---|---|---
tinyint|	Allows whole numbers from 0 to 255	|1 byte
smallint|	Allows whole numbers between -32,768 and 32,767|	2 bytes
int	|Allows whole numbers between -2,147,483,648 and 2,147,483,647|	4 bytes
bigint|	Allows whole numbers between -9,223,372,036,854,775,808 and 9,223,372,036,854,775,807|	8 bytes
decimal(p,s)|	Fixed precision and scale numbers. Allows numbers from -10^38 +1 to 10^38 –1. The p parameter indicates the maximum total number of digits that can be stored (both to the left and to the right of the decimal point). p must be a value from 1 to 38. Default is 18. The s parameter indicates the maximum number of digits stored to the right of the decimal point. s must be a value from 0 to p. Default value is 0|5-17 bytes
numeric(p,s)|	Fixed precision and scale numbers. Allows numbers from -10^38 +1 to 10^38 –1. The p parameter indicates the maximum total number of digits that can be stored (both to the left and to the right of the decimal point). p must be a value from 1 to 38. Default is 18. The s parameter indicates the maximum number of digits stored to the right of the decimal point. s must be a value from 0 to p. Default value is 0|5-17 bytes
smallmoney|Monetary data from -214,748.3648 to 214,748.3647|	4 bytes
money|Monetary data from -922,337,203,685,477.5808 to 922,337,203,685,477.5807|8 bytes
float(n)|Floating precision number data from -1.79E + 308 to 1.79E + 308. The n parameter indicates whether the field should hold 4 or 8 bytes. float(24) holds a 4-byte field and float(53) holds an 8-byte field. Default value of n is 53.|4 or 8 bytes
real|	Floating precision number data from -3.40E + 38 to 3.40E + 38|	4 bytes
#### Date types:
Data type|	Description|	Storage
---|---|---
datetime|	From January 1, 1753 to December 31, 9999 with an accuracy of 3.33 milliseconds	|8 bytes
datetime2|	From January 1, 0001 to December 31, 9999 with an accuracy of 100 nanoseconds	|6-8 bytes
smalldatetime|	From January 1, 1900 to June 6, 2079 with an accuracy of 1 minute|	4 bytes
date|	Store a date only. From January 1, 0001 to December 31, 9999	|3 bytes
time|	Store a time only to an accuracy of 100 nanoseconds	|3-5 bytes
datetimeoffset|	The same as datetime2 with the addition of a time zone offset	|8-10 bytes
timestamp|	Stores a unique number that gets updated every time a row gets created or modified. The timestamp value is based upon an internal clock and does not correspond to real time. Each table may have only one timestamp variable
#### Other data types:
Data type|	Description
---|---
sql_variant|	Stores up to 8,000 bytes of data of various data types, except text, ntext, and timestamp
uniqueidentifier|	Stores a globally unique identifier (GUID)
xml|	Stores XML formatted data. Maximum 2GB
cursor|	Stores a reference to a cursor used for database operations
table|	Stores a result-set for later processing

## SQL Comments
>주석을 사용하여 SQL 문의 섹션을 설명 할 수 있습니다.
### Single Line Comments
```
--Select all:
SELECT * FROM Customers;
```
```
SELECT * FROM Customers -- WHERE City='Berlin';
```
```
--SELECT * FROM Customers;
SELECT * FROM Products;
```
### Multi-line Comments
```
/*Select all the columns
of all the records
in the Customers table:*/
SELECT * FROM Customers;
```
```
/*SELECT * FROM Customers;
SELECT * FROM Products;
SELECT * FROM Orders;
SELECT * FROM Categories;*/
SELECT * FROM Suppliers;
```
### Comments in Statements
```
SELECT CustomerName, /*City,*/ Country FROM Customers;
```
```
SELECT * FROM Customers WHERE (CustomerName LIKE 'L%'
OR CustomerName LIKE 'R%' /*OR CustomerName LIKE 'S%'
OR CustomerName LIKE 'T%'*/ OR CustomerName LIKE 'W%')
AND Country='USA'
ORDER BY CustomerName;
```

# SQL Functions
>SQL은 데이터에 대한 계산을 수행하는 많은 내장 함수를 가지고 있습니다.
### SQL Aggregate Functions
>열의 값에서 계산 된 단일 값을 반환합니다
>
- AVG () - 평균값을 반환합니다.
- COUNT () - 행 수를 반환합니다.
- FIRST () - 첫 번째 값을 반환합니다.
- LAST () - 마지막 값을 반환합니다.
- MAX () - 가장 큰 값을 반환합니다.
- MIN () - 가장 작은 값을 반환합니다.
- SUM () - 합계를 반환합니다.
>
### SQL Scalar functions
>입력 값을 기반으로 단일 값을 리턴합니다.
>
- UCASE () - 필드를 대문자로 변환합니다.
- LCASE () - 필드를 소문자로 변환합니다.
- MID () - 텍스트 필드에서 문자 추출
- LEN () - 텍스트 필드의 길이를 반환합니다.
- ROUND () - 숫자 필드를 지정된 소수 자릿수로 반올림합니다.
- NOW () - 현재 시스템 날짜와 시간을 반환합니다.
- FORMAT () - 필드가 표시되는 방식을 지정합니다.

## SQL AVG() Function
>숫자 열의 평균값을 반환
### SQL AVG() Syntax
```
SELECT AVG(column_name) FROM table_name
```
ß
## SQL COUNT() Function
>지정된 기준과 일치하는 행 수를 반환
### SQL COUNT(column_name) Syntax
```
SELECT COUNT(column_name) FROM table_name;
```
>>지정된 열의 값 수 (NULL 값은 계산되지 않음)를 반환
>
### SQL COUNT(*) Syntax
```
SELECT COUNT(*) FROM table_name;
```
>>테이블의 레코드 수를 반환
>
### SQL COUNT(DISTINCT column_name) Syntax
```
SELECT COUNT(DISTINCT column_name) FROM table_name;
```
>>지정된 열의 고유 값 수를 반환

## SQL FIRST() Function
>선택한 열의 첫 번째 값을 반환
### SQL FIRST() Syntax
```
SELECT FIRST(column_name) FROM table_name;
```

## The LAST() Function
>선택한 열의 마지막 값을 반환
### SQL LAST() Syntax
```
SELECT LAST(column_name) FROM table_name;
```

## SQL MAX() Function
>선택된 열의 가장 큰 값을 반환
### SQL MAX() Syntax
```
SELECT MAX(column_name) FROM table_name;
```

## SQL MIN() Function
>선택된 열의 가장 작은 값을 반환
### SQL MIN() Syntax
```
SELECT MIN(column_name) FROM table_name;
```

## SQL SUM() Function
>숫자 열의 총 합계를 반환
### SQL SUM() Syntax
```
SELECT SUM(column_name) FROM table_name;
```

## SQL GROUP BY Statement
>### SQL GROUP BY Syntax
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

## SQL HAVING Clause
>
집계 함수에서 WHERE 키워드를 사용할 수 없으므로 HAVING 절이 SQL에 추가되었습니다.
### SQL HAVING Syntax
```
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

## SQL UCASE() Function
>필드 값을 대문자로 변환
### SQL UCASE() Syntax
```
SELECT UCASE(column_name) FROM table_name;
```

## SQL LCASE() Function
>필드 값을 소문자로 변환
### SQL LCASE() Syntax
```
SELECT LCASE(column_name) FROM table_name;
```

## SQL MID() Function
>텍스트 필드에서 문자를 추출
### SQL MID() Syntax
```
SELECT MID(column_name,start,length) AS some_name FROM table_name;
```
>
Parameter|	Description
---|---
column_name|필수. 문자를 추출하는 필드
start|	필수. 시작 위치를 지정합니다 (1부터 시작)
length|어떤. 반환되는 문자. 생략하면 MID () 함수는 나머지 텍스트를 돌려줍니다

## SQL LEN() Function
>텍스트 필드에 값의 길이를 반환
### SQL LEN() Syntax
```
SELECT LEN(column_name) FROM table_name;
```

## SQL ROUND() Function
> 숫자 필드를 지정된 소수 자리로 반올림
### SQL ROUND() Syntax
```
SELECT ROUND(column_name,decimals) FROM table_name;
```
Parameter|	Description
---|---
column_name|	필수. 반올림 필드.
decimals|	필수. 반환되는 소수점 이하 자릿수를 지정합니다.

## SQL NOW() Function
>현재 시스템 날짜와 시간을 반환
### SQL NOW() Syntax
```
SELECT NOW() FROM table_name;
```

## SQL FORMAT() Function
>필드 표시 방법을 형식화하는 데 사용
### SQL FORMAT() Syntax
```
SELECT FORMAT(column_name,format) FROM table_name;
```
Parameter|	Description
---|---
column_name|	필수. 서식 필드
format|필수. 형식을 지정합니다.



