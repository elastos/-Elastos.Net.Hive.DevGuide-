# Scripting 使用示例

Hive Node 的 Scripting 机制 可以用于分享特定权限的数据，本文将通过一些示例来展示如何分享文件内容和数据库数据。

## 数据库数据

假设有如下需求：

- 用户A 会发表文章至自己 Vault 的数据库内，文章有三部分信息：标题、内容、访问次数
- 用户A 使用 scripting 机制 分享特定的内容：
    * 文章的标题，所有人都可以访问
    * 文章的内容，只有订阅文章的人才可以访问
    * 文章的访问次数，只有用户A自己可以访问
    
### 表定义

针对此需求，简化的表结构设计如下：

```
post: # 文章
    - _id
    - title # 文章标题，所有人能看到
    - content # 文章内容，关注的人才能看到
    - visits # 访问次数，私有数据

subscribe: # 订阅者列表
    - _id
    - post_id # 文章id
    - user_did # 订阅者的did
```

对于数据的访问权限分为三类：
    - 私有权限：只有数据的使用者能访问
    - 指定权限：只有满足条件的用户能访问
    - 公开权限：所有人都可以访问

### 私有权限

对于只有私有权限的字段，由于 用户A 是数据的拥有者，那么可以通过 Database Service 来直接访问 `post` 表内的任意数据。
此时不需要用到 scripting 机制。

### 指定权限

此场景下的 指定权限，对于 scripting 机制 而言，则是在注册脚本的时候，限定条件 （condition）为订阅文章的人，才可以访问文章的标题和内容。

- 注意：标题是所有人能访问，那么订阅文章的人也能访问。

以下为注册名为 specified_permission 的脚本，脚本的名称可以任意指定，只要不和其它名称冲突就行。

- condition 为限制条件的设置，类型为 queryHasResult 的 condition 会去用 post_id 和 user_did 去表 subscribe 查询。
若能查询到结果，表示有权限。若没有，则不能执行该脚本，也就不能获取到任何结果。
$caller_did 表示调用脚本的 user did，此参数不需要用户在执行的时候指定，执行脚本的时候会自动获取到。
- executable 表示需要执行的任务定义，类型为 find 的 executable 表示需要通过 _id 查询文章的内容。
projection 为 mongodb find 接口的选项，指定输出结果中不展示 _id 和 visits，即只展示字段 title 和 content 。

```
URL: PUT /api/v2/vault/scripting/specified_permission
PAYLOAD: {
  "condition": {
    "type": "queryHasResult",
    "name": "verify_permission",
    "body": {
      "collection": "subscribe",
      "filter": {
        "post_id": "$params.post_id",
        "user_did": "$caller_did"
      }
    }
  },
  "executable": {
    "type": "find",
    "name": "find_post",
    "output": true,
    "body": {
      "collection": "post",
      "filter": {
        "_id": "$params.post_id"
      },
      "options": {"projection": {
          "_id": false,
          "visits": false,
        }, 
      }
    }
  },
  "allowAnonymousUser": false,
  "allowAnonymousApp": false
}
```

执行脚本的请求如下：

- 需要指定脚本名称为 specified_permission，此名称必须和注册的名称一样。
- context 用于指定用户A（数据拥有者）的 user did 和 app did，这样方便 Scripting Service 查询到指定用户的脚本。
- params 为指定脚本定义中定义的参数的值，此处指定义了 post_id，故仅需指定该值即可。

```
URL: PATCH /api/v2/vault/scripting/specified_permission
{
    "context": {
        "target_did": "did:elastos:icXtpDnZRSDrjmD5NQt6TYSphFRqoo2q6n",
        "target_app_did":"did:elastos:ikkFHgoUHrVDTU8HTYDAWH9Z8S377Qvt7n"
    },
    "params": {
        "post_id": {"$oid": "5f8d9dfe2f4c8b7a6f8ec0f1"}
    }
}
```

### 公开权限

相对于 指定权限，公开权限不需要设置脚本的条件（condition），注册脚本的请求如下。

