---
layout: post
title:  "DataBase Index - Mysql"
createdDate:   2022-04-30T08:27:00+09:00
date:   2022-04-30T08:27:00+09:00
pagination: enabled
author: SoonYong Hong
categories: DataBase
tags: DataBase Mysql Index DBMS
---

# Index

## 목차

1. 인덱스 기본
2. Clustered Index & Nonclustered Index
3. InnoDB Index
4. 인덱스 활용하기

## 인덱스 기본

대부분의 DBMS에서 인덱스는 B-Tree 구조를 가지고 있다.

트리의 각 노드에는 인덱스 키와 자식노드의 주소를 key:value 셋의 형태로 가지고 있으며 마지막 리프노드에는 인덱스 키와 데이터 주소를 가지고 있게된다.

각 노드에는 같은 레벨의 다음노드와 이전노드를 오갈수 있는 링크를 가지고 있다.

인덱스를 이용한 레코드 검색은 루트노드부터 리프노드까지 해당하는 키를 찾아서 탐색하여 리프노드에 도달한뒤 획득한 데이터 주소를 이용하여 row 전체를 획득한다.

<table>
<thead style="background-color: #d8dfec"><tr><td>루트 노드</td><td>브랜치 노드</td><td>리프 노드</td></tr></thead>
<tbody>
<tr>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>1</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>2</td></tr><tr><td>Jebra</td><td>3</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>2</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>4</td></tr><tr><td>Egg</td><td>5</td></tr><tr><td>Golf</td><td>6</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>3</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>7</td></tr><tr><td>Loop</td><td>8</td></tr><tr><td>Good</td><td>9</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>4</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>12314</td></tr><tr><td>Banana</td><td>12321</td></tr><tr><td>Car</td><td>15646</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>5</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Egg</td><td>12647</td></tr><tr><td>Farm</td><td>13458</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>6</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Golf</td><td>13456</td></tr><tr><td>Half</td><td>15228</td></tr><tr><td>Iter</td><td>14509</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>7</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>12027</td></tr><tr><td>Keep</td><td>13339</td></tr></tbody>
    </table>
</td>
</tr>
</tbody>
</table>

## Clustered Index & Nonclustered Index
### Clustered Index

클러스터 인덱스는 데이터를 따로 저장하지 않고 인덱스의 리프노드에 데이터를 저장하는 방식입니다.

클러스터 인덱스는 데이터 자체를 포함하고 있기 때문에 단 한개만 생성할 수 있습니다.


<table>
<thead style="background-color: #d8dfec"><tr><td>루트 노드</td><td>브랜치 노드</td><td>리프 노드</td></tr></thead>
<tbody>
<tr>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>1</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>2</td></tr><tr><td>Jebra</td><td>3</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>2</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>4</td></tr><tr><td>Egg</td><td>5</td></tr><tr><td>Golf</td><td>6</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>3</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>7</td></tr><tr><td>Loop</td><td>8</td></tr><tr><td>Good</td><td>9</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>4</td></tr><tr><td>인덱스 키</td><td>Data</td></tr></thead>
    <tbody><tr><td>Apple</td><td>10개,사과,10</td></tr><tr><td>Banana</td><td>20개,바나나,12</td></tr><tr><td>Car</td><td>1개,자동차,10000</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>5</td></tr><tr><td>인덱스 키</td><td>Data</td></tr></thead>
    <tbody><tr><td>Egg</td><td>10개,계란,1</td></tr><tr><td>Farm</td><td>2개,농장,1000000</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>6</td></tr><tr><td>인덱스 키</td><td>Data</td></tr></thead>
    <tbody><tr><td>Golf</td><td>null,골프,null</td></tr><tr><td>Half</td><td>null,절반,null</td></tr><tr><td>Iter</td><td>null,반복,null</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>7</td></tr><tr><td>인덱스 키</td><td>Data</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>1개,얼룩말,$10000</td></tr><tr><td>Keep</td><td>null,보관,null</td></tr></tbody>
    </table>
</td>
</tr>
</tbody>
</table>

### Nonclustered Index

넌클러스터 인덱스는 데이터를 따로 저장해두고 리프노드에 데이터의 주소를 저장하는 방식입니다.

