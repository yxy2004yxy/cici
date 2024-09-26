# 实验二  索引&文档操作

**学院：**省级示范性软件学院

**题目：**《实验二  索引&文档操作》

**姓名：**岳馨雨

**学号：**2200770267

**班级：**软工2202

**日期：**2024-09-26

**实验环境：** elasticsearch-8.12.2

## **实验内容：**

1. **Elasticsearch 安装IK分词器**
2. **Elasticsearch 索引操作训练**
3. **Elasticsearch 文档操作训练**
4. **Elasticsearch 高级查询与DSL训练**

## **实验目标：**

1. **掌握Elasticsearch 安装IK分词器安装方法**
2. **掌握Elasticsearch 索引操作方法**
3. **掌握Elasticsearch 文档操作训练**
4. **Elasticsearch 高级查询与DSL训练**

## **任务一**：

### **创建索引**

```json
//创建索引用户信息
PUT /user_information
{"mappings":{"properties":{"user_id":{"type":"keyword"},"name":{"type":"text"},"email":{"type":"text"},"date_of_birth":{"type":"date","format":"yyyy-MM-dd"},"gender":{"type":"keyword"},"address":{"type":"text"},"phone_number":{"type":"text"},"registration_date":{"type":"date","format":"yyyy-MM-dd"},"last_login":{"type":"date","format":"yyyy-MM-dd"},"status":{"type":"keyword"}}}}

```

![image-20240926124543552](./assets/image-20240926124543552.png)

创建成功后查看该索引

![image-20240926124616554](./assets/image-20240926124616554.png)

### **修改索引**

```json
PUT /user_information/_mapping
{
  "properties": {
    "people_number": {
      "type": "double"
    }
  }
}
```

修改前如上图

![image-20240926124815067](./assets/image-20240926124815067.png)

修改后再次查看索引

![image-20240926124854974](./assets/image-20240926124854974.png)



### **删除索引**

```json
//删除用户信息索引
DELETE /user_information
```

![image-20240926124917925](./assets/image-20240926124917925.png)

删除成功后查看索引

![image-20240926125013626](./assets/image-20240926125013626.png)

### **查看索引**

```json
//查看用户信息索引
GET /user_information
//查看结果
{
  "user_information": {
    "aliases": {},
    "mappings": {
      "properties": {
        "address": {
          "type": "text"
        },
        "date_of_birth": {
          "type": "date",
          "format": "yyyy-MM-dd"
        },
        "email": {
          "type": "text"
        },
        "gender": {
          "type": "keyword"
        },
        "last_login": {
          "type": "date",
          "format": "yyyy-MM-dd"
        },
        "name": {
          "type": "text"
        },
        "phone_number": {
          "type": "text"
        },
        "registration_date": {
          "type": "date",
          "format": "yyyy-MM-dd"
        },
        "status": {
          "type": "keyword"
        },
        "user_id": {
          "type": "keyword"
        }
      }
    },
    "settings": {
      "index": {
        "routing": {
          "allocation": {
            "include": {
              "_tier_preference": "data_content"
            }
          }
        },
        "number_of_shards": "1",
        "provided_name": "user_information",
        "creation_date": "1727180579429",
        "number_of_replicas": "1",
        "uuid": "-v4ix1HoQqK-FrVfCSLZ-g",
        "version": {
          "created": "8500010"
        }
      }
    }
  }
}
```

## 任务二

### 创建文档

```json
//创建文档
POST /user_information/_doc/1
{
    "user_id": "001",
    "name": "Alice Johnson",
    "email": "alice.johnson@example.com",
    "date_of_birth": "1990-05-15",
    "gender": "female",
    "address": "123 Main St, Anytown, USA",
    "phone_number": "123-456-7890",
    "registration_date": "2023-01-15",
    "last_login": "2024-09-01",
    "status": "active"
  }
```

![image-20240926125131989](./assets/image-20240926125131989.png)

### 查看文档

```
GET /user_information/_doc/1
```

![image-20240926125322658](./assets/image-20240926125322658.png)

### 修改文档

```json
//修改文档中的部分内容
POST /user_information/_update/1
{
  "doc": {
    "name": "Alice"
  }
}
```

![image-20240926125401540](./assets/image-20240926125401540.png)

修改后再次查看该文档

![image-20240926125439737](./assets/image-20240926125439737.png)

### 删除文档

```json
//删除文档
DELETE /user_information/_doc/1 
```

![image-20240926125455459](./assets/image-20240926125455459.png)

删除文档后再次查看该文档

![image-20240926125518684](./assets/image-20240926125518684.png)


### 将Json数据批量导入ES数据库中

导入user_information数据

```json
//批量导入user_information数据
POST /user_information/_bulk
{"index":{"_id":"001"}}
{"user_id":"001","name":"Alice Johnson","email":"alice.johnson@example.com","date_of_birth":"1990-05-15","gender":"female","address":"123 Main St, Anytown, USA","phone_number":"123-456-7890","registration_date":"2023-01-15","last_login":"2024-09-01","status":"active"}
{"index":{"_id":"002"}}
{"user_id":"002","name":"Bob Smith","email":"bob.smith@example.com","date_of_birth":"1985-08-20","gender":"male","address":"456 Elm St, Othertown, USA","phone_number":"234-567-8901","registration_date":"2023-02-20","last_login":"2024-08-25","status":"active"}
{"index":{"_id":"003"}}
{"user_id":"003","name":"Charlie Brown","email":"charlie.brown@example.com","date_of_birth":"1992-11-30","gender":"male","address":"789 Maple Ave, Sometown, USA","phone_number":"345-678-9012","registration_date":"2023-03-10","last_login":"2024-09-05","status":"inactive"}
{"index":{"_id":"004"}}
{"user_id":"004","name":"David Wilson","email":"david.wilson@example.com","date_of_birth":"1988-02-14","gender":"male","address":"101 Oak St, Anycity, USA","phone_number":"456-789-0123","registration_date":"2023-04-05","last_login":"2024-08-30","status":"active"}
{"index":{"_id":"005"}}
{"user_id":"005","name":"Eve Davis","email":"eve.davis@example.com","date_of_birth":"1995-07-22","gender":"female","address":"202 Pine St, Otherville, USA","phone_number":"567-890-1234","registration_date":"2023-05-18","last_login":"2024-09-02","status":"active"}
{"index":{"_id":"006"}}
{"user_id":"006","name":"Frank Miller","email":"frank.miller@example.com","date_of_birth":"1991-10-05","gender":"male","address":"303 Cedar Rd, Newtown, USA","phone_number":"678-901-2345","registration_date":"2023-06-22","last_login":"2024-09-03","status":"active"}
{"index":{"_id":"007"}}
{"user_id":"007","name":"Grace Lee","email":"grace.lee@example.com","date_of_birth":"1989-04-17","gender":"female","address":"404 Birch Ln, Oldtown, USA","phone_number":"789-012-3456","registration_date":"2023-07-30","last_login":"2024-09-04","status":"inactive"}
{"index":{"_id":"008"}}
{"user_id":"008","name":"Hannah White","email":"hannah.white@example.com","date_of_birth":"1993-12-25","gender":"female","address":"505 Spruce St, Littletown, USA","phone_number":"890-123-4567","registration_date":"2023-08-15","last_login":"2024-09-06","status":"active"}
{"index":{"_id":"009"}}
{"user_id":"009","name":"Ivy Green","email":"ivy.green@example.com","date_of_birth":"1994-03-03","gender":"female","address":"606 Willow Ave, Bigcity, USA","phone_number":"901-234-5678","registration_date":"2023-09-01","last_login":"2024-09-07","status":"active"}
{"index":{"_id":"010"}}
{"user_id":"010","name":"Jack Black","email":"jack.black@example.com","date_of_birth":"1987-06-18","gender":"male","address":"707 Poplar Dr, Smalltown, USA","phone_number":"012-345-6789","registration_date":"2023-10-10","last_login":"2024-09-08","status":"inactive"}
{"index":{"_id":"011"}}
{"user_id":"011","name":"Karen Adams","email":"karen.adams@example.com","date_of_birth":"1990-09-21","gender":"female","address":"808 Chestnut Blvd, Metropolis, USA","phone_number":"123-456-7890","registration_date":"2023-11-25","last_login":"2024-09-09","status":"active"}
{"index":{"_id":"012"}}
{"user_id":"012","name":"Leo King","email":"leo.king@example.com","date_of_birth":"1986-01-12","gender":"male","address":"909 Ash Ct, Villagetown, USA","phone_number":"234-567-8901","registration_date":"2023-12-05","last_login":"2024-09-10","status":"active"}
{"index":{"_id":"013"}}
{"user_id":"013","name":"Mia Moore","email":"mia.moore@example.com","date_of_birth":"1992-02-28","gender":"female","address":"1010 Fir Ln, Hamlet, USA","phone_number":"345-678-9012","registration_date":"2024-01-10","last_login":"2024-09-11","status":"inactive"}
{"index":{"_id":"014"}}
{"user_id":"014","name":"Nina Scott","email":"nina.scott@example.com","date_of_birth":"1988-05-05","gender":"female","address":"1111 Cypress St, Township, USA","phone_number":"456-789-0123","registration_date":"2024-02-14","last_login":"2024-09-12","status":"active"}
{"index":{"_id":"015"}}
{"user_id":"015","name":"Oscar Turner","email":"oscar.turner@example.com","date_of_birth":"1991-07-19","gender":"male","address":"1212 Redwood Rd, Cityville, USA","phone_number":"567-890-1234","registration_date":"2024-03-20","last_login":"2024-09-13","status":"active"}
{"index":{"_id":"016"}}
{"user_id":"016","name":"Paul Walker","email":"paul.walker@example.com","date_of_birth":"1989-10-10","gender":"male","address":"1313 Cherry Ave, Urbantown, USA","phone_number":"678-901-2345","registration_date":"2024-04-25","last_login":"2024-09-14","status":"inactive"}
{"index":{"_id":"017"}}
{"user_id":"017","name":"Quinn Hughes","email":"quinn.hughes@example.com","date_of_birth":"1993-11-11","gender":"male","address":"1414 Beech St, Suburbia, USA","phone_number":"789-012-3456","registration_date":"2024-05-30","last_login":"2024-09-15","status":"active"}
{"index":{"_id":"018"}}
{"user_id":"018","name":"Rose Hall","email":"rose.hall@example.com","date_of_birth":"1990-03-22","gender":"female","address":"1515 Palm Dr, Downtown, USA","phone_number":"890-123-4567","registration_date":"2024-06-15","last_login":"2024-09-16","status":"active"}
{"index":{"_id":"019"}}
{"user_id":"019","name":"Steve Young","email":"steve.young@example.com","date_of_birth":"1987-09-09","gender":"male","address":"1616 Pine St, Uptown, USA","phone_number":"901-234-5678","registration_date":"2024-07-20","last_login":"2024-09-17","status":"inactive"}
{"index":{"_id":"020"}}
{"user_id":"020","name":"Tina Brown","email":"tina.brown@example.com","date_of_birth":"1994-12-12","gender":"female","address":"1717 Maple Ave, Seaside, USA","phone_number":"012-345-6789","registration_date":"2024-08-10","last_login":"2024-09-18","status":"active"}
```

![image-20240926125602280](./assets/image-20240926125602280.png)

查看导入的数据

```json
GET /user_information/_search
{
  "query": {
    "match_all": {}
  },
      "size": 20
}

```

![image-20240926125622261](./assets/image-20240926125622261.png)

导入结果