- 需要指定 公开权限 的脚本名称，此处设置为 public_permission 。
- executable 和 指定权限 唯一的差别是，返回结果中，没有了内容字段。

```
URL: PUT /api/v2/vault/scripting/public_permission
PAYLOAD: {
  "executable": {
    "type": "find",
    "name": "find_post",
    "output": true,
    "body": {
      "collection": "post",
      "filter": {
        "_id": "$params.post_id"
      },
      "options": {"projection": {
          "_id": false,
          "visits": false,
          "content": false,
        }, 
      }
    }
  },
  "allowAnonymousUser": false,
  "allowAnonymousApp": false
}
```

执行脚本的请求和 指定权限 一样。注意：此处的脚本名称为 公开权限 的脚本名称。

```
URL: PATCH /api/v2/vault/scripting/public_permission
{
    "context": {
        "target_did": "did:elastos:icXtpDnZRSDrjmD5NQt6TYSphFRqoo2q6n",
        "target_app_did":"did:elastos:ikkFHgoUHrVDTU8HTYDAWH9Z8S377Qvt7n"
    },
    "params": {
        "post_id": {"$oid": "5f8d9dfe2f4c8b7a6f8ec0f1"}
    }
}
```

## 文件内容

scripting 机制 同样支持文件的分享。在如上的场景，假设文章内有文件的查看是和用户是否订阅有关的，那么我们同样分三种类型来查看文件。

### 私有权限

对于私有权限而言，获取文件内容，可以通过 Files Service 来获取。

### 指定权限

若是需要指定订阅文章的人才能看见文章里面的文件内容，那么注册脚本请求如下：

- 脚本的名称不能重复，此处指定为 file_specified_permission
- 条件（condition）的设置和获取文章内容一样，指定类型为 queryHasResult，实际上是查询订阅表（`subscribe`）确保用户已订阅文章。
- executable 的类型为 fileDownload 表示获取文件内容，此处设置了文件的参数 path，便于执行脚本的时候指定文件的路径。

```
URL: PUT /api/v2/vault/scripting/file_specified_permission
PAYLOAD: {
  "condition": {
    "type": "queryHasResult",
    "name": "verify_permission",
    "body": {
      "collection": "subscribe",
      "filter": {
        "post_id": "$params.post_id",
        "user_did": "$caller_did"
      }
    }
  },
  "executable": {
    "type": "fileDownload",
    "name": "get_file",
    "output": true,
    "body": {
      "path":"$params.path"
    }
  },
  "allowAnonymousUser": false,
  "allowAnonymousApp": false
}
```

执行获取文件内容脚本的消息如下。

- 相对于文章内容的获取脚本，此处增加了文件路径参数的值，按具体的文件路径设置即可

```
URL: PATCH /api/v2/vault/scripting/file_specified_permission
{
    "context": {
        "target_did": "did:elastos:icXtpDnZRSDrjmD5NQt6TYSphFRqoo2q6n",
        "target_app_did":"did:elastos:ikkFHgoUHrVDTU8HTYDAWH9Z8S377Qvt7n"
    },
    "params": {
        "post_id": {"$oid": "5f8d9dfe2f4c8b7a6f8ec0f1"},
        "path": "/post/file_name"
    }
}
```

### 公开权限

相对于文件的 指定权限 下载，公开权限不需要指定条件，仅需要指定文件的路径。脚本的注册消息：

```
URL: PUT /api/v2/vault/scripting/file_public_permission
PAYLOAD: {
  "executable": {
    "type": "fileDownload",
    "name": "get_file",
    "output": true,
    "body": {
      "path":"$params.path"
    }
  },
  "allowAnonymousUser": false,
  "allowAnonymousApp": false
}
```

脚本的执行消息：

```
URL: PATCH /api/v2/vault/scripting/file_public_permission
{
    "context": {
        "target_did": "did:elastos:icXtpDnZRSDrjmD5NQt6TYSphFRqoo2q6n",
        "target_app_did":"did:elastos:ikkFHgoUHrVDTU8HTYDAWH9Z8S377Qvt7n"
    },
    "params": {
        "path": "/post/file_name"
    }
}
```