클러스터 인덱스와 달리 여러개를 생성할 수 있습니다.

<table>
<thead style="background-color: #d8dfec"><tr><td>루트 노드</td><td>브랜치 노드</td><td>리프 노드</td></tr></thead>
<tbody>
<tr>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>1</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>2</td></tr><tr><td>Jebra</td><td>3</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>2</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>4</td></tr><tr><td>Egg</td><td>5</td></tr><tr><td>Golf</td><td>6</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>3</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>7</td></tr><tr><td>Loop</td><td>8</td></tr><tr><td>Good</td><td>9</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>4</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>12314</td></tr><tr><td>Banana</td><td>12321</td></tr><tr><td>Car</td><td>15646</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>5</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Egg</td><td>12647</td></tr><tr><td>Farm</td><td>13458</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>6</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Golf</td><td>13456</td></tr><tr><td>Half</td><td>15228</td></tr><tr><td>Iter</td><td>14509</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>7</td></tr><tr><td>인덱스 키</td><td>데이터 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>12027</td></tr><tr><td>Keep</td><td>13339</td></tr></tbody>
    </table>
</td>
</tr>
</tbody>
</table>

---


<table>
<thead style="background-color: #d8dfec"><tr><td>주소</td><td>데이터</td></tr></thead>
<tbody>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
12027
</td>
<td>
Jebra,1개,얼룩말,$10000
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
12314
</td>
<td>
Apple,10개,사과,10
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
12321
</td>
<td>
Banana,20개,바나나,12
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
12647
</td>
<td>
Egg,10개,계란,1
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
13339
</td>
<td>
Keep,null,보관,null
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
13456
</td>
<td>
Golf,null,골프,null
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
13458
</td>
<td>
Farm,2개,농장,1000000
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
14509
</td>
<td>
Iter,null,반복,null
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
<tr>
<td>
15228
</td>
<td>
Half,null,절반,null
</td>
</tr>
<tr>
<td>
...
</td>
<td>
...
</td>
</tr>
</tbody>
</table>

# InnoDB 인덱스

Mysql InnoDB에서는 클러스터 인덱스와 넌클러스터 인덱스 모두를 사용한다.

테이블을 생성하면 자동으로 PK을 인덱스 키로하는 클러스터 인덱스가 생성된다.

그 외 모든 인덱스는 넌클러스터 인덱스로 생성되며 이 넌클러스터 인덱스의 리프노드에는 데이터의 주소대신에 PK가 저장되어 있다.

즉 넌클러스터 인덱스를 이용하여 PK를 조회하고 클러스터 인덱스와 PK를 이용하여 데이터를 획득한다.


#### 클러스터 인덱스

<table>
<thead style="background-color: #d8dfec"><tr><td>루트 노드</td><td>브랜치 노드</td><td>리프 노드</td></tr></thead>
<tbody>
<tr>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>1</td></tr><tr><td>PK</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>12027</td><td>2</td></tr><tr><td>15228</td><td>3</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>2</td></tr><tr><td>PK</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>12027</td><td>4</td></tr><tr><td>12647</td><td>5</td></tr><tr><td>13458</td><td>6</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>3</td></tr><tr><td>PK</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>15228</td><td>7</td></tr><tr><td>15647</td><td>8</td></tr><tr><td>16509</td><td>9</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>4</td></tr><tr><td>PK</td><td>Data</td></tr></thead>
    <tbody><tr><td>12027</td><td>Jebra,1개,얼룩말,$10000</td></tr><tr><td>12314</td><td>Apple,10개,사과,10</td></tr><tr><td>12321</td><td>Banana,20개,바나나,12</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>5</td></tr><tr><td>PK</td><td>Data</td></tr></thead>
    <tbody><tr><td>12647</td><td>Egg,10개,계란,1</td></tr><tr><td>13339</td><td>Keep,null,보관,null</td></tr><tr><td>13456</td><td>Golf,null,골프,null</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>6</td></tr><tr><td>PK</td><td>Data</td></tr></thead>
    <tbody><tr><td>13458</td><td>Farm,2개,농장,1000000</td></tr><tr><td>14509</td><td>Iter,null,반복,null</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>7</td></tr><tr><td>PK</td><td>Data</td></tr></thead>
    <tbody><tr><td>15228</td><td>Half,null,절반,null</td></tr><tr><td>15646</td><td>Car,1개,자동차,10000</td></tr></tbody>
    </table>
