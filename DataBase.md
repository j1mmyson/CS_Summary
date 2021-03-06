
# MySQL 설치부터 CRUD까지

> 최근 Go언어로 블록체인을 구현하는걸 시도해보고 있는데 내가 생성한 블록을 저장할 데이터베이스가 있어야 한다는 것을 알았고, 데이터베이스에 대해 무지한 나를 발견했다..ㅋㅋ   
> 그래서 데이터베이스의 종류중에 가장 많이 쓰이는 관계형 데이터베이스, 그 중 가장 많이 쓰이는 것 중 하나인 MySQL을 알아보기로 했고 생활코딩 이고잉님의 강의를 보며 내용을 정리해보았다.

<br>

## 설치 (Linux)

1. `$ sudo apt-get update`
2. `$ sudo apt-get install mysql-server`

<br>   

## 실행

1. `$ sudo service mysql start`로 서버를 켜고,

2. `$ sudo mysql -u root -p`를 통해 root 아이디로 mysql server에 접속한다.

   `-u :user` , `-p: password`를 의미한다

3. 초기 비밀번호는 없다. 그냥 엔터한번 쳐주면 된다. 그러면

   ``` mysql
   mysql>
   ```

   과 같은 행이 나오면 성공적으로 설치가 된 것이다..!

<br>

## 비밀번호 설정

```mysql
SET PASSWORD = PASSWORD('<your_password>');
```

리눅스에서 처음 설치하면 비밀번호가 설정되어있지 않아 그냥 엔터만 치고 들어왔는데
위 명령어로 비밀번호를 설정해줄 수 있다.   

`mysql> quit` 하고 다시 `$ sudo mysql -u root -p`로 mysql을 켜주면 방금 설정한 비밀번호를 입력해야만 들어갈 수 있는걸 확인할수 있겠다.

<br>  

## DataBase(Schema)

### 데이터베이스 생성

  ``` mysql
  CREATE DATABASE <DB_NAME>;
  ```

### 데이터베이스 삭제

  ```mysql
  DROP DATABASE <DB_NAME>;
  ```

### 데이터베이스 조회

  ```mysql
  SHOW DATABASES;
  ```

### 데이터베이스 접근
  : 데이터베이스에 접근하여 SQL언어를 통해 데이터베이스 서버에 명령을 내릴 수 있다.

  ```mysql
  USE <DB_NAME>;
  ```

<br>

## TABLE (표)

### CREATE

```mysql
CREATE TABLE topic(
    id INT(11) NOT NULL AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    description TEXT NULL,
    created DATETIME NOT NULL,
    author VARCHAR(30) NULL,
    profile VARCHAR(100) NULL,
    PRIMARY KEY(id)
);
```

- `NULL`: NULL값도 허용하겠다.
- `NOT NULL`: NULL값이 들어오는걸 허용하지 않겠다.
- `INT(11)`: 자릿수가 최대 11인 int형 데이터만 받겠다.
- `AUTO_INCREMENT`: 행이 추가될때마다 자동으로 1씩 증가시키겠다.
- `PRIMARY KEY(id)`: id를 primary key로 설정하겠다.
  (`primary key`는 중복될 수 없는 유일한 값을 가져야 한다.)

### SHOW

```mysql
SHOW COLUMNS FROM topic;
```

```mysql
DESC topic;
```

생성된 테이블에 대한 정보를 한눈에 보고싶다면 위 명령어를 입력해주면 아래처럼 보기좋게 나오는걸 볼 수 있다. bb

```bash
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| title       | varchar(100) | NO   |     | NULL    |                |
| description | text         | YES  |     | NULL    |                |
| created     | datetime     | NO   |     | NULL    |                |
| author      | varchar(30)  | YES  |     | NULL    |                |
| profile     | varchar(100) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
```

현재 접속(?)한 데이터베이스에 있는 테이블 목록을 보고싶으면 데이터베이스 서버에서 데이터베이스 목록을 조회했던 것과 같이 `SHOW TABLES` 해주면 된다.

```mysql
SHOW TABLES;
```

<br>  

## CRUD

이제 데이터베이스의 꽃(?)이라고 볼 수 있는 CRUD를 해보자.   
CRUD란 Create Read Update Delete 를 의미한다.   

### CREATE

`INSERT INTO` 명령어를 통해 내가 원하는 테이블에 데이터를 입력할 수 있다.
```mysql
INSERT INTO topic (title, description, created, author, profile) VALUES ("MySQL", "MySQL is ...", NOW(), "byungwook", "developer");
```

### READ

`SELECT` 명령어를 통해 `topic` 테이블에 있는 모든 열에 대한 데이터를 읽어올 수 있다. 

```mysql
SELECT * FROM topic;
```

이런식으로 특정 열 혹은 특정 조건을 만족하는 데이터만 읽어올 수도 있다.

```mysql
# 특정 열만 읽기.
SELECT id, title FROM topic;

# 특정 조건을 만족하는 데이터만 읽기.
SELECT * FROM topic WHERE id > 2;

# 이런식으로 저자가 byungwook인 데이터의 title만 읽어오는 식으로 응용 가능!
SELECT title FROM topic WHERE author = "byungwook";
```

### UPDATE

`UPDATE` 명령어로 특정 조건을 만족하는 데이터를 수정할 수 있다.

```mysql
# topic 테이블에서 id가 2인 데이터의 description을 "Oracle is ..."로, title을 "ORACLE"로 수정하겠다.
UPDATE topic SET description= "Oracle is ...", title = "ORACLE" WHERE id = 2;
```

### DELETE

`DELETE`명령어를 사용할 때 `WHERE`키워드를 써주지 않으면 모든 데이터가 날라가니 주의하자..!

```mysql
DELETE FROM topic WHERE id = 1;
```