```json
{
  "took": 11,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 20,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 1,
        "_source": {
          "user_id": "001",
          "name": "Alice Johnson",
          "email": "alice.johnson@example.com",
          "date_of_birth": "1990-05-15",
          "gender": "female",
          "address": "123 Main St, Anytown, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-01-15",
          "last_login": "2024-09-01",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "002",
        "_score": 1,
        "_source": {
          "user_id": "002",
          "name": "Bob Smith",
          "email": "bob.smith@example.com",
          "date_of_birth": "1985-08-20",
          "gender": "male",
          "address": "456 Elm St, Othertown, USA",
          "phone_number": "234-567-8901",
          "registration_date": "2023-02-20",
          "last_login": "2024-08-25",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "003",
        "_score": 1,
        "_source": {
          "user_id": "003",
          "name": "Charlie Brown",
          "email": "charlie.brown@example.com",
          "date_of_birth": "1992-11-30",
          "gender": "male",
          "address": "789 Maple Ave, Sometown, USA",
          "phone_number": "345-678-9012",
          "registration_date": "2023-03-10",
          "last_login": "2024-09-05",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "004",
        "_score": 1,
        "_source": {
          "user_id": "004",
          "name": "David Wilson",
          "email": "david.wilson@example.com",
          "date_of_birth": "1988-02-14",
          "gender": "male",
          "address": "101 Oak St, Anycity, USA",
          "phone_number": "456-789-0123",
          "registration_date": "2023-04-05",
          "last_login": "2024-08-30",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 1,
        "_source": {
          "user_id": "005",
          "name": "Eve Davis",
          "email": "eve.davis@example.com",
          "date_of_birth": "1995-07-22",
          "gender": "female",
          "address": "202 Pine St, Otherville, USA",
          "phone_number": "567-890-1234",
          "registration_date": "2023-05-18",
          "last_login": "2024-09-02",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "006",
        "_score": 1,
        "_source": {
          "user_id": "006",
          "name": "Frank Miller",
          "email": "frank.miller@example.com",
          "date_of_birth": "1991-10-05",
          "gender": "male",
          "address": "303 Cedar Rd, Newtown, USA",
          "phone_number": "678-901-2345",
          "registration_date": "2023-06-22",
          "last_login": "2024-09-03",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 1,
        "_source": {
          "user_id": "007",
          "name": "Grace Lee",
          "email": "grace.lee@example.com",
          "date_of_birth": "1989-04-17",
          "gender": "female",
          "address": "404 Birch Ln, Oldtown, USA",
          "phone_number": "789-012-3456",
          "registration_date": "2023-07-30",
          "last_login": "2024-09-04",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 1,
        "_source": {
          "user_id": "008",
          "name": "Hannah White",
          "email": "hannah.white@example.com",
          "date_of_birth": "1993-12-25",
          "gender": "female",
          "address": "505 Spruce St, Littletown, USA",
          "phone_number": "890-123-4567",
          "registration_date": "2023-08-15",
          "last_login": "2024-09-06",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 1,
        "_source": {
          "user_id": "009",
          "name": "Ivy Green",
          "email": "ivy.green@example.com",
          "date_of_birth": "1994-03-03",
          "gender": "female",
          "address": "606 Willow Ave, Bigcity, USA",
          "phone_number": "901-234-5678",
          "registration_date": "2023-09-01",
          "last_login": "2024-09-07",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "010",
        "_score": 1,
        "_source": {
          "user_id": "010",
          "name": "Jack Black",
          "email": "jack.black@example.com",
          "date_of_birth": "1987-06-18",
          "gender": "male",
          "address": "707 Poplar Dr, Smalltown, USA",
          "phone_number": "012-345-6789",
          "registration_date": "2023-10-10",
          "last_login": "2024-09-08",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 1,
        "_source": {
          "user_id": "011",
          "name": "Karen Adams",
          "email": "karen.adams@example.com",
          "date_of_birth": "1990-09-21",
          "gender": "female",
          "address": "808 Chestnut Blvd, Metropolis, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-11-25",
          "last_login": "2024-09-09",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "012",
        "_score": 1,
        "_source": {
          "user_id": "012",
          "name": "Leo King",
          "email": "leo.king@example.com",
          "date_of_birth": "1986-01-12",
          "gender": "male",
          "address": "909 Ash Ct, Villagetown, USA",
          "phone_number": "234-567-8901",
          "registration_date": "2023-12-05",
          "last_login": "2024-09-10",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "013",
        "_score": 1,
        "_source": {
          "user_id": "013",
          "name": "Mia Moore",
          "email": "mia.moore@example.com",
          "date_of_birth": "1992-02-28",
          "gender": "female",
          "address": "1010 Fir Ln, Hamlet, USA",
          "phone_number": "345-678-9012",
          "registration_date": "2024-01-10",
          "last_login": "2024-09-11",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "014",
        "_score": 1,
        "_source": {
          "user_id": "014",
          "name": "Nina Scott",
          "email": "nina.scott@example.com",
          "date_of_birth": "1988-05-05",
          "gender": "female",
          "address": "1111 Cypress St, Township, USA",
          "phone_number": "456-789-0123",
          "registration_date": "2024-02-14",
          "last_login": "2024-09-12",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "015",
        "_score": 1,
        "_source": {
          "user_id": "015",
          "name": "Oscar Turner",
          "email": "oscar.turner@example.com",
          "date_of_birth": "1991-07-19",
          "gender": "male",
          "address": "1212 Redwood Rd, Cityville, USA",
          "phone_number": "567-890-1234",
          "registration_date": "2024-03-20",
          "last_login": "2024-09-13",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "016",
        "_score": 1,
        "_source": {
          "user_id": "016",
          "name": "Paul Walker",
          "email": "paul.walker@example.com",
          "date_of_birth": "1989-10-10",
          "gender": "male",
          "address": "1313 Cherry Ave, Urbantown, USA",
          "phone_number": "678-901-2345",
          "registration_date": "2024-04-25",
          "last_login": "2024-09-14",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "017",
        "_score": 1,
        "_source": {
          "user_id": "017",
          "name": "Quinn Hughes",
          "email": "quinn.hughes@example.com",
          "date_of_birth": "1993-11-11",
          "gender": "male",
          "address": "1414 Beech St, Suburbia, USA",
          "phone_number": "789-012-3456",
          "registration_date": "2024-05-30",
          "last_login": "2024-09-15",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "018",
        "_score": 1,
        "_source": {
          "user_id": "018",
          "name": "Rose Hall",
          "email": "rose.hall@example.com",
          "date_of_birth": "1990-03-22",
          "gender": "female",
          "address": "1515 Palm Dr, Downtown, USA",
          "phone_number": "890-123-4567",
          "registration_date": "2024-06-15",
          "last_login": "2024-09-16",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "019",
        "_score": 1,
        "_source": {
          "user_id": "019",
          "name": "Steve Young",
          "email": "steve.young@example.com",
          "date_of_birth": "1987-09-09",
          "gender": "male",
          "address": "1616 Pine St, Uptown, USA",
          "phone_number": "901-234-5678",
          "registration_date": "2024-07-20",
          "last_login": "2024-09-17",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "020",
        "_score": 1,
        "_source": {
          "user_id": "020",
          "name": "Tina Brown",
          "email": "tina.brown@example.com",
          "date_of_birth": "1994-12-12",
          "gender": "female",
          "address": "1717 Maple Ave, Seaside, USA",
          "phone_number": "012-345-6789",
          "registration_date": "2024-08-10",
          "last_login": "2024-09-18",
          "status": "active"
        }
      }
    ]
  }
}
```

导入product_catalog数据

```json
//批量导入product_catalog数据
POST /product_catalog/_bulk
{"index":{"_id":"1"}}
{"product_id":"P001","name":"Wireless Mouse","description":"A high precision wireless mouse with ergonomic design.","category":"Electronics","price":29.99,"stock_quantity":150,"supplier":"TechCorp","release_date":"2023-01-15","tags":["wireless","mouse","electronics"],"rating":4.5}
{"index":{"_id":"2"}}
{"product_id":"P002","name":"Bluetooth Speaker","description":"Portable Bluetooth speaker with excellent sound quality.","category":"Audio","price":49.99,"stock_quantity":200,"supplier":"SoundWave","release_date":"2023-02-20","tags":["bluetooth","speaker","audio"],"rating":4.7}
{"index":{"_id":"3"}}
{"product_id":"P003","name":"Smartphone Stand","description":"Adjustable smartphone stand for hands-free viewing.","category":"Accessories","price":15.99,"stock_quantity":300,"supplier":"GadgetPlus","release_date":"2023-03-10","tags":["smartphone","stand","accessories"],"rating":4.3}
{"index":{"_id":"4"}}
{"product_id":"P004","name":"USB-C Charger","description":"Fast charging USB-C charger compatible with most devices.","category":"Chargers","price":19.99,"stock_quantity":250,"supplier":"ChargeMaster","release_date":"2023-04-05","tags":["usb-c","charger","electronics"],"rating":4.6}
{"index":{"_id":"5"}}
{"product_id":"P005","name":"Noise Cancelling Headphones","description":"Over-ear headphones with active noise cancellation.","category":"Audio","price":89.99,"stock_quantity":100,"supplier":"AudioTech","release_date":"2023-05-18","tags":["noise cancelling","headphones","audio"],"rating":4.8}
{"index":{"_id":"6"}}
{"product_id":"P006","name":"4K Monitor","description":"Ultra HD 4K monitor with stunning display quality.","category":"Monitors","price":299.99,"stock_quantity":80,"supplier":"ViewPerfect","release_date":"2023-06-22","tags":["4k","monitor","electronics"],"rating":4.9}
{"index":{"_id":"7"}}
{"product_id":"P007","name":"Gaming Keyboard","description":"Mechanical keyboard with customizable RGB lighting.","category":"Gaming","price":59.99,"stock_quantity":120,"supplier":"GameGear","release_date":"2023-07-30","tags":["gaming","keyboard","electronics"],"rating":4.4}
{"index":{"_id":"8"}}
{"product_id":"P008","name":"Fitness Tracker","description":"Smart fitness tracker with heart rate monitor.","category":"Wearables","price":39.99,"stock_quantity":180,"supplier":"FitLife","release_date":"2023-08-15","tags":["fitness","tracker","wearables"],"rating":4.2}
{"index":{"_id":"9"}}
{"product_id":"P009","name":"Electric Toothbrush","description":"Rechargeable electric toothbrush with multiple modes.","category":"Personal Care","price":34.99,"stock_quantity":140,"supplier":"SmileBright","release_date":"2023-09-01","tags":["electric","toothbrush","personal care"],"rating":4.5}
{"index":{"_id":"10"}}
{"product_id":"P010","name":"Smart Thermostat","description":"Wi-Fi enabled smart thermostat with energy saving features.","category":"Home Automation","price":129.99,"stock_quantity":90,"supplier":"HomeSmart","release_date":"2023-10-10","tags":["smart","thermostat","home automation"],"rating":4.7}
{"index":{"_id":"11"}}
{"product_id":"P011","name":"LED Desk Lamp","description":"Adjustable LED desk lamp with touch control.","category":"Lighting","price":24.99,"stock_quantity":160,"supplier":"BrightLight","release_date":"2023-11-25","tags":["led","desk lamp","lighting"],"rating":4.3}
{"index":{"_id":"12"}}
{"product_id":"P012","name":"Portable Hard Drive","description":"1TB portable hard drive with fast data transfer.","category":"Storage","price":64.99,"stock_quantity":110,"supplier":"DataSafe","release_date":"2023-12-05","tags":["portable","hard drive","storage"],"rating":4.6}
{"index":{"_id":"13"}}
{"product_id":"P013","name":"Coffee Maker","description":"Automatic coffee maker with programmable settings.","category":"Kitchen Appliances","price":79.99,"stock_quantity":130,"supplier":"BrewMaster","release_date":"2024-01-10","tags":["coffee","maker","appliances"],"rating":4.4}
{"index":{"_id":"14"}}
{"product_id":"P014","name":"Smart Light Bulb","description":"Color changing smart light bulb with app control.","category":"Lighting","price":14.99,"stock_quantity":220,"supplier":"LightSmart","release_date":"2024-02-14","tags":["smart","light bulb","lighting"],"rating":4.2}
{"index":{"_id":"15"}}
{"product_id":"P015","name":"Electric Kettle","description":"Quick boil electric kettle with auto shut-off.","category":"Kitchen Appliances","price":29.99,"stock_quantity":170,"supplier":"QuickBoil","release_date":"2024-03-20","tags":["electric","kettle","appliances"],"rating":4.5}
{"index":{"_id":"16"}}
{"product_id":"P016","name":"Robot Vacuum","description":"Smart robot vacuum cleaner with scheduling features.","category":"Home Appliances","price":199.99,"stock_quantity":60,"supplier":"CleanBot","release_date":"2024-04-25","tags":["robot","vacuum","home appliances"],"rating":4.8}
{"index":{"_id":"17"}}
{"product_id":"P017","name":"Digital Camera","description":"Compact digital camera with high resolution.","category":"Photography","price":149.99,"stock_quantity":95,"supplier":"PhotoSnap","release_date":"2024-05-30","tags":["digital","camera","photography"],"rating":4.7}
{"index":{"_id":"18"}}
{"product_id":"P018","name":"Smartwatch","description":"Feature-rich smartwatch with fitness tracking.","category":"Wearables","price":99.99,"stock_quantity":140,"supplier":"WristTech","release_date":"2024-06-15","tags":["smartwatch","wearables","fitness"],"rating":4.4}
{"index":{"_id":"19"}}
{"product_id":"P019","name":"Air Fryer","description":"Healthier cooking with a digital air fryer.","category":"Kitchen Appliances","price":89.99,"stock_quantity":115,"supplier":"HealthyCook","release_date":"2024-07-20","tags":["air fryer","kitchen","appliances"],"rating":4.6}
{"index":{"_id":"20"}}
{"product_id":"P020","name":"Electric Scooter","description":"Eco-friendly electric scooter with long battery life.","category":"Transportation","price":299.99,"stock_quantity":50,"supplier":"EcoRide","release_date":"2024-08-10","tags":["electric","scooter","transportation"],"rating":4.7}

```

