# SQL Note
## KeyWord整理
**CREATE DATABASE** databaseName(建立新的資料庫)

**SHOW DATABASES** databaseName （查看所有資料庫）

**DROP DATABASE** databaseName（刪除一個資料庫）

**USE** databaseName（使用一個資料庫）

**CREATE TABLE** tableName（建立一個表格）
```sql
CREATE TABLE `employee`(
	`emp_id` INT PRIMARY KEY,
    `name` VARCHAR(20),
    `birth_date` DATE,
    `sex` VARCHAR(1),
    `salary` INT,
    `branch_id` INT,
    `sup_id` INT
);
```

**DESCRIBE** tableName(列出表格資訊)
僅會顯示該表格的設定，內容不會顯示

**ALTER TABLE** tableName(
修改表格資訊，例如設定FOREIGN KEY，但不能修改資料)


**UPDATE** tableName （修改指定表格內的資料）

**SELECT** **DISTINCT** (撈出不重複的資料）)




**INSERT** **INTO** tableName **VALUES**(對指定表格插入一筆新的資料)

# 設定主鍵(PARIMARY KEY)
```sql
CREATE TABLE `branch`(
    `branch_id` INT PRIMARY KEY,
    `branch_name` VARCHAR(20),
    `manager_id` INT,
    FOREIGN KEY(`manager_id`) REFERENCES `employee`(`emp_id`) ON DELETE SET NULL
);
```
1. 在創立表格時，可以直接在屬性後面加上 PRIMARY KEY來設定主鍵
2. 可以在PRIMARY KEY 後面加上 AUTO_INCREMENT，可使主鍵自動+1
3. 一個表格若只有一個主鍵，則主鍵只能為UNIQUE數，但若主鍵為複數，則數字可以重複。


# 設定FOREIGN KEY
```sql
ALTER TABLE `employee`
ADD FOREIGN KEY (`branch_id`)
REFERENCES `branch`(`branch_id`)
ON DELETE SET NULL;
```
1.可使用ALTER TALBE 修改表格屬性，並設定FOREIGN KEY
2.需要設定若FOREIGN KEY被刪除後的後續動作
3.範例顯示被刪除後，會將其連結的資料的欄位設定為NULL。




# UNION 連集
```sql
SELECT `name`  AS `new_name` FROM `employee`
UNION
SELECT `client_name` FROM `client`;
```
1. 可以將不同表個的資料結合後回傳
2. 回傳的資料屬性需要相同
3. 可以串接多個表格
4. 將以第一個搜尋的屬性作為標題，若欲更改可使用AS 語法更改為其他名字。


# JOIN 連接
```sql
SELECT `emp_id`,`name`,`branch_name` FROM `employee`
LEFT JOIN `branch`
ON `employee`.`emp_id`=`manager_id`; 
```
1. 將多個表格的資料串接。
2. 單使用JOIN時，左右兩邊的表格需要同時滿足條件才會回傳資料
3. 如果使用LEFT JOIN，則左邊表格無論是否滿足條件都會回傳資料
4. 同上，另有RIGHT JOIN的功能可以使用。


# SUBQUERY （子查詢）
```sql
SELECT `name` 
FROM `employee`
WHERE `emp_id` IN (
	SELECT `emp_id`
	FROM `work_with`
	WHERE `total_sales` > 50000
);
```
1. 可於WHERE指定式中寫入另一搜尋式
2. 若子查詢回傳的值>1，則需使用IN連結，否則會出錯
3. 若子查詢回傳的值=1,則可以直接使用等號來表示。


# 聚合函數(Aggregation function)
```sql
SELECT COUNT(*) FROM `employee`;
SELECT MAX(`salary`) FROM `employee`;
```
可以使用內建函數來篩選資料，看起來跟EXCEL的函數使用方式差不多。

## LENGTH(colum_name) 計算字元長度
```sql
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) ASC, CITY ASC LIMIT 1;
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) DESC, CITY DESC LIMIT 1;
```
* 篩選STATIO表格中的CITY欄位，以及計算長度
* 並以CITY字串長度排序，再以CITY進行排序。
* 取升冪排序第一，降冪排序第一。

## REGEXP_LIKE(colum_name,'REGEX') 
```sql
SELECT * FROM employees WHERE REGEXP_LIKE(city, '[b-d]'); 
```

# 新增/刪除欄位
* ALTER TABLE table_name DROP COLUMN column_name;
* ADD COLUMN column_name1 column_definition [FIRST|AFTER existing_column]





# 基礎資料類型

1. INT (整數)
2. CHAR (1~255字元字串)
3. VARCHAR (不超過255字元不定長度字串)
4. TEXT (不定長度字串最多65535字元) 