</td>
</tr>
</tbody>
</table>

#### 넌클러스터 인덱스

<table>
<thead style="background-color: #d8dfec"><tr><td>루트 노드</td><td>브랜치 노드</td><td>리프 노드</td></tr></thead>
<tbody>
<tr>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>1</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>2</td></tr><tr><td>Jebra</td><td>3</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>2</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Apple</td><td>4</td></tr><tr><td>Egg</td><td>5</td></tr><tr><td>Golf</td><td>6</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>3</td></tr><tr><td>인덱스 키</td><td>자식노드 주소</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>7</td></tr><tr><td>Loop</td><td>8</td></tr><tr><td>Good</td><td>9</td></tr></tbody>
    </table>
</td>
<td>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>4</td></tr><tr><td>인덱스 키</td><td>PK</td></tr></thead>
    <tbody><tr><td>Apple</td><td>12314</td></tr><tr><td>Banana</td><td>12321</td></tr><tr><td>Car</td><td>15646</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>5</td></tr><tr><td>인덱스 키</td><td>PK</td></tr></thead>
    <tbody><tr><td>Egg</td><td>12647</td></tr><tr><td>Farm</td><td>13458</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>6</td></tr><tr><td>인덱스 키</td><td>PK</td></tr></thead>
    <tbody><tr><td>Golf</td><td>13456</td></tr><tr><td>Half</td><td>15228</td></tr><tr><td>Iter</td><td>14509</td></tr></tbody>
    </table>
	<div style="text-align: center;">
		&uarr; &darr;
	</div>
    <table>
    <thead style="background-color: #d8dfec"><tr><td>페이지</td><td>7</td></tr><tr><td>인덱스 키</td><td>PK</td></tr></thead>
    <tbody><tr><td>Jebra</td><td>12027</td></tr><tr><td>Keep</td><td>13339</td></tr></tbody>
    </table>
</td>
</tr>
</tbody>
</table>

## 인덱스 활용하기

Mysql 인덱스를 활용하는 방법

### 커버링 인덱스

커버링 인덱스는 쿼리를 실행하는데 필요한 모든 데이터를 인덱스에서 추출할 수 있는 경우를 말합니다.

일반적으로 페이지네이션에 주로 사용합니다.

```sql
SELECT * FROM myTable ORDER BY index_key_part1 DESC LIMIT 1000, 10;
```
위 쿼리는 일반적으로 사용하는 페이지네이션 쿼리입니다.
이 경우 filesort를 사용하여 쿼리가 실행됩니다.

```sql
SELECT * FROM myTable AS b JOIN (SELECT pk FROM myTable ORDER BY index_key_part1 DESC LIMIT 1000, 10) AS a ON a.pk = b.pk;
```
위 쿼리는 index search를 이용하여 쿼리가 실행됩니다.

index_key와 넌클러스터 인덱스를 사용하여 PK를 획득하고 PK와 클러스터 인덱스를 이용하여 실제 데이터를 획득하게 됩니다.

#### 주의 사항

인덱스 key의 순서에 맞게 사용해야 합니다.

```sql
SELECT pk FROM myTable ORDER BY index_key_part1, index_key_part2, index_key_part3
SELECT count(pk) FROM myTable GROUP BY index_key_part1, index_key_part2
SELECT pk FROM myTable ORDER BY index_key_part1
SELECT pk FROM myTable WHERE index_key_part1 = 1  ORDER BY index_key_part2, index_key_part3
```
위 쿼리는 모두 index search를 사용하여 실행됩니다.


```sql
SELECT pk FROM myTable ORDER BY index_key_part2, index_key_part1, index_key_part3
SELECT count(pk) FROM myTable GROUP BY index_key_part2, index_key_part3
SELECT pk FROM myTable ORDER BY index_key_part2
```
위 쿼리는 모두 filesort를 사용하여 실행됩니다.