![image-20240926125819769](./assets/image-20240926125819769.png)

查看导入的数据

```json
GET /product_catalog/_search
{
  "query": {
    "match_all": {}
  },
      "size": 20
}
```

![image-20240926125839249](./assets/image-20240926125839249.png)

导入结果

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 20,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "1",
        "_score": 1,
        "_source": {
          "product_id": "P001",
          "name": "Wireless Mouse",
          "description": "A high precision wireless mouse with ergonomic design.",
          "category": "Electronics",
          "price": 29.99,
          "stock_quantity": 150,
          "supplier": "TechCorp",
          "release_date": "2023-01-15",
          "tags": [
            "wireless",
            "mouse",
            "electronics"
          ],
          "rating": 4.5
        }
      },
      {
        "_index": "product_catalog",
        "_id": "2",
        "_score": 1,
        "_source": {
          "product_id": "P002",
          "name": "Bluetooth Speaker",
          "description": "Portable Bluetooth speaker with excellent sound quality.",
          "category": "Audio",
          "price": 49.99,
          "stock_quantity": 200,
          "supplier": "SoundWave",
          "release_date": "2023-02-20",
          "tags": [
            "bluetooth",
            "speaker",
            "audio"
          ],
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "3",
        "_score": 1,
        "_source": {
          "product_id": "P003",
          "name": "Smartphone Stand",
          "description": "Adjustable smartphone stand for hands-free viewing.",
          "category": "Accessories",
          "price": 15.99,
          "stock_quantity": 300,
          "supplier": "GadgetPlus",
          "release_date": "2023-03-10",
          "tags": [
            "smartphone",
            "stand",
            "accessories"
          ],
          "rating": 4.3
        }
      },
      {
        "_index": "product_catalog",
        "_id": "4",
        "_score": 1,
        "_source": {
          "product_id": "P004",
          "name": "USB-C Charger",
          "description": "Fast charging USB-C charger compatible with most devices.",
          "category": "Chargers",
          "price": 19.99,
          "stock_quantity": 250,
          "supplier": "ChargeMaster",
          "release_date": "2023-04-05",
          "tags": [
            "usb-c",
            "charger",
            "electronics"
          ],
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "5",
        "_score": 1,
        "_source": {
          "product_id": "P005",
          "name": "Noise Cancelling Headphones",
          "description": "Over-ear headphones with active noise cancellation.",
          "category": "Audio",
          "price": 89.99,
          "stock_quantity": 100,
          "supplier": "AudioTech",
          "release_date": "2023-05-18",
          "tags": [
            "noise cancelling",
            "headphones",
            "audio"
          ],
          "rating": 4.8
        }
      },
      {
        "_index": "product_catalog",
        "_id": "6",
        "_score": 1,
        "_source": {
          "product_id": "P006",
          "name": "4K Monitor",
          "description": "Ultra HD 4K monitor with stunning display quality.",
          "category": "Monitors",
          "price": 299.99,
          "stock_quantity": 80,
          "supplier": "ViewPerfect",
          "release_date": "2023-06-22",
          "tags": [
            "4k",
            "monitor",
            "electronics"
          ],
          "rating": 4.9
        }
      },
      {
        "_index": "product_catalog",
        "_id": "7",
        "_score": 1,
        "_source": {
          "product_id": "P007",
          "name": "Gaming Keyboard",
          "description": "Mechanical keyboard with customizable RGB lighting.",
          "category": "Gaming",
          "price": 59.99,
          "stock_quantity": 120,
          "supplier": "GameGear",
          "release_date": "2023-07-30",
          "tags": [
            "gaming",
            "keyboard",
            "electronics"
          ],
          "rating": 4.4
        }
      },
      {
        "_index": "product_catalog",
        "_id": "8",
        "_score": 1,
        "_source": {
          "product_id": "P008",
          "name": "Fitness Tracker",
          "description": "Smart fitness tracker with heart rate monitor.",
          "category": "Wearables",
          "price": 39.99,
          "stock_quantity": 180,
          "supplier": "FitLife",
          "release_date": "2023-08-15",
          "tags": [
            "fitness",
            "tracker",
            "wearables"
          ],
          "rating": 4.2
        }
      },
      {
        "_index": "product_catalog",
        "_id": "9",
        "_score": 1,
        "_source": {
          "product_id": "P009",
          "name": "Electric Toothbrush",
          "description": "Rechargeable electric toothbrush with multiple modes.",
          "category": "Personal Care",
          "price": 34.99,
          "stock_quantity": 140,
          "supplier": "SmileBright",
          "release_date": "2023-09-01",
          "tags": [
            "electric",
            "toothbrush",
            "personal care"
          ],
          "rating": 4.5
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1,
        "_source": {
          "product_id": "P010",
          "name": "Smart Thermostat",
          "description": "Wi-Fi enabled smart thermostat with energy saving features.",
          "category": "Home Automation",
          "price": 129.99,
          "stock_quantity": 90,
          "supplier": "HomeSmart",
          "release_date": "2023-10-10",
          "tags": [
            "smart",
            "thermostat",
            "home automation"
          ],
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "11",
        "_score": 1,
        "_source": {
          "product_id": "P011",
          "name": "LED Desk Lamp",
          "description": "Adjustable LED desk lamp with touch control.",
          "category": "Lighting",
          "price": 24.99,
          "stock_quantity": 160,
          "supplier": "BrightLight",
          "release_date": "2023-11-25",
          "tags": [
            "led",
            "desk lamp",
            "lighting"
          ],
          "rating": 4.3
        }
      },
      {
        "_index": "product_catalog",
        "_id": "12",
        "_score": 1,
        "_source": {
          "product_id": "P012",
          "name": "Portable Hard Drive",
          "description": "1TB portable hard drive with fast data transfer.",
          "category": "Storage",
          "price": 64.99,
          "stock_quantity": 110,
          "supplier": "DataSafe",
          "release_date": "2023-12-05",
          "tags": [
            "portable",
            "hard drive",
            "storage"
          ],
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "13",
        "_score": 1,
        "_source": {
          "product_id": "P013",
          "name": "Coffee Maker",
          "description": "Automatic coffee maker with programmable settings.",
          "category": "Kitchen Appliances",
          "price": 79.99,
          "stock_quantity": 130,
          "supplier": "BrewMaster",
          "release_date": "2024-01-10",
          "tags": [
            "coffee",
            "maker",
            "appliances"
          ],
          "rating": 4.4
        }
      },
      {
        "_index": "product_catalog",
        "_id": "14",
        "_score": 1,
        "_source": {
          "product_id": "P014",
          "name": "Smart Light Bulb",
          "description": "Color changing smart light bulb with app control.",
          "category": "Lighting",
          "price": 14.99,
          "stock_quantity": 220,
          "supplier": "LightSmart",
          "release_date": "2024-02-14",
          "tags": [
            "smart",
            "light bulb",
            "lighting"
          ],
          "rating": 4.2
        }
      },
      {
        "_index": "product_catalog",
        "_id": "15",
        "_score": 1,
        "_source": {
          "product_id": "P015",
          "name": "Electric Kettle",
          "description": "Quick boil electric kettle with auto shut-off.",
          "category": "Kitchen Appliances",
          "price": 29.99,
          "stock_quantity": 170,
          "supplier": "QuickBoil",
          "release_date": "2024-03-20",
          "tags": [
            "electric",
            "kettle",
            "appliances"
          ],
          "rating": 4.5
        }
      },
      {
        "_index": "product_catalog",
        "_id": "16",
        "_score": 1,
        "_source": {
          "product_id": "P016",
          "name": "Robot Vacuum",
          "description": "Smart robot vacuum cleaner with scheduling features.",
          "category": "Home Appliances",
          "price": 199.99,
          "stock_quantity": 60,
          "supplier": "CleanBot",
          "release_date": "2024-04-25",
          "tags": [
            "robot",
            "vacuum",
            "home appliances"
          ],
          "rating": 4.8
        }
      },
      {
        "_index": "product_catalog",
        "_id": "17",
        "_score": 1,
        "_source": {
          "product_id": "P017",
          "name": "Digital Camera",
          "description": "Compact digital camera with high resolution.",
          "category": "Photography",
          "price": 149.99,
          "stock_quantity": 95,
          "supplier": "PhotoSnap",
          "release_date": "2024-05-30",
          "tags": [
            "digital",
            "camera",
            "photography"
          ],
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "18",
        "_score": 1,
        "_source": {
          "product_id": "P018",
          "name": "Smartwatch",
          "description": "Feature-rich smartwatch with fitness tracking.",
          "category": "Wearables",
          "price": 99.99,
          "stock_quantity": 140,
          "supplier": "WristTech",
          "release_date": "2024-06-15",
          "tags": [
            "smartwatch",
            "wearables",
            "fitness"
          ],
          "rating": 4.4
        }
      },
      {
        "_index": "product_catalog",
        "_id": "19",
        "_score": 1,
        "_source": {
          "product_id": "P019",
          "name": "Air Fryer",
          "description": "Healthier cooking with a digital air fryer.",
          "category": "Kitchen Appliances",
          "price": 89.99,
          "stock_quantity": 115,
          "supplier": "HealthyCook",
          "release_date": "2024-07-20",
          "tags": [
            "air fryer",
            "kitchen",
            "appliances"
          ],
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "20",
        "_score": 1,
        "_source": {
          "product_id": "P020",
          "name": "Electric Scooter",
          "description": "Eco-friendly electric scooter with long battery life.",
          "category": "Transportation",
          "price": 299.99,
          "stock_quantity": 50,
          "supplier": "EcoRide",
          "release_date": "2024-08-10",
          "tags": [
            "electric",
            "scooter",
            "transportation"
          ],
          "rating": 4.7
        }
      }
    ]
  }
}
```

导入order_records数据

```json
//批量导入order_records数据
POST /order_records/_bulk
{"index":{"_id":"1"}}
{"order_id":"OR001","customer_id":"C001","order_date":"2024-01-10","status":"completed","total_amount":150.75,"items":[{"product_id":"P001","quantity":2,"price":50},{"product_id":"P002","quantity":1,"price":50.75}],"shipping_address":"123 Main St, Anytown, USA","payment_method":"credit_card","shipping_date":"2024-01-11","delivery_date":"2024-01-15"}
{"index":{"_id":"2"}}
{"order_id":"OR002","customer_id":"C002","order_date":"2024-01-15","status":"pending","total_amount":89.99,"items":[{"product_id":"P003","quantity":1,"price":89.99}],"shipping_address":"456 Elm St, Othertown, USA","payment_method":"paypal","shipping_date":"2024-01-16","delivery_date":"2024-01-20"}
{"index":{"_id":"3"}}
{"order_id":"OR003","customer_id":"C003","order_date":"2024-01-20","status":"shipped","total_amount":120.5,"items":[{"product_id":"P004","quantity":3,"price":40}],"shipping_address":"789 Maple Ave, Sometown, USA","payment_method":"debit_card","shipping_date":"2024-01-21","delivery_date":"2024-01-25"}
{"index":{"_id":"4"}}
{"order_id":"OR004","customer_id":"C004","order_date":"2024-01-25","status":"completed","total_amount":75,"items":[{"product_id":"P005","quantity":5,"price":15}],"shipping_address":"101 Oak St, Anycity, USA","payment_method":"credit_card","shipping_date":"2024-01-26","delivery_date":"2024-01-30"}
{"index":{"_id":"5"}}
{"order_id":"OR005","customer_id":"C005","order_date":"2024-02-01","status":"cancelled","total_amount":200,"items":[{"product_id":"P006","quantity":2,"price":100}],"shipping_address":"202 Pine St, Otherville, USA","payment_method":"bank_transfer","shipping_date":"2024-02-02","delivery_date":"2024-02-06"}
{"index":{"_id":"6"}}
{"order_id":"OR006","customer_id":"C006","order_date":"2024-02-05","status":"completed","total_amount":300.25,"items":[{"product_id":"P007","quantity":1,"price":300.25}],"shipping_address":"303 Cedar Rd, Newtown, USA","payment_method":"credit_card","shipping_date":"2024-02-06","delivery_date":"2024-02-10"}
{"index":{"_id":"7"}}
{"order_id":"OR007","customer_id":"C007","order_date":"2024-02-10","status":"pending","total_amount":45.99,"items":[{"product_id":"P008","quantity":3,"price":15.33}],"shipping_address":"404 Birch Ln, Oldtown, USA","payment_method":"paypal","shipping_date":"2024-02-11","delivery_date":"2024-02-15"}
{"index":{"_id":"8"}}
{"order_id":"OR008","customer_id":"C008","order_date":"2024-02-15","status":"shipped","total_amount":89.5,"items":[{"product_id":"P009","quantity":2,"price":44.75}],"shipping_address":"505 Spruce St, Littletown, USA","payment_method":"debit_card","shipping_date":"2024-02-16","delivery_date":"2024-02-20"}
{"index":{"_id":"9"}}
{"order_id":"OR009","customer_id":"C009","order_date":"2024-02-20","status":"completed","total_amount":60,"items":[{"product_id":"P010","quantity":4,"price":15}],"shipping_address":"606 Willow Ave, Bigcity, USA","payment_method":"credit_card","shipping_date":"2024-02-21","delivery_date":"2024-02-25"}
{"index":{"_id":"10"}}
{"order_id":"OR010","customer_id":"C010","order_date":"2024-02-25","status":"cancelled","total_amount":150,"items":[{"product_id":"P011","quantity":10,"price":15}],"shipping_address":"707 Poplar Dr, Smalltown, USA","payment_method":"bank_transfer","shipping_date":"2024-02-26","delivery_date":"2024-03-01"}
{"index":{"_id":"11"}}
{"order_id":"OR011","customer_id":"C011","order_date":"2024-03-01","status":"completed","total_amount":110,"items":[{"product_id":"P012","quantity":2,"price":55}],"shipping_address":"808 Chestnut Blvd, Metropolis, USA","payment_method":"credit_card","shipping_date":"2024-03-02","delivery_date":"2024-03-06"}
{"index":{"_id":"12"}}
{"order_id":"OR012","customer_id":"C012","order_date":"2024-03-05","status":"pending","total_amount":75.99,"items":[{"product_id":"P013","quantity":3,"price":25.33}],"shipping_address":"909 Ash Ct, Villagetown, USA","payment_method":"paypal","shipping_date":"2024-03-06","delivery_date":"2024-03-10"}
{"index":{"_id":"13"}}
{"order_id":"OR013","customer_id":"C013","order_date":"2024-03-10","status":"shipped","total_amount":200,"items":[{"product_id":"P014","quantity":4,"price":50}],"shipping_address":"1010 Fir Ln, Hamlet, USA","payment_method":"debit_card","shipping_date":"2024-03-11","delivery_date":"2024-03-15"}
{"index":{"_id":"14"}}
{"order_id":"OR014","customer_id":"C014","order_date":"2024-03-15","status":"completed","total_amount":90,"items":[{"product_id":"P015","quantity":6,"price":15}],"shipping_address":"1111 Cypress St, Township, USA","payment_method":"credit_card","shipping_date":"2024-03-16","delivery_date":"2024-03-20"}
{"index":{"_id":"15"}}
{"order_id":"OR015","customer_id":"C015","order_date":"2024-03-20","status":"cancelled","total_amount":135,"items":[{"product_id":"P016","quantity":9,"price":15}],"shipping_address":"1212 Redwood Rd, Cityville, USA","payment_method":"bank_transfer","shipping_date":"2024-03-21","delivery_date":"2024-03-25"}
{"index":{"_id":"16"}}
{"order_id":"OR016","customer_id":"C016","order_date":"2024-03-25","status":"completed","total_amount":250,"items":[{"product_id":"P017","quantity":5,"price":50}],"shipping_address":"1313 Cherry Ave, Urbantown, USA","payment_method":"credit_card","shipping_date":"2024-03-26","delivery_date":"2024-03-30"}
{"index":{"_id":"17"}}
{"order_id":"OR017","customer_id":"C017","order_date":"2024-03-30","status":"pending","total_amount":60,"items":[{"product_id":"P018","quantity":2,"price":30}],"shipping_address":"1414 Beech St, Suburbia, USA","payment_method":"paypal","shipping_date":"2024-03-31","delivery_date":"2024-04-04"}
{"index":{"_id":"18"}}
{"order_id":"OR018","customer_id":"C018","order_date":"2024-04-04","status":"shipped","total_amount":100,"items":[{"product_id":"P019","quantity":4,"price":25}],"shipping_address":"1515 Palm Dr, Downtown, USA","payment_method":"debit_card","shipping_date":"2024-04-05","delivery_date":"2024-04-09"}
{"index":{"_id":"19"}}
{"order_id":"OR019","customer_id":"C019","order_date":"2024-04-09","status":"completed","total_amount":180,"items":[{"product_id":"P020","quantity":6,"price":30}],"shipping_address":"1616 Pine St, Uptown, USA","payment_method":"credit_card","shipping_date":"2024-04-10","delivery_date":"2024-04-14"}
{"index":{"_id":"20"}}
{"order_id":"OR020","customer_id":"C020","order_date":"2024-04-14","status":"cancelled","total_amount":220,"items":[{"product_id":"P021","quantity":11,"price":20}],"shipping_address":"1717 Maple Ave, Seaside, USA","payment_method":"bank_transfer","shipping_date":"2024-04-15","delivery_date":"2024-04-19"}

```

![image-20240926125923366](./assets/image-20240926125923366.png)

查看导入数据

```json
GET /order_records/_search
{
  "query": {
    "match_all": {}
  },
  "size":20
}
```

![image-20240926125944253](./assets/image-20240926125944253.png)

导入结果

```json
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 20,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 1,
        "_source": {
          "order_id": "OR001",
          "customer_id": "C001",
          "order_date": "2024-01-10",
          "status": "completed",
          "total_amount": 150.75,
          "items": [
            {
              "product_id": "P001",
              "quantity": 2,
              "price": 50
            },
            {
              "product_id": "P002",
              "quantity": 1,
              "price": 50.75
            }
          ],
          "shipping_address": "123 Main St, Anytown, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-01-11",
          "delivery_date": "2024-01-15"
        }
      },
      {
        "_index": "order_records",
        "_id": "2",
        "_score": 1,
        "_source": {
          "order_id": "OR002",
          "customer_id": "C002",
          "order_date": "2024-01-15",
          "status": "pending",
          "total_amount": 89.99,
          "items": [
            {
              "product_id": "P003",
              "quantity": 1,
              "price": 89.99
            }
          ],
          "shipping_address": "456 Elm St, Othertown, USA",
          "payment_method": "paypal",
          "shipping_date": "2024-01-16",
          "delivery_date": "2024-01-20"
        }
      },
      {
        "_index": "order_records",
        "_id": "3",
        "_score": 1,
        "_source": {
          "order_id": "OR003",
          "customer_id": "C003",
          "order_date": "2024-01-20",
          "status": "shipped",
          "total_amount": 120.5,
          "items": [
            {
              "product_id": "P004",
              "quantity": 3,
              "price": 40
            }
          ],
          "shipping_address": "789 Maple Ave, Sometown, USA",
          "payment_method": "debit_card",
          "shipping_date": "2024-01-21",
          "delivery_date": "2024-01-25"
        }
      },
      {
        "_index": "order_records",
        "_id": "4",
        "_score": 1,
        "_source": {
          "order_id": "OR004",
          "customer_id": "C004",
          "order_date": "2024-01-25",
          "status": "completed",
          "total_amount": 75,
          "items": [
            {
              "product_id": "P005",
              "quantity": 5,
              "price": 15
            }
          ],
          "shipping_address": "101 Oak St, Anycity, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-01-26",
          "delivery_date": "2024-01-30"
        }
      },
      {
        "_index": "order_records",
        "_id": "5",
        "_score": 1,
        "_source": {
          "order_id": "OR005",
          "customer_id": "C005",
          "order_date": "2024-02-01",
          "status": "cancelled",
          "total_amount": 200,
          "items": [
            {
              "product_id": "P006",
              "quantity": 2,
              "price": 100
            }
          ],
          "shipping_address": "202 Pine St, Otherville, USA",
          "payment_method": "bank_transfer",
          "shipping_date": "2024-02-02",
          "delivery_date": "2024-02-06"
        }
      },
      {
        "_index": "order_records",
        "_id": "6",
        "_score": 1,
        "_source": {
          "order_id": "OR006",
          "customer_id": "C006",
          "order_date": "2024-02-05",
          "status": "completed",
          "total_amount": 300.25,
          "items": [
            {
              "product_id": "P007",
              "quantity": 1,
              "price": 300.25
            }
          ],
          "shipping_address": "303 Cedar Rd, Newtown, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-02-06",
          "delivery_date": "2024-02-10"
        }
      },
      {
        "_index": "order_records",
        "_id": "7",
        "_score": 1,
        "_source": {
          "order_id": "OR007",
          "customer_id": "C007",
          "order_date": "2024-02-10",
          "status": "pending",
          "total_amount": 45.99,
          "items": [
            {
              "product_id": "P008",
              "quantity": 3,
              "price": 15.33
            }
          ],
          "shipping_address": "404 Birch Ln, Oldtown, USA",
          "payment_method": "paypal",
          "shipping_date": "2024-02-11",
          "delivery_date": "2024-02-15"
        }
      },
      {
        "_index": "order_records",
        "_id": "8",
        "_score": 1,
        "_source": {
          "order_id": "OR008",
          "customer_id": "C008",
          "order_date": "2024-02-15",
          "status": "shipped",
          "total_amount": 89.5,
          "items": [
            {
              "product_id": "P009",
              "quantity": 2,
              "price": 44.75
            }
          ],
          "shipping_address": "505 Spruce St, Littletown, USA",
          "payment_method": "debit_card",
          "shipping_date": "2024-02-16",
          "delivery_date": "2024-02-20"
        }
      },
      {
        "_index": "order_records",
        "_id": "9",
        "_score": 1,
        "_source": {
          "order_id": "OR009",
          "customer_id": "C009",
          "order_date": "2024-02-20",
          "status": "completed",
          "total_amount": 60,
          "items": [
            {
              "product_id": "P010",
              "quantity": 4,
              "price": 15
            }
          ],
          "shipping_address": "606 Willow Ave, Bigcity, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-02-21",
          "delivery_date": "2024-02-25"
        }
      },
      {
        "_index": "order_records",
        "_id": "10",
        "_score": 1,
        "_source": {
          "order_id": "OR010",
          "customer_id": "C010",
          "order_date": "2024-02-25",
          "status": "cancelled",
          "total_amount": 150,
          "items": [
            {
              "product_id": "P011",
              "quantity": 10,
              "price": 15
            }
          ],
          "shipping_address": "707 Poplar Dr, Smalltown, USA",
          "payment_method": "bank_transfer",
          "shipping_date": "2024-02-26",
          "delivery_date": "2024-03-01"
        }
      },
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 1,
        "_source": {
          "order_id": "OR011",
          "customer_id": "C011",
          "order_date": "2024-03-01",
          "status": "completed",
          "total_amount": 110,
          "items": [
            {
              "product_id": "P012",
              "quantity": 2,
              "price": 55
            }
          ],
          "shipping_address": "808 Chestnut Blvd, Metropolis, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-03-02",
          "delivery_date": "2024-03-06"
        }
      },
      {
        "_index": "order_records",
        "_id": "12",
        "_score": 1,
        "_source": {
          "order_id": "OR012",
          "customer_id": "C012",
          "order_date": "2024-03-05",
          "status": "pending",
          "total_amount": 75.99,
          "items": [
            {
              "product_id": "P013",
              "quantity": 3,
              "price": 25.33
            }
          ],
          "shipping_address": "909 Ash Ct, Villagetown, USA",
          "payment_method": "paypal",
          "shipping_date": "2024-03-06",
          "delivery_date": "2024-03-10"
        }
      },
      {
        "_index": "order_records",
        "_id": "13",
        "_score": 1,
        "_source": {
          "order_id": "OR013",
          "customer_id": "C013",
          "order_date": "2024-03-10",
          "status": "shipped",
          "total_amount": 200,
          "items": [
            {
              "product_id": "P014",
              "quantity": 4,
              "price": 50
            }
          ],
          "shipping_address": "1010 Fir Ln, Hamlet, USA",
          "payment_method": "debit_card",
          "shipping_date": "2024-03-11",
          "delivery_date": "2024-03-15"
        }
      },
      {
        "_index": "order_records",
        "_id": "14",
        "_score": 1,
        "_source": {
          "order_id": "OR014",
          "customer_id": "C014",
          "order_date": "2024-03-15",
          "status": "completed",
          "total_amount": 90,
          "items": [
            {
              "product_id": "P015",
              "quantity": 6,
              "price": 15
            }
          ],
          "shipping_address": "1111 Cypress St, Township, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-03-16",
          "delivery_date": "2024-03-20"
        }
      },
      {
        "_index": "order_records",
        "_id": "15",
        "_score": 1,
        "_source": {
          "order_id": "OR015",
          "customer_id": "C015",
          "order_date": "2024-03-20",
          "status": "cancelled",
          "total_amount": 135,
          "items": [
            {
              "product_id": "P016",
              "quantity": 9,
              "price": 15
            }
          ],
          "shipping_address": "1212 Redwood Rd, Cityville, USA",
          "payment_method": "bank_transfer",
          "shipping_date": "2024-03-21",
          "delivery_date": "2024-03-25"
        }
      },
      {
        "_index": "order_records",
        "_id": "16",
        "_score": 1,
        "_source": {
          "order_id": "OR016",
          "customer_id": "C016",
          "order_date": "2024-03-25",
          "status": "completed",
          "total_amount": 250,
          "items": [
            {
              "product_id": "P017",
              "quantity": 5,
              "price": 50
            }
          ],
          "shipping_address": "1313 Cherry Ave, Urbantown, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-03-26",
          "delivery_date": "2024-03-30"
        }
      },
      {
        "_index": "order_records",
        "_id": "17",
        "_score": 1,
        "_source": {
          "order_id": "OR017",
          "customer_id": "C017",
          "order_date": "2024-03-30",
          "status": "pending",
          "total_amount": 60,
          "items": [
            {
              "product_id": "P018",
              "quantity": 2,
              "price": 30
            }
          ],
          "shipping_address": "1414 Beech St, Suburbia, USA",
          "payment_method": "paypal",
          "shipping_date": "2024-03-31",
          "delivery_date": "2024-04-04"
        }
      },
      {
        "_index": "order_records",
        "_id": "18",
        "_score": 1,
        "_source": {
          "order_id": "OR018",
          "customer_id": "C018",
          "order_date": "2024-04-04",
          "status": "shipped",
          "total_amount": 100,
          "items": [
            {
              "product_id": "P019",
              "quantity": 4,
              "price": 25
            }
          ],
          "shipping_address": "1515 Palm Dr, Downtown, USA",
          "payment_method": "debit_card",
          "shipping_date": "2024-04-05",
          "delivery_date": "2024-04-09"
        }
      },
      {
        "_index": "order_records",
        "_id": "19",
        "_score": 1,
        "_source": {
          "order_id": "OR019",
          "customer_id": "C019",
          "order_date": "2024-04-09",
          "status": "completed",
          "total_amount": 180,
          "items": [
            {
              "product_id": "P020",
              "quantity": 6,
              "price": 30
            }
          ],
          "shipping_address": "1616 Pine St, Uptown, USA",
          "payment_method": "credit_card",
          "shipping_date": "2024-04-10",
          "delivery_date": "2024-04-14"
        }
      },
      {
        "_index": "order_records",
        "_id": "20",
        "_score": 1,
        "_source": {
          "order_id": "OR020",
          "customer_id": "C020",
          "order_date": "2024-04-14",
          "status": "cancelled",
          "total_amount": 220,
          "items": [
            {
              "product_id": "P021",
              "quantity": 11,
              "price": 20
            }
          ],
          "shipping_address": "1717 Maple Ave, Seaside, USA",
          "payment_method": "bank_transfer",
          "shipping_date": "2024-04-15",
          "delivery_date": "2024-04-19"
        }
      }
    ]
  }
}
```



## 任务三

### 用户信息数据查询


1. **查询所有女性用户的姓名和电子邮件。**

```json
GET /user_information/_search
{
  "query": {
    "match": {
      "gender": "female"
    }
  },
  "_source": ["name", "email"]
}
```

查询结果

![image-20240926130622614](./assets/image-20240926130622614.png)

```json
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 11,
      "relation": "eq"
    },
    "max_score": 0.6486954,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 0.6486954,
        "_source": {
          "name": "Alice Johnson",
          "email": "alice.johnson@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 0.6486954,
        "_source": {
          "name": "Eve Davis",
          "email": "eve.davis@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 0.6486954,
        "_source": {
          "name": "Grace Lee",
          "email": "grace.lee@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 0.6486954,
        "_source": {
          "name": "Hannah White",
          "email": "hannah.white@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 0.6486954,
        "_source": {
          "name": "Ivy Green",
          "email": "ivy.green@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 0.6486954,
        "_source": {
          "name": "Karen Adams",
          "email": "karen.adams@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "013",
        "_score": 0.6486954,
        "_source": {
          "name": "Mia Moore",
          "email": "mia.moore@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "014",
        "_score": 0.6486954,
        "_source": {
          "name": "Nina Scott",
          "email": "nina.scott@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "018",
        "_score": 0.6486954,
        "_source": {
          "name": "Rose Hall",
          "email": "rose.hall@example.com"
        }
      },
      {
        "_index": "user_information",
        "_id": "020",
        "_score": 0.6486954,
        "_source": {
          "name": "Tina Brown",
          "email": "tina.brown@example.com"
        }
      }
    ]
  }
}
```

2. **查找最后登录日期在2024年9月1日之后的所有活跃用户。**

```json
GET /user_information/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "last_login": {
              "gte": "2024-09-01"
            }
          }
        },
        {
          "term": {
            "status": "active"
          }
        }
      ]
    }
  },"_source": ["name","last_login","status"]
}
```

查询结果

![image-20240926130655947](./assets/image-20240926130655947.png)

```json
{
  "took": 7,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 13,
      "relation": "eq"
    },
    "max_score": 1.3502024,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 1.3502024,
        "_source": {
          "name": "Alice Johnson",
          "last_login": "2024-09-01",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 1.3502024,
        "_source": {
          "name": "Eve Davis",
          "last_login": "2024-09-02",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "006",
        "_score": 1.3502024,
        "_source": {
          "name": "Frank Miller",
          "last_login": "2024-09-03",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 1.3502024,
        "_source": {
          "name": "Hannah White",
          "last_login": "2024-09-06",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 1.3502024,
        "_source": {
          "name": "Ivy Green",
          "last_login": "2024-09-07",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 1.3502024,
        "_source": {
          "name": "Karen Adams",
          "last_login": "2024-09-09",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "012",
        "_score": 1.3502024,
        "_source": {
          "name": "Leo King",
          "last_login": "2024-09-10",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "014",
        "_score": 1.3502024,
        "_source": {
          "name": "Nina Scott",
          "last_login": "2024-09-12",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "015",
        "_score": 1.3502024,
        "_source": {
          "name": "Oscar Turner",
          "last_login": "2024-09-13",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "017",
        "_score": 1.3502024,
        "_source": {
          "name": "Quinn Hughes",
          "last_login": "2024-09-15",
          "status": "active"
        }
      }
    ]
  }
}
```

3. **查询住在"Anytown"的用户。**

```json
GET /user_information/_search
{
  "query": {
    "match": {
      "address": "Anytown"
    }
  }
}
```

查询结果

![image-20240926130712139](./assets/image-20240926130712139.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 2,
      "relation": "eq"
    },
    "max_score": 2.1747518,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 2.1747518,
        "_source": {
          "user_id": "001",
          "name": "Alice Johnson",
          "email": "alice.johnson@example.com",
          "date_of_birth": "1990-05-15",
          "gender": "female",
          "address": "123 Main St, Anytown, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-01-15",
          "last_login": "2024-09-01",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "1",
        "_score": 2.1747518,
        "_source": {
          "user_id": "001",
          "name": "Alice Johnson",
          "email": "alice.johnson@example.com",
          "date_of_birth": "1990-05-15",
          "gender": "female",
          "address": "123 Main St, Anytown, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-01-15",
          "last_login": "2024-09-01",
          "status": "active"
        }
      }
    ]
  }
}
```

4. **查找出生日期在1990年之后的所有用户。**

```json
GET /user_information/_search
{
  "query": {
    "range": {
      "date_of_birth": {
        "gt": "1990-01-01"
      }
    }
  },
  "_source": [
    "name",
    "date_of_birth"
  ]
}
```

查询结果

![image-20240926130732311](./assets/image-20240926130732311.png)

```json
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 13,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 1,
        "_source": {
          "name": "Alice Johnson",
          "date_of_birth": "1990-05-15"
        }
      },
      {
        "_index": "user_information",
        "_id": "003",
        "_score": 1,
        "_source": {
          "name": "Charlie Brown",
          "date_of_birth": "1992-11-30"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 1,
        "_source": {
          "name": "Eve Davis",
          "date_of_birth": "1995-07-22"
        }
      },
      {
        "_index": "user_information",
        "_id": "006",
        "_score": 1,
        "_source": {
          "name": "Frank Miller",
          "date_of_birth": "1991-10-05"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 1,
        "_source": {
          "name": "Hannah White",
          "date_of_birth": "1993-12-25"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 1,
        "_source": {
          "name": "Ivy Green",
          "date_of_birth": "1994-03-03"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 1,
        "_source": {
          "name": "Karen Adams",
          "date_of_birth": "1990-09-21"
        }
      },
      {
        "_index": "user_information",
        "_id": "013",
        "_score": 1,
        "_source": {
          "name": "Mia Moore",
          "date_of_birth": "1992-02-28"
        }
      },
      {
        "_index": "user_information",
        "_id": "015",
        "_score": 1,
        "_source": {
          "name": "Oscar Turner",
          "date_of_birth": "1991-07-19"
        }
      },
      {
        "_index": "user_information",
        "_id": "017",
        "_score": 1,
        "_source": {
          "name": "Quinn Hughes",
          "date_of_birth": "1993-11-11"
        }
      }
    ]
  }
}
```

5. **查询所有状态为"inactive"的用户。**

```json
GET /user_information/_search
{
  "query": {
    "match": {
      "status":"inactive"
    }
  },
  "_source": [
    "name",
    "status"
  ]
}
```

查询结果

![image-20240926130821860](./assets/image-20240926130821860.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 6,
      "relation": "eq"
    },
    "max_score": 1.2192403,
    "hits": [
      {
        "_index": "user_information",
        "_id": "003",
        "_score": 1.2192403,
        "_source": {
          "name": "Charlie Brown",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 1.2192403,
        "_source": {
          "name": "Grace Lee",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "010",
        "_score": 1.2192403,
        "_source": {
          "name": "Jack Black",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "013",
        "_score": 1.2192403,
        "_source": {
          "name": "Mia Moore",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "016",
        "_score": 1.2192403,
        "_source": {
          "name": "Paul Walker",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "019",
        "_score": 1.2192403,
        "_source": {
          "name": "Steve Young",
          "status": "inactive"
        }
      }
    ]
  }
}
```

6. **查找注册日期在2023年1月1日到2023年12月31日之间的用户。**

```json
GET /user_information/_search
{
  "query": {
    "range": {
      "registration_date": {
        "gte": "2023-01-01",
        "lte": "2023-12-31"
      }
    }
  },
  "_source": [
    "name",
    "registration_date"
  ]
}
```

查询结果

![image-20240926130838790](./assets/image-20240926130838790.png)

```json
{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 13,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 1,
        "_source": {
          "name": "Alice Johnson",
          "registration_date": "2023-01-15"
        }
      },
      {
        "_index": "user_information",
        "_id": "002",
        "_score": 1,
        "_source": {
          "name": "Bob Smith",
          "registration_date": "2023-02-20"
        }
      },
      {
        "_index": "user_information",
        "_id": "003",
        "_score": 1,
        "_source": {
          "name": "Charlie Brown",
          "registration_date": "2023-03-10"
        }
      },
      {
        "_index": "user_information",
        "_id": "004",
        "_score": 1,
        "_source": {
          "name": "David Wilson",
          "registration_date": "2023-04-05"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 1,
        "_source": {
          "name": "Eve Davis",
          "registration_date": "2023-05-18"
        }
      },
      {
        "_index": "user_information",
        "_id": "006",
        "_score": 1,
        "_source": {
          "name": "Frank Miller",
          "registration_date": "2023-06-22"
        }
      },
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 1,
        "_source": {
          "name": "Grace Lee",
          "registration_date": "2023-07-30"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 1,
        "_source": {
          "name": "Hannah White",
          "registration_date": "2023-08-15"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 1,
        "_source": {
          "name": "Ivy Green",
          "registration_date": "2023-09-01"
        }
      },
      {
        "_index": "user_information",
        "_id": "010",
        "_score": 1,
        "_source": {
          "name": "Jack Black",
          "registration_date": "2023-10-10"
        }
      }
    ]
  }
}
```

7. **查询名字为"Bob Smith"的用户的详细信息。**

```json
GET /user_information/_search
{
  "query": {
    "match": {
      "name":"Bob Smith"
    }
  }
}
```

查询结果

![image-20240926130852623](./assets/image-20240926130852623.png)

```json
{
  "took": 6,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 5.371155,
    "hits": [
      {
        "_index": "user_information",
        "_id": "002",
        "_score": 5.371155,
        "_source": {
          "user_id": "002",
          "name": "Bob Smith",
          "email": "bob.smith@example.com",
          "date_of_birth": "1985-08-20",
          "gender": "male",
          "address": "456 Elm St, Othertown, USA",
          "phone_number": "234-567-8901",
          "registration_date": "2023-02-20",
          "last_login": "2024-08-25",
          "status": "active"
        }
      }
    ]
  }
}
```

8. **查找电话号码以"123"开头的用户。**

```json
GET /user_information/_search
{
  "query": {
    "wildcard": {
      "phone_number": {
        "value": "123*"
      }
    }
  },
  "_source": [
    "name",
    "phone_number"
  ]
}
```

查询结果

![image-20240926130905298](./assets/image-20240926130905298.png)

```json
{
  "took": 4,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 7,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 1,
        "_source": {
          "name": "Alice Johnson",
          "phone_number": "123-456-7890"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 1,
        "_source": {
          "name": "Eve Davis",
          "phone_number": "567-890-1234"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 1,
        "_source": {
          "name": "Hannah White",
          "phone_number": "890-123-4567"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 1,
        "_source": {
          "name": "Karen Adams",
          "phone_number": "123-456-7890"
        }
      },
      {
        "_index": "user_information",
        "_id": "015",
        "_score": 1,
        "_source": {
          "name": "Oscar Turner",
          "phone_number": "567-890-1234"
        }
      },
      {
        "_index": "user_information",
        "_id": "018",
        "_score": 1,
        "_source": {
          "name": "Rose Hall",
          "phone_number": "890-123-4567"
        }
      },
      {
        "_index": "user_information",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "Alice Johnson",
          "phone_number": "123-456-7890"
        }
      }
    ]
  }
}
```

9. **查询电子邮件域为"example.com"的所有用户。**

```json
GET /user_information/_search
{
  "query": {
    "match_phrase": {
      "email": "example.com"
    }
  },
      "size": 20
}
```

查询结果

![image-20240926130919070](./assets/image-20240926130919070.png)

```json
{
  "took": 5,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 21,
      "relation": "eq"
    },
    "max_score": 0.022989519,
    "hits": [
      {
        "_index": "user_information",
        "_id": "001",
        "_score": 0.022989519,
        "_source": {
          "user_id": "001",
          "name": "Alice Johnson",
          "email": "alice.johnson@example.com",
          "date_of_birth": "1990-05-15",
          "gender": "female",
          "address": "123 Main St, Anytown, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-01-15",
          "last_login": "2024-09-01",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "002",
        "_score": 0.022989519,
        "_source": {
          "user_id": "002",
          "name": "Bob Smith",
          "email": "bob.smith@example.com",
          "date_of_birth": "1985-08-20",
          "gender": "male",
          "address": "456 Elm St, Othertown, USA",
          "phone_number": "234-567-8901",
          "registration_date": "2023-02-20",
          "last_login": "2024-08-25",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "003",
        "_score": 0.022989519,
        "_source": {
          "user_id": "003",
          "name": "Charlie Brown",
          "email": "charlie.brown@example.com",
          "date_of_birth": "1992-11-30",
          "gender": "male",
          "address": "789 Maple Ave, Sometown, USA",
          "phone_number": "345-678-9012",
          "registration_date": "2023-03-10",
          "last_login": "2024-09-05",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "004",
        "_score": 0.022989519,
        "_source": {
          "user_id": "004",
          "name": "David Wilson",
          "email": "david.wilson@example.com",
          "date_of_birth": "1988-02-14",
          "gender": "male",
          "address": "101 Oak St, Anycity, USA",
          "phone_number": "456-789-0123",
          "registration_date": "2023-04-05",
          "last_login": "2024-08-30",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "005",
        "_score": 0.022989519,
        "_source": {
          "user_id": "005",
          "name": "Eve Davis",
          "email": "eve.davis@example.com",
          "date_of_birth": "1995-07-22",
          "gender": "female",
          "address": "202 Pine St, Otherville, USA",
          "phone_number": "567-890-1234",
          "registration_date": "2023-05-18",
          "last_login": "2024-09-02",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "006",
        "_score": 0.022989519,
        "_source": {
          "user_id": "006",
          "name": "Frank Miller",
          "email": "frank.miller@example.com",
          "date_of_birth": "1991-10-05",
          "gender": "male",
          "address": "303 Cedar Rd, Newtown, USA",
          "phone_number": "678-901-2345",
          "registration_date": "2023-06-22",
          "last_login": "2024-09-03",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 0.022989519,
        "_source": {
          "user_id": "007",
          "name": "Grace Lee",
          "email": "grace.lee@example.com",
          "date_of_birth": "1989-04-17",
          "gender": "female",
          "address": "404 Birch Ln, Oldtown, USA",
          "phone_number": "789-012-3456",
          "registration_date": "2023-07-30",
          "last_login": "2024-09-04",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "008",
        "_score": 0.022989519,
        "_source": {
          "user_id": "008",
          "name": "Hannah White",
          "email": "hannah.white@example.com",
          "date_of_birth": "1993-12-25",
          "gender": "female",
          "address": "505 Spruce St, Littletown, USA",
          "phone_number": "890-123-4567",
          "registration_date": "2023-08-15",
          "last_login": "2024-09-06",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "009",
        "_score": 0.022989519,
        "_source": {
          "user_id": "009",
          "name": "Ivy Green",
          "email": "ivy.green@example.com",
          "date_of_birth": "1994-03-03",
          "gender": "female",
          "address": "606 Willow Ave, Bigcity, USA",
          "phone_number": "901-234-5678",
          "registration_date": "2023-09-01",
          "last_login": "2024-09-07",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "010",
        "_score": 0.022989519,
        "_source": {
          "user_id": "010",
          "name": "Jack Black",
          "email": "jack.black@example.com",
          "date_of_birth": "1987-06-18",
          "gender": "male",
          "address": "707 Poplar Dr, Smalltown, USA",
          "phone_number": "012-345-6789",
          "registration_date": "2023-10-10",
          "last_login": "2024-09-08",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "011",
        "_score": 0.022989519,
        "_source": {
          "user_id": "011",
          "name": "Karen Adams",
          "email": "karen.adams@example.com",
          "date_of_birth": "1990-09-21",
          "gender": "female",
          "address": "808 Chestnut Blvd, Metropolis, USA",
          "phone_number": "123-456-7890",
          "registration_date": "2023-11-25",
          "last_login": "2024-09-09",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "012",
        "_score": 0.022989519,
        "_source": {
          "user_id": "012",
          "name": "Leo King",
          "email": "leo.king@example.com",
          "date_of_birth": "1986-01-12",
          "gender": "male",
          "address": "909 Ash Ct, Villagetown, USA",
          "phone_number": "234-567-8901",
          "registration_date": "2023-12-05",
          "last_login": "2024-09-10",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "013",
        "_score": 0.022989519,
        "_source": {
          "user_id": "013",
          "name": "Mia Moore",
          "email": "mia.moore@example.com",
          "date_of_birth": "1992-02-28",
          "gender": "female",
          "address": "1010 Fir Ln, Hamlet, USA",
          "phone_number": "345-678-9012",
          "registration_date": "2024-01-10",
          "last_login": "2024-09-11",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "014",
        "_score": 0.022989519,
        "_source": {
          "user_id": "014",
          "name": "Nina Scott",
          "email": "nina.scott@example.com",
          "date_of_birth": "1988-05-05",
          "gender": "female",
          "address": "1111 Cypress St, Township, USA",
          "phone_number": "456-789-0123",
          "registration_date": "2024-02-14",
          "last_login": "2024-09-12",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "015",
        "_score": 0.022989519,
        "_source": {
          "user_id": "015",
          "name": "Oscar Turner",
          "email": "oscar.turner@example.com",
          "date_of_birth": "1991-07-19",
          "gender": "male",
          "address": "1212 Redwood Rd, Cityville, USA",
          "phone_number": "567-890-1234",
          "registration_date": "2024-03-20",
          "last_login": "2024-09-13",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "016",
        "_score": 0.022989519,
        "_source": {
          "user_id": "016",
          "name": "Paul Walker",
          "email": "paul.walker@example.com",
          "date_of_birth": "1989-10-10",
          "gender": "male",
          "address": "1313 Cherry Ave, Urbantown, USA",
          "phone_number": "678-901-2345",
          "registration_date": "2024-04-25",
          "last_login": "2024-09-14",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "017",
        "_score": 0.022989519,
        "_source": {
          "user_id": "017",
          "name": "Quinn Hughes",
          "email": "quinn.hughes@example.com",
          "date_of_birth": "1993-11-11",
          "gender": "male",
          "address": "1414 Beech St, Suburbia, USA",
          "phone_number": "789-012-3456",
          "registration_date": "2024-05-30",
          "last_login": "2024-09-15",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "018",
        "_score": 0.022989519,
        "_source": {
          "user_id": "018",
          "name": "Rose Hall",
          "email": "rose.hall@example.com",
          "date_of_birth": "1990-03-22",
          "gender": "female",
          "address": "1515 Palm Dr, Downtown, USA",
          "phone_number": "890-123-4567",
          "registration_date": "2024-06-15",
          "last_login": "2024-09-16",
          "status": "active"
        }
      },
      {
        "_index": "user_information",
        "_id": "019",
        "_score": 0.022989519,
        "_source": {
          "user_id": "019",
          "name": "Steve Young",
          "email": "steve.young@example.com",
          "date_of_birth": "1987-09-09",
          "gender": "male",
          "address": "1616 Pine St, Uptown, USA",
          "phone_number": "901-234-5678",
          "registration_date": "2024-07-20",
          "last_login": "2024-09-17",
          "status": "inactive"
        }
      },
      {
        "_index": "user_information",
        "_id": "020",
        "_score": 0.022989519,
        "_source": {
          "user_id": "020",
          "name": "Tina Brown",
          "email": "tina.brown@example.com",
          "date_of_birth": "1994-12-12",
          "gender": "female",
          "address": "1717 Maple Ave, Seaside, USA",
          "phone_number": "012-345-6789",
          "registration_date": "2024-08-10",
          "last_login": "2024-09-18",
          "status": "active"
        }
      }
    ]
  }
}
```

10. **查找所有名字中包含"Lee"的用户。**

```json
GET /user_information/_search
{
  "query": {
    "match_phrase": {
      "name": "Lee"
    }
  },
      "size": 20
}
```

查询结果

![image-20240926130936064](./assets/image-20240926130936064.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 2.6855774,
    "hits": [
      {
        "_index": "user_information",
        "_id": "007",
        "_score": 2.6855774,
        "_source": {
          "user_id": "007",
          "name": "Grace Lee",
          "email": "grace.lee@example.com",
          "date_of_birth": "1989-04-17",
          "gender": "female",
          "address": "404 Birch Ln, Oldtown, USA",
          "phone_number": "789-012-3456",
          "registration_date": "2023-07-30",
          "last_login": "2024-09-04",
          "status": "inactive"
        }
      }
    ]
  }
}
```


### 产品目录数据查询


1. **查询所有类别为"Audio"的产品名称和价格。**

```json
{
  "query":{
    "match": {
      "category": "Audio"
    }
  },
  "_source": ["name","price"]
}
```

查询结果

![image-20240926131154129](./assets/image-20240926131154129.png)

```json
{
  "took": 10,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 2,
      "relation": "eq"
    },
    "max_score": 2.1282315,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "2",
        "_score": 2.1282315,
        "_source": {
          "name": "Bluetooth Speaker",
          "price": 49.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "5",
        "_score": 2.1282315,
        "_source": {
          "name": "Noise Cancelling Headphones",
          "price": 89.99
        }
      }
    ]
  }
}
```

2. **查找价格高于50美元的所有产品。**

```json
GET /product_catalog/_search
{
  "query": {
    "range": {
      "price": {
        "gt": 50
      }
    }
  },
  "_source": ["name","price"]
}
```

查询结果

![image-20240926131213493](./assets/image-20240926131213493.png)

```json
{
  "took": 4,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 11,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "5",
        "_score": 1,
        "_source": {
          "name": "Noise Cancelling Headphones",
          "price": 89.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "6",
        "_score": 1,
        "_source": {
          "name": "4K Monitor",
          "price": 299.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "7",
        "_score": 1,
        "_source": {
          "name": "Gaming Keyboard",
          "price": 59.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1,
        "_source": {
          "name": "Smart Thermostat",
          "price": 129.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "12",
        "_score": 1,
        "_source": {
          "name": "Portable Hard Drive",
          "price": 64.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "13",
        "_score": 1,
        "_source": {
          "name": "Coffee Maker",
          "price": 79.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "16",
        "_score": 1,
        "_source": {
          "name": "Robot Vacuum",
          "price": 199.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "17",
        "_score": 1,
        "_source": {
          "name": "Digital Camera",
          "price": 149.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "18",
        "_score": 1,
        "_source": {
          "name": "Smartwatch",
          "price": 99.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "19",
        "_score": 1,
        "_source": {
          "name": "Air Fryer",
          "price": 89.99
        }
      }
    ]
  }
}
```

3. **查询库存数量少于100的产品。**

```json
GET /product_catalog/_search{
  "query": {
    "range": {
      "stock_quantity": {
        "lt": 100
      }
    }
  },
  "_source": ["name","stock_quantity"]
}
```

查询结果

![image-20240926131235402](./assets/image-20240926131235402.png)

```json
{
  "took": 4,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 5,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "6",
        "_score": 1,
        "_source": {
          "name": "4K Monitor",
          "stock_quantity": 80
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1,
        "_source": {
          "name": "Smart Thermostat",
          "stock_quantity": 90
        }
      },
      {
        "_index": "product_catalog",
        "_id": "16",
        "_score": 1,
        "_source": {
          "name": "Robot Vacuum",
          "stock_quantity": 60
        }
      },
      {
        "_index": "product_catalog",
        "_id": "17",
        "_score": 1,
        "_source": {
          "name": "Digital Camera",
          "stock_quantity": 95
        }
      },
      {
        "_index": "product_catalog",
        "_id": "20",
        "_score": 1,
        "_source": {
          "name": "Electric Scooter",
          "stock_quantity": 50
        }
      }
    ]
  }
}
```

4. **查找评分高于4.5的所有产品。**

```json
GET /product_catalog/_search{
  "query": {
    "range": {
      "rating": {
        "gt": 4.5
      }
    }
  },
  "_source": ["name","rating"]
}
```

查询结果

![image-20240926131248248](./assets/image-20240926131248248.png)

```json
{
  "took": 3,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 10,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "2",
        "_score": 1,
        "_source": {
          "name": "Bluetooth Speaker",
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "4",
        "_score": 1,
        "_source": {
          "name": "USB-C Charger",
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "5",
        "_score": 1,
        "_source": {
          "name": "Noise Cancelling Headphones",
          "rating": 4.8
        }
      },
      {
        "_index": "product_catalog",
        "_id": "6",
        "_score": 1,
        "_source": {
          "name": "4K Monitor",
          "rating": 4.9
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1,
        "_source": {
          "name": "Smart Thermostat",
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "12",
        "_score": 1,
        "_source": {
          "name": "Portable Hard Drive",
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "16",
        "_score": 1,
        "_source": {
          "name": "Robot Vacuum",
          "rating": 4.8
        }
      },
      {
        "_index": "product_catalog",
        "_id": "17",
        "_score": 1,
        "_source": {
          "name": "Digital Camera",
          "rating": 4.7
        }
      },
      {
        "_index": "product_catalog",
        "_id": "19",
        "_score": 1,
        "_source": {
          "name": "Air Fryer",
          "rating": 4.6
        }
      },
      {
        "_index": "product_catalog",
        "_id": "20",
        "_score": 1,
        "_source": {
          "name": "Electric Scooter",
          "rating": 4.7
        }
      }
    ]
  }
}
```

5. **查询标签中包含"smart"的所有产品。**

```json
GET /product_catalog/_search{
  "query": {
    "match_phrase": {
      "description": "smart"
    }
  }
}
```

查询结果

![image-20240926131301906](./assets/image-20240926131301906.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 4,
      "relation": "eq"
    },
    "max_score": 1.5624864,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "8",
        "_score": 1.5624864,
        "_source": {
          "product_id": "P008",
          "name": "Fitness Tracker",
          "description": "Smart fitness tracker with heart rate monitor.",
          "category": "Wearables",
          "price": 39.99,
          "stock_quantity": 180,
          "supplier": "FitLife",
          "release_date": "2023-08-15",
          "tags": [
            "fitness",
            "tracker",
            "wearables"
          ],
          "rating": 4.2
        }
      },
      {
        "_index": "product_catalog",
        "_id": "16",
        "_score": 1.5624864,
        "_source": {
          "product_id": "P016",
          "name": "Robot Vacuum",
          "description": "Smart robot vacuum cleaner with scheduling features.",
          "category": "Home Appliances",
          "price": 199.99,
          "stock_quantity": 60,
          "supplier": "CleanBot",
          "release_date": "2024-04-25",
          "tags": [
            "robot",
            "vacuum",
            "home appliances"
          ],
          "rating": 4.8
        }
      },
      {
        "_index": "product_catalog",
        "_id": "14",
        "_score": 1.4779006,
        "_source": {
          "product_id": "P014",
          "name": "Smart Light Bulb",
          "description": "Color changing smart light bulb with app control.",
          "category": "Lighting",
          "price": 14.99,
          "stock_quantity": 220,
          "supplier": "LightSmart",
          "release_date": "2024-02-14",
          "tags": [
            "smart",
            "light bulb",
            "lighting"
          ],
          "rating": 4.2
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1.4020027,
        "_source": {
          "product_id": "P010",
          "name": "Smart Thermostat",
          "description": "Wi-Fi enabled smart thermostat with energy saving features.",
          "category": "Home Automation",
          "price": 129.99,
          "stock_quantity": 90,
          "supplier": "HomeSmart",
          "release_date": "2023-10-10",
          "tags": [
            "smart",
            "thermostat",
            "home automation"
          ],
          "rating": 4.7
        }
      }
    ]
  }
}
```

6. **查找供应商为"TechCorp"的产品。**

```json
GET /product_catalog/_search{
  "query": {
    "match": {
      "supplier": "TechCorp"
    }
  }
}
```

查询结果

![image-20240926131324957](./assets/image-20240926131324957.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 2.6390574,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "1",
        "_score": 2.6390574,
        "_source": {
          "product_id": "P001",
          "name": "Wireless Mouse",
          "description": "A high precision wireless mouse with ergonomic design.",
          "category": "Electronics",
          "price": 29.99,
          "stock_quantity": 150,
          "supplier": "TechCorp",
          "release_date": "2023-01-15",
          "tags": [
            "wireless",
            "mouse",
            "electronics"
          ],
          "rating": 4.5
        }
      }
    ]
  }
}
```

7. **查询发布日期在2023年6月1日之后的所有产品。**

```json
GET /product_catalog/_search{
  "query": {
    "range": {
      "release_date": {
        "gt": "2023-06-01"
      }
    }
  },
  "_source": [
    "name",
    "release_date"
  ]
}
```

查询结果

![image-20240926131343049](./assets/image-20240926131343049.png)

```json
{
  "took": 4,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 15,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "6",
        "_score": 1,
        "_source": {
          "name": "4K Monitor",
          "release_date": "2023-06-22"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "7",
        "_score": 1,
        "_source": {
          "name": "Gaming Keyboard",
          "release_date": "2023-07-30"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "8",
        "_score": 1,
        "_source": {
          "name": "Fitness Tracker",
          "release_date": "2023-08-15"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "9",
        "_score": 1,
        "_source": {
          "name": "Electric Toothbrush",
          "release_date": "2023-09-01"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "10",
        "_score": 1,
        "_source": {
          "name": "Smart Thermostat",
          "release_date": "2023-10-10"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "11",
        "_score": 1,
        "_source": {
          "name": "LED Desk Lamp",
          "release_date": "2023-11-25"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "12",
        "_score": 1,
        "_source": {
          "name": "Portable Hard Drive",
          "release_date": "2023-12-05"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "13",
        "_score": 1,
        "_source": {
          "name": "Coffee Maker",
          "release_date": "2024-01-10"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "14",
        "_score": 1,
        "_source": {
          "name": "Smart Light Bulb",
          "release_date": "2024-02-14"
        }
      },
      {
        "_index": "product_catalog",
        "_id": "15",
        "_score": 1,
        "_source": {
          "name": "Electric Kettle",
          "release_date": "2024-03-20"
        }
      }
    ]
  }
}
```

8. **查找描述中包含"wireless"的产品。**

```json
GET /product_catalog/_search
{
  "query": {
    "match_phrase": {
      "description": "wireless"
    }
  }
}
```

查询结果

![image-20240926131403036](./assets/image-20240926131403036.png)

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 2.5319078,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "1",
        "_score": 2.5319078,
        "_source": {
          "product_id": "P001",
          "name": "Wireless Mouse",
          "description": "A high precision wireless mouse with ergonomic design.",
          "category": "Electronics",
          "price": 29.99,
          "stock_quantity": 150,
          "supplier": "TechCorp",
          "release_date": "2023-01-15",
          "tags": [
            "wireless",
            "mouse",
            "electronics"
          ],
          "rating": 4.5
        }
      }
    ]
  }
}
```

9. **查询价格在20美元到100美元之间的所有产品。**

```json
GET /product_catalog/_search{
  "query": {
    "range": {
      "price": {
          "gte": 20,
          "lte":100
      }
    }
  },"_source": ["name","price"]
}
```

查询结果

![image-20240926131422189](./assets/image-20240926131422189.png)

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 12,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "Wireless Mouse",
          "price": 29.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "2",
        "_score": 1,
        "_source": {
          "name": "Bluetooth Speaker",
          "price": 49.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "5",
        "_score": 1,
        "_source": {
          "name": "Noise Cancelling Headphones",
          "price": 89.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "7",
        "_score": 1,
        "_source": {
          "name": "Gaming Keyboard",
          "price": 59.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "8",
        "_score": 1,
        "_source": {
          "name": "Fitness Tracker",
          "price": 39.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "9",
        "_score": 1,
        "_source": {
          "name": "Electric Toothbrush",
          "price": 34.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "11",
        "_score": 1,
        "_source": {
          "name": "LED Desk Lamp",
          "price": 24.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "12",
        "_score": 1,
        "_source": {
          "name": "Portable Hard Drive",
          "price": 64.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "13",
        "_score": 1,
        "_source": {
          "name": "Coffee Maker",
          "price": 79.99
        }
      },
      {
        "_index": "product_catalog",
        "_id": "15",
        "_score": 1,
        "_source": {
          "name": "Electric Kettle",
          "price": 29.99
        }
      }
    ]
  }
}
```

10. **查找产品名称中包含"Light"的所有产品。**

```json
GET /product_catalog/_search
{
  "query": {
    "match_phrase": {
      "name": "Light"
    }
  }
}
```

查询结果

![image-20240926131437666](./assets/image-20240926131437666.png)

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 2.2973087,
    "hits": [
      {
        "_index": "product_catalog",
        "_id": "14",
        "_score": 2.2973087,
        "_source": {
          "product_id": "P014",
          "name": "Smart Light Bulb",
          "description": "Color changing smart light bulb with app control.",
          "category": "Lighting",
          "price": 14.99,
          "stock_quantity": 220,
          "supplier": "LightSmart",
          "release_date": "2024-02-14",
          "tags": [
            "smart",
            "light bulb",
            "lighting"
          ],
          "rating": 4.2
        }
      }
    ]
  }
}
```

### 订单记录数据查询


1. **查询所有状态为"completed"的订单的订单ID和总金额。**

```json
GET /order_records/_search
{
  "query": {
    "match": {
      "status": "completed"
    }
  },"_source": ["order_id","total_amount"]
}
```

查询结果

![image-20240926131559609](./assets/image-20240926131559609.png)

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 8,
      "relation": "eq"
    },
    "max_score": 0.90445626,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR001",
          "total_amount": 150.75
        }
      },
      {
        "_index": "order_records",
        "_id": "4",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR004",
          "total_amount": 75
        }
      },
      {
        "_index": "order_records",
        "_id": "6",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR006",
          "total_amount": 300.25
        }
      },
      {
        "_index": "order_records",
        "_id": "9",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR009",
          "total_amount": 60
        }
      },
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR011",
          "total_amount": 110
        }
      },
      {
        "_index": "order_records",
        "_id": "14",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR014",
          "total_amount": 90
        }
      },
      {
        "_index": "order_records",
        "_id": "16",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR016",
          "total_amount": 250
        }
      },
      {
        "_index": "order_records",
        "_id": "19",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR019",
          "total_amount": 180
        }
      }
    ]
  }
}
```


2. **查找总金额大于100美元的所有订单。**

```json
GET /order_records/_search
{
  "query": {
    "range": {
      "total_amount": {
        "gt": 100
      }
    }
  },"_source": ["order_id","total_amount"]
}
```

查询结果

![image-20240926141719076](./assets/image-20240926141719076.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 11,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 1,
        "_source": {
          "order_id": "OR001",
          "total_amount": 150.75
        }
      },
      {
        "_index": "order_records",
        "_id": "3",
        "_score": 1,
        "_source": {
          "order_id": "OR003",
          "total_amount": 120.5
        }
      },
      {
        "_index": "order_records",
        "_id": "5",
        "_score": 1,
        "_source": {
          "order_id": "OR005",
          "total_amount": 200
        }
      },
      {
        "_index": "order_records",
        "_id": "6",
        "_score": 1,
        "_source": {
          "order_id": "OR006",
          "total_amount": 300.25
        }
      },
      {
        "_index": "order_records",
        "_id": "10",
        "_score": 1,
        "_source": {
          "order_id": "OR010",
          "total_amount": 150
        }
      },
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 1,
        "_source": {
          "order_id": "OR011",
          "total_amount": 110
        }
      },
      {
        "_index": "order_records",
        "_id": "13",
        "_score": 1,
        "_source": {
          "order_id": "OR013",
          "total_amount": 200
        }
      },
      {
        "_index": "order_records",
        "_id": "15",
        "_score": 1,
        "_source": {
          "order_id": "OR015",
          "total_amount": 135
        }
      },
      {
        "_index": "order_records",
        "_id": "16",
        "_score": 1,
        "_source": {
          "order_id": "OR016",
          "total_amount": 250
        }
      },
      {
        "_index": "order_records",
        "_id": "19",
        "_score": 1,
        "_source": {
          "order_id": "OR019",
          "total_amount": 180
        }
      }
    ]
  }
}
```

3. **查询支付方式为"paypal"的订单。**

```json
GET /order_records/_search
{
  "query": {
    "match": {
      "payment_method": "paypal"
    }
  },"_source": ["order_id","payment_method"]
}
```

查询结果

![image-20240926141735655](./assets/image-20240926141735655.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 4,
      "relation": "eq"
    },
    "max_score": 1.540445,
    "hits": [
      {
        "_index": "order_records",
        "_id": "2",
        "_score": 1.540445,
        "_source": {
          "order_id": "OR002",
          "payment_method": "paypal"
        }
      },
      {
        "_index": "order_records",
        "_id": "7",
        "_score": 1.540445,
        "_source": {
          "order_id": "OR007",
          "payment_method": "paypal"
        }
      },
      {
        "_index": "order_records",
        "_id": "12",
        "_score": 1.540445,
        "_source": {
          "order_id": "OR012",
          "payment_method": "paypal"
        }
      },
      {
        "_index": "order_records",
        "_id": "17",
        "_score": 1.540445,
        "_source": {
          "order_id": "OR017",
          "payment_method": "paypal"
        }
      }
    ]
  }
}
```

4. **查找订单日期在2024年2月之后的所有订单。**

```json
GET /order_records/_search
{
  "query": {
    "range": {
      "order_date": {
          "gte": "2024-03-01"
      }
    }
  },"_source": ["order_id","order_date"]
}
```

查询结果

![image-20240926141747422](./assets/image-20240926141747422.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 10,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 1,
        "_source": {
          "order_id": "OR011",
          "order_date": "2024-03-01"
        }
      },
      {
        "_index": "order_records",
        "_id": "12",
        "_score": 1,
        "_source": {
          "order_id": "OR012",
          "order_date": "2024-03-05"
        }
      },
      {
        "_index": "order_records",
        "_id": "13",
        "_score": 1,
        "_source": {
          "order_id": "OR013",
          "order_date": "2024-03-10"
        }
      },
      {
        "_index": "order_records",
        "_id": "14",
        "_score": 1,
        "_source": {
          "order_id": "OR014",
          "order_date": "2024-03-15"
        }
      },
      {
        "_index": "order_records",
        "_id": "15",
        "_score": 1,
        "_source": {
          "order_id": "OR015",
          "order_date": "2024-03-20"
        }
      },
      {
        "_index": "order_records",
        "_id": "16",
        "_score": 1,
        "_source": {
          "order_id": "OR016",
          "order_date": "2024-03-25"
        }
      },
      {
        "_index": "order_records",
        "_id": "17",
        "_score": 1,
        "_source": {
          "order_id": "OR017",
          "order_date": "2024-03-30"
        }
      },
      {
        "_index": "order_records",
        "_id": "18",
        "_score": 1,
        "_source": {
          "order_id": "OR018",
          "order_date": "2024-04-04"
        }
      },
      {
        "_index": "order_records",
        "_id": "19",
        "_score": 1,
        "_source": {
          "order_id": "OR019",
          "order_date": "2024-04-09"
        }
      },
      {
        "_index": "order_records",
        "_id": "20",
        "_score": 1,
        "_source": {
          "order_id": "OR020",
          "order_date": "2024-04-14"
        }
      }
    ]
  }
}
```

5. **查询包含产品ID为"P001"的订单。**

```json
GET /order_records/_search
{
  "query": {
    "nested": {
      "path": "items",
      "query": {
        "match": {
          "items.product_id": "P001"
        }
      }
    }
  },
  "_source": [
    "order_id",
    "items.product_id"
  ]
}
```

查询结果

![image-20240926141801022](./assets/image-20240926141801022.png)

```json
{
  "took": 15,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 2.6855774,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 2.6855774,
        "_source": {
          "order_id": "OR001",
          "items": [
            {
              "product_id": "P001"
            },
            {
              "product_id": "P002"
            }
          ]
        }
      }
    ]
  }
}
```

6. **查找所有状态为"cancelled"的订单的客户ID。**

```json
GET /order_records/_search
{
  "query": {
    "match": {
      "status": "cancelled"
    }
  },
  "_source": ["customer_id","status"]
}
```

查询结果

![image-20240926141814262](./assets/image-20240926141814262.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 4,
      "relation": "eq"
    },
    "max_score": 1.540445,
    "hits": [
      {
        "_index": "order_records",
        "_id": "5",
        "_score": 1.540445,
        "_source": {
          "customer_id": "C005",
          "status": "cancelled"
        }
      },
      {
        "_index": "order_records",
        "_id": "10",
        "_score": 1.540445,
        "_source": {
          "customer_id": "C010",
          "status": "cancelled"
        }
      },
      {
        "_index": "order_records",
        "_id": "15",
        "_score": 1.540445,
        "_source": {
          "customer_id": "C015",
          "status": "cancelled"
        }
      },
      {
        "_index": "order_records",
        "_id": "20",
        "_score": 1.540445,
        "_source": {
          "customer_id": "C020",
          "status": "cancelled"
        }
      }
    ]
  }
}
```

7. **查询发货日期在2024年1月15日之前的订单。**

```json

GET /order_records/_search
{
  "query": {
    "range": {
      "delivery_date": {
          "lt": "2024-01-15"
      }
    }
  },"_source": ["order_id","delivery_date"]
}
```

查询结果

![image-20240926141825881](./assets/image-20240926141825881.png)

```json
{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 0,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  }
}
```

8. **查找使用"credit\_card"支付的订单。**

```json
GET /order_records/_search
{
  "query": {
    "match": {
      "payment_method": "credit_card"
    }
  },"_source": ["order_id","payment_method"]
}
```

查询结果

![image-20240926141838186](./assets/image-20240926141838186.png)

```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 8,
      "relation": "eq"
    },
    "max_score": 0.90445626,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR001",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "4",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR004",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "6",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR006",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "9",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR009",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR011",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "14",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR014",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "16",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR016",
          "payment_method": "credit_card"
        }
      },
      {
        "_index": "order_records",
        "_id": "19",
        "_score": 0.90445626,
        "_source": {
          "order_id": "OR019",
          "payment_method": "credit_card"
        }
      }
    ]
  }
}
```

9. **查询总金额在50美元到200美元之间的所有订单。**

```json
GET /order_records/_search
{
  "query": {
    "range": {
      "total_amount": {
        "gte":50,
        "lte":200
      }
    }
  },"_source": ["order_id","total_amount"]
}
```

查询结果

![image-20240926141849534](./assets/image-20240926141849534.png)

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 16,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "order_records",
        "_id": "1",
        "_score": 1,
        "_source": {
          "order_id": "OR001",
          "total_amount": 150.75
        }
      },
      {
        "_index": "order_records",
        "_id": "2",
        "_score": 1,
        "_source": {
          "order_id": "OR002",
          "total_amount": 89.99
        }
      },
      {
        "_index": "order_records",
        "_id": "3",
        "_score": 1,
        "_source": {
          "order_id": "OR003",
          "total_amount": 120.5
        }
      },
      {
        "_index": "order_records",
        "_id": "4",
        "_score": 1,
        "_source": {
          "order_id": "OR004",
          "total_amount": 75
        }
      },
      {
        "_index": "order_records",
        "_id": "5",
        "_score": 1,
        "_source": {
          "order_id": "OR005",
          "total_amount": 200
        }
      },
      {
        "_index": "order_records",
        "_id": "8",
        "_score": 1,
        "_source": {
          "order_id": "OR008",
          "total_amount": 89.5
        }
      },
      {
        "_index": "order_records",
        "_id": "9",
        "_score": 1,
        "_source": {
          "order_id": "OR009",
          "total_amount": 60
        }
      },
      {
        "_index": "order_records",
        "_id": "10",
        "_score": 1,
        "_source": {
          "order_id": "OR010",
          "total_amount": 150
        }
      },
      {
        "_index": "order_records",
        "_id": "11",
        "_score": 1,
        "_source": {
          "order_id": "OR011",
          "total_amount": 110
        }
      },
      {
        "_index": "order_records",
        "_id": "12",
        "_score": 1,
        "_source": {
          "order_id": "OR012",
          "total_amount": 75.99
        }
      }
    ]
  }
}
```

10. **查找订单ID中包含"OR01"的所有订单。**

```json
GET /order_records/_search
{
  "query": {
    "match": {
      "order_id": "OR01"
    }
  },"_source": ["order_id"]
}
```

查询结果

![image-20240926141904372](./assets/image-20240926141904372.png)

```json
{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 0,
      "relation": "eq"
    },
    "max_score": null,
    "hits": []
  }
}
```
