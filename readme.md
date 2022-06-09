# TableQA API Manual
## 域名
API Host URL
```python
http://180.184.68.175:9003
```
## 验证
```python
User: yuyitest 
Password: KismetFTW123
```
## API文档
## 表格问答 ```/tableqa/```
### post
输入一批表格及问题，返回SQL语句
### 请求参数
#### schema
参数 | 类型 | 是否必选 | 示例值 |描述
-----|------|-----|-----|-----
db_id | string | 是 | "文集"  | 输入多表格名称
table_names | list | 是 | ["作者","文集"]  | 输入各表格名称
column_names | list | 是 | [[-1,"*"],[0,"姓名"],[0,"国籍"],[1,"页数"],[1,"定价"] | 各列名称
column_types | list | 是 | ["text","text","number","number"] | 每列数据类型
foreign_keys | list | 否 | [[3,1][2,1]] | 外键约束
primary_keys | list | 否 | [1] | 主键约束

#### content
参数 | 类型 | 是否必选 | 示例值 |描述
-----|------|-----|-----|-----
db_id | string | 是 | "文集"  | 输入多表格名称
tables | dict | 是 | {"作者":{"cell":[["鲁迅", "中国"], ["郭沫若", "中国"]],"header":["姓名","国籍"],"table_name":"作者","type":["text","text"]},"文集":…} | 表中内容

#### questions
参数 | 类型 | 是否必选 | 示例值 |描述
-----|------|-----|-----|-----
db_id | string | 是 | "文集"  | 提问表格名称
question | string | 是 | "哪些作家没有写过文集？给出这些作家名字和国籍。" | 问题
question_id | string | 否 | 0001 | 问题编号

### 示例
###### 请求示例
```json
{"schema":[{"db_id": "文集",
          "table_names": ["作者","文集"],
          "column_names": [[-1,"*"],[0,"词条id"],[0,"姓名"],[0,"国籍"],[0,"毕业院校"],[0,"民族"],[1,"词条id"],[1,"名称"],[1,"作者id"],[1,"页数"],[1,"定价"],[1,"出版社"],[1,"出版时间"],[1,"开本"]],
          "column_types": ["text","number","text","text","text","text","number","text","number","number","number","text","time","text"],
          "foreign_keys": [[8,1]],
          "primary_keys": [1,6]},
          {"db_id": "智能音箱",
           "table_names": ["公司","音箱产品","产品销售"],
           "column_names": [[-1,"*"],[0,"词条id"],[0,"名称"],[0,"所属国家"],[0,"智能音箱款数"],[0,"排名"],[1,"词条id"][1,"名称"],[1,"所属公司id"],[1,"售价"],[1,"排名"],[1,"上升名次"],[2,"产品id"],[2,"季度"],[2,"销售量"],[2,"销售量增长"]],
           "column_types": ["text","number","text","text","number","number","number","text","number","number","number","number","number","text","number","number"],
           "foreign_keys": [[8,1],[12,6]],
           "primary_keys": [1,6]}],
"content":[{"db_id": "文集",
            "tables": {"作者": {"cell": [["item_book.2_6_56","鲁迅","中国","南京矿路学堂","汉族"],["item_book.2_6_57","郭沫若","中国","北师学堂","满族"],["item_book.2_6_58","冰心","中国","燕京大学","回族"],["item_book.2_6_59","张恨水","中国","九州帝国大学","土家族"],["item_book.2_6_60","贾平凹","中国","九州帝国大学","傣族"]],
                                "header": ["词条id","姓名","国籍","毕业院校","民族"],
                                "table_name": "作者",
                                "type": ["number","text","text","text","text"]},
                       "文集": {"cell": [["item_book.2_6_61","鲁迅全集","item_book.2_6_57","200","2.8","人民文学出版社","1982-07-21","32开"],["item_book.2_6_62","郭沫若全集","item_book.2_6_57","230","4","译林出版社","1983-06-21","17开"],["item_book.2_6_63","路遥集","item_book.2_6_58","250","5","安徽人民出版社","1989-11-21","15开"],["item_book.2_6_64","冰心全集","item_book.2_6_56","400","20","武汉文艺出版社","1988-07-21","20开"],["item_book.2_6_65","贾平凹全集","item_book.2_6_59","500","28","安徽人民出版社","1990-03-02","16开"]],
                                "header": ["词条id","名称","作者id","页数","定价","出版社","出版时间","开本"],
                                "table_name": "文集",
                                "type": ["number","text","number","number","number","text","time","text"]}}},
          {"db_id": "智能音箱",
           "tables": {"产品销售": {"cell": [["item_product_7_51","1","1万","20%"],["item_product_7_54","4","300万","80%"],["item_product_7_55","1","300万","20%"],["item_product_7_54","1","300万","20%"],["item_product_7_52","4","1万","20%"]],
                                  "header": ["产品id","季度","销售量","销售量增长"],
                                  "table_name": "产品销售",
                                  "type": ["number","text","number","number"]},
                       "公司": {"cell": [["item_product_7_46","百度","中国","1","1"],["item_product_7_47","阿里巴巴集团","美国","8","10"],["item_product_7_48","小米公司","韩国","1","1"],["item_product_7_49","亚马逊集团","日本","8","1"],["item_product_7_50","微软公司","德国","1","1"]],
                                "header": ["词条id","名称","所属国家","智能音箱款数","排名"],
                                "table_name": "公司",
                                "type": ["number","text","text","number","number"]},
                       "音箱产品": {"cell": [["item_product_7_51","小爱","item_product_7_46","89","1","-3"],["item_product_7_52","天猫精灵","item_product_7_47","599","20","+3"],["item_product_7_53","小度在家","item_product_7_47","89","20","+3"],["item_product_7_54","小度音箱","item_product_7_50","89","1","+3"],["item_product_7_55","亚马逊echo","item_product_7_46","89","1","+3"]],
                                   "header": ["词条id","名称","所属公司id","售价","排名","上升名次"],
                                   "table_name": "音箱产品",
                                   "type": ["number","text","number","number","number","number"]}}}],
"questions":[{"db_id": "智能音箱", 
        "question": "比89元便宜的音箱产品有哪些，并给出它们原来的排名", 
        "question_id": "qid0001"},
        {"db_id": "智能音箱", 
        "question": "给出不属于公司智能音箱总共不超过70款的国家，以及这些国家有哪些公司", 
        "question_id": "qid0002"},
        {"db_id": "智能音箱", 
        "question": "给出原排名在10名及之外的音箱产品和原排名", 
        "question_id": "qid0003"},
        {"db_id": "文集", 
        "question": "不属于作者最少的两个国籍，给出其他国家的作者的名字", 
        "question_id": "qid0004"}]}
```

###### 返回示例
```sql
select 名称 , 排名 - 上升名次 from 音箱产品 where 售价 < 89
select 名称 from 公司 where 所属国家 not in ( select 所属国家 from 公司 group by 所属国家 having sum ( 智能音箱款数 ) <= 70 )
select 名称 , 排名 + 上升名次 from 音箱产品 where 排名 + 上升名次 >= 10
select 姓名 from 作者 where 国籍 not in ( select 国籍 from 作者 group by 国籍 order by count ( * ) asc limit 2 )
```



