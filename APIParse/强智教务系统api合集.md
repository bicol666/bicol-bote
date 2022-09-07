# 校验，获取Token

URL： http://edu.cqcvc.com.cn:800/app/app.ashx?method=authUser&xh=2040403192&pwd=2040403192

请求方法：GET

参数列表：

| 参数 | 必填 | 说明 |
| ---- | ---- | ---- |
| xh   | 是   | 学号 |
| pwd  | 是   | 密码 |

响应示例：

```json
{
    "flag":"1",	//是否成功：1-成功、0-失败；
    "token":"BABBFA7442D4F967E7017E2578E413BC",	//token验证
    "msg":"验证成功",	//处理结果描述信息；
    "userrealname":"xxx",	//用户真实姓名，验证失败值为null；
    "userdwmc":"信息与智能工程",	//学生所在院系名称，是失败值为null；
    "usertype":"2"	//用户类别，已知学生身份为2；
}
```



# -------------------------------------------在这之后的请求，请求头需添加token，默认值：BABBFA7442D4F967E7017E2578E413BC

# --------------------------------------------



# 获取学生ID信息

URL： http://edu.cqcvc.com.cn:800/app/app.ashx?method=getStudentIdInfo&xh=2040403192

请求方法：GET

参数列表：

| 参数 | 必填 | 说明 |
| ---- | ---- | ---- |
| xh   | 是   | 学号 |

响应示例：

```json
{
    "bjid":"00409",//未知，推测：班级ID；
    "yxid":"00003",//未知，推测：院系ID；
    "ndzyid":"164",//未知，推测：专业ID；
    "xxdm":"13734"//未知，推测：学校代码;
}
```





# 获取学生信息

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getUserInfo&xh=2040403192

请求方法：GET

参数列表：

| 参数 | 必填 | 说明 |
| ---- | ---- | ---- |
| xh   | 是   | 学号 |

响应示例：

```json
{
    "xh":"2040403192",	//学生学号
    "bj":"20计应0035",	//班级
    "xm":"李缘阳",	//姓名
    "yxmc":"信息与智能制造学院",	//院系名称
    "ksh":"20500124150721",	//未知
    "xz":"3.0",	//未知
    "nj":"2020",	//年级
    "zymc":"计算机应用技术",	//专业
    "usertype":"2"	//用户类别
}
```



# 获取本周日期信息：周次、周开始/结束日期……

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getCurrentTime&currDate=2021-12-03

请求方法：GET

参数列表：

| 参数     | 必填 | 说明                       |
| -------- | ---- | -------------------------- |
| currDate | 是   | 当前日期，格式：yyyy-MM-dd |

响应示例：

```json
{
    "zc":"13",//周次
    "e_time":"2021-12-04",//当前周结束日期
    "s_time":"2021-11-28",//当前周开始日期
    "xnxqh":"2021-2022-1"//学年信息
}
```


# 获取本周课程信息

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getKbcxAzc&xh=2040403192&xnxqid=2021-2022-1&zc=13

请求方法：GET

参数列表：

| 参数   | 必填 | 说明                                           |
| ------ | ---- | ---------------------------------------------- |
| xh     | 是   | 学号                                           |
| xnxqid | 否   | 学年学期，不填返回当前日期所在学年周课表信息； |
| zc     | 是   | 周次                                           |

响应示例：

```json
[
    {
        "jsxm":"何瑞英",//教师姓名
        "jsmc":"明华楼605(公共机房)",//教室名称
        "jssj":"",//结束时间
        "kssj":"",//开始时间
        "kkzc":"2-14,17-18",//开课周次
        "kcsj":"10506",//课程时间，格式为X0a0b，X代表星期几，ab代表上第a、b节课；
        "kcmc":"前端JS设计",//课程名称
        "sjbz":"0"//未知
    },
	……
]
```



# 获取成绩信息

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getCjcx&xh=2040403193&xnxqid=2020-2021-1

请求方法：GET

参数列表：

| 参数   | 必填 | 说明                             |
| ------ | ---- | -------------------------------- |
| xh     | 是   | 学号                             |
| xnxqid | 否   | 学年学期，不填输出所有成绩信息； |

响应示例：

```json
[
    {
        "zcj":"88",//总成绩
        "xm":"xx",//未知
        "xqmc":"2020-2021-1",//学年学期信息
        "kcmc":"体育Ⅰ",//课程名称
        "xf":"2.00",//学分
        "kclbmc":"公共课",//课程类别
        "kcxzmc":"必修",//课程性质
        "kcywmc":"",//未知，通常为空
        "ksxzmc":"正常考试",//考试性质
        "cjbsmc":"",//未知，通常为空
        "bz":""//未知，通常为空
    },
    ……
]
```



# 获取学校建筑物信息

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getJxlcx&xqid=00001

请求方法：GET

参数列表：

| 参数 | 必填 | 说明                     |
| ---- | ---- | ------------------------ |
| xqid | 是   | 校区ID,本校区ID为：00001 |

响应示例：

```json
[
    {
        "jzwid":"00001",//建筑物ID
        "jzwmc":"文华楼"//建筑物名称
    },
   ……
]
```



# 获取空教室信息(废弃)

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getKxJscx&time=324&idleTime=allday&xqid=1&jxlid=2443&classroomNumber=30

请求方法：GET

参数列表：

| 参数            | 必填 | 说明                                                         |
| --------------- | ---- | ------------------------------------------------------------ |
| time            | 否   | 当前日期，格式"YYYY-MM-DD"，不填返回当前日期空闲教室         |
| idleTime        | 否   | 有allday,am,pm,night四种取值                                 |
| xqid            | 否   | 校区ID                                                       |
| jxlid           | 否   | 教学楼ID                                                     |
| classroomNumber | 是   | 可选项 30,30-40,40-50,60(分别意为30人以下，30-40人,···,60人以上) |

响应示例：

```json
{
暂无
}
```


# 获取校区信息

URL：http://edu.cqcvc.com.cn:800/app/app.ashx?method=getXqcx

请求方法：GET

参数列表：无

响应示例：

```json
[
    {
        "xqid":"00001",//校区ID
        "xqmc":"本校区"//校区名称
    }
]
```
