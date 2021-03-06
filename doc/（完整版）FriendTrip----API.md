#                接口文档最终版

 简述：首先实现核心的功能，然后再实现其他附加功能，按照优先级顺序开发。为了开发进度以及开发时的测试，需要按照优先级进行开发。

 优先级解释：

 -----开发的时候按照下面的优先级来做，大的优先级按照一、二、三、四、五、六、七的顺序。

 -----每个优先级别里面，按照接口编号的罗列顺序来开发。比如：优先级一里面，首先得把1号接口做完，才能进行注册；优先级四里面，首先得把接口5（发布）写完，那么后续的接口写了才能有意义。
 

##优先级：##

 **一、注册、登录**
  
  + 接口编号:1、2、3、4、23、9 
 
 **二、首页推荐**
 
   + 接口编号：10、17、
    
    
 **三、景区（景点）详情：**
     
   + 接口编号：12、13、14、15

 **四、出行（拼车）**-----只有有了推荐模块，才能有出行发布和获取
    
   + 接口编号：5、6、7、16

 ** 五、附加用户行为**----评论、想去、去过等等
    
   + 接口编号：19、9、21、22、20、
   
 ** 六、搜索**
   + 接口编号：11、18
   
 ** 七、消息推送**
   + 接口编号：8、24

 
##                  接口详情文档

**接口目录**

                                              发起源 
    + 1、请求验证码-------------------------------系统
    + 2、注册------------------------------------系统
    + 3、登录------------------------------------系统
    + 4、填写（修改）用户信息-----------------------用户
    + 5、发布出行（拼车）-------------------------（景区、景点、个人中心、出行）
    + 6、加入出行（拼车）-------------------------（景区、景点、出行）
    + 7、获取发布列表----------------------------（景区、景点、个人中心）
    + 8、--我的私信-----------------------------------用户
    + 9、获取评论---------------------------------（景区、景点、用户）
    + 10、获取首页推荐信息--------------------------系统
    + 11、获取搜索省份、城市推荐表（by name)-----------用户搜索省份、城市
    + 12、获取景区详情(by ID)-----------------------景区
    + 13、获取景区详情(by name)-----------------用户搜索景区
    + 14、获取景点详情（by name）----------------用户搜索景点
    + 15、获取景点详情（by-ID）---------------------景区
    + 16、获取出行记录-----------------------------出行
    + 17、获取搜索推荐（所有景区、热门景点）-----------系统
    + 18、搜索出行记录------------------------------用户
    + 19、发表评论---------------------------------景区、景点
    + 20、去过-------------------------------------景区、景点
    + 21、想去-------------------------------------景区、景点
    + 22、获取想去人GPS信息列表----------------------景区、景点
    + 23、获取用户信息------------------------------用户
    + 24、消息推送（私信、出行）----------------------系统
    + 25、头像上传----------------------------------用户

**规则**

  
  + 接口名称：    全部小写，单词之间下划线链接
  + 传输方式：    POST
  + 传输格式：    JSON（小驼峰命名方式）
  + URL：        本地----localhost:3001/interface/.....
  + 自定义状态码： 便于确认返回信息的状态。
  
**自定义状态码**
  
    定义：
       使用四位状态码：1000
       前两位表示API的编号（即目录上的编号），第三位表示请求类型，第四位表示返回状态。如：1010表示：10号接口发来的类型为1的请求；返回0表示请求成功；否则，1011表示服务器返回类型为1的失败状态，该状态表示：服务器正在维护
       状态码是用来标识请求的类型，以及服务器返回给客户端的结果状态。
    规则：
       如果请求成功，任何接口的请求返回的状态码都为 0；如果失败则按照定义，返回的状态码中前两位表示API编号，后面表示具体的返回状态。
    举例：
       如：3号接口发出首次登录请求。
      
        客户端（POST）：
        URL：localhost：3001/sign_in
        {
          "type":0301，              // number 
          "phone":"15709345512",     // string
          "pwd":"xxfwe234",          // string
          "token":"",                // string--首次登录token为空
        }
        服务器端：
        Success:
         {
           "status":"0" ,             // string --成功
           "token":"23535323"，       // string --返回token值
           "userinfo":                // string --个人信息
              {
                "name":"张三",
                "phone":"15700873459"，
                "school":"四川大学",
                "compus":"江安校区",
                "grade":"大二",
                "sex":"性别",
                "imageUrl":"头像地址",
              }
            "joinNum": 3 ,              // number -加入数
            "releaseNum": 6 ,           // number -发布数
            "commentNum": 10 ,          // number -评论数
            "letterNum": 3,             // number -私信数
         }
        Failure:
         {
           "status": "0311" ,         // string --表示登录失败
           "message":"登录失败"，
         }
**自定义状态码一览**
   
   自定状态码：
     状态码之后开发的时候再写吧，因为现在还不知道后台会遇到什么错误。所以暂时先
     返回成功和失败吧。

**接口详情**

1、请求验证码

功能简介：

    用户点击请求验证码，客服端向服务器发送请求，服务器收到请求后
    1、生成验证码
    2、返回给客户端同时短信下发到用户手机。
    返回的验证码作为客户端输入时的验证。

客户端（POST）：

    URL:localhost:3001/request_code
    {
      "phone":"123553",                // string
      "type":"0110",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "code":"4451",                   // string
    }
    Failure:
    {
      "status": 0111 ,                 // string
      "message":"手机号码错误"          //  string 错误信息
    }


2、注册

功能简介：

    用户点击注册，客户端发送请求。服务器接收到请求之后；
    1、匹配验证码是否正确
    2、手机号码是否已注册
    3、返回结果

客户端（POST）：

    URL:localhost:3001/sign_in
    {
      "phone":"123553",                // string
      "pwd":"32452",                   // string
      "code":"55422",                  // string
      "type":"0210",                   // string
      "info":
         {                             // string
           "name":"张三"，             
           "province":"四川",
           "city":"成都",
           "school":"四川大学",
           "compus":"江安校区",
           "grade":"大二",
           "address":"学校地址：四川省成都市双流县",
           "sex":"男",
         }
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "uid":"34451",                   // string
    }
    Failure:
    {
      "status": 0211 ,                 // string
      "message":"验证码错误"            //  string 错误信息
    }


3、登录

功能简介：

    用户登录，服务器收到请求，判断账号密码是否正确。返回相应结果

客户端（POST）：

    URL：localhost：3001/log_in
    {
      "type":0301，                  // number 
      "phone":"15709345512",         // string
      "pwd":"xxfwe234",              // string
      "token":"",                    // string--首次登录token为空
    }
服务器端：

    Success:
    {
      "status":"0" ,             // string --成功
      "token":"23535323"，       // string --返回token值
      "info":                // string --个人信息
         {
           "name":"张三",
           "phone":"15700873459"，
           "school":"四川大学",
           "compus":"江安校区",
           "grade":"大二",
           "sex":"性别",
           "imageUrl":"头像地址",
         }
       "joinNum": 3 ,              // number -加入数
       "releaseNum": 6 ,           // number -发布数        "commentNum": 10 ,          // number -评论数
       "letterNum": 3,             // number -私信数
    }
    Failure:
    {
      "status": "0311" ,           // string --表示登录失败
      "message":"用户名不存在"，     // string --信息
    }

4、填写、修改用户信息

功能简介：

     用户修改（完善）个人信息，服务器收到请求。查找相应用户，检测是否重名，返回结果。

客户端（POST）：

    URL:localhost:3001/fix_info
    {
      "uid":"123553",                  // string
      "type":"0410",                   // string
      "token":"34252",                 // string
      "info":                          // string --个人信息
         {
           "name":"张三",
           "phone":"15700873459"，
           "school":"四川大学",
           "compus":"江安校区",
           "grade":"大二",
           "sex":"性别",
           "imageUrl":"头像地址",
         }
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "uid":"32451",                   // string
    }
    Failure:
    {
      "status": 0411 ,                 // string
      "message":"用户名已存在"          //  string 错误信息
    }


5、发布出行（拼车）

功能简介：

    用户从景区、景点、个人中心、出行那里发布出行信息。
    1、用户点击发布出行（拼车），服务器收到请求后,生成发布时间
    2、服务器收到请求后，要创建一个推送的服务，在截止时间前x小时（默认为3小时）给用户发送短信（以及APP消息的通知）。

客户端（POST）：

    URL:localhost:3001/release_trip
    {
      "uid":"4353";                // string
      "token": "token",            // string
      "type":"0510"                // string  [1,2,3] 0510、景区 ，0520、景点 ，0530、拼车 
      "spotId":"32551",            // string 景点、景区ID；从出行、个人中心发布的出行没有该ID
      "origin":"1";                // number 1：景点、景区 2：直接发布（个人中心，出行）
     "info":                       // string
       {
         "uid":"发布者ID",
         "oriCity":"始发城市；如：成都",
         "oriSchool":"始发学校；如：四川大学"，
         "destination":"目的地名；如：峨眉山、锦里。。",
         "goDate":"2001-01-14-20",    //年月日时
         "phone":"手机联系方式",
         "content":"出行说明"
         "wantNumber":  "出行（拼车）人数"，
       }
    }    

服务器：

    Success:
    {
      "status": 0 ,                      // string
      "tripID":"4451",                   // string
    }
    Failure:
    {
      "status": 0511 ,                   // string
      "message":"发布失败"                // string 错误信息
    }


6、加入出行（拼车）

功能简介：

    用户点击加入出行（拼车），生成请求。服务器收到请求后，判定出行人数是否已满。然后返回结果

客户端（POST）：

    URL:localhost:3001/join_trip
    {
      "uid":"23412",                     // string
      "type":"0610",                     // string 6010:出行 ，6020：拼车
      "token":"52342",                   // string 
      "tripId":"55321",                  // string
      "releaserId""53213",               // string 发布者ID 
    }    

服务器：

    Success:
    {
      "status": 0 ,                      // string
      "tripId":"4451",                   // string
    }
    Failure:
    {
      "status": 0611 ,                   // string
      "message":"人数已满"                //  string 错误信息
    }


7、获取发布记录------景区、景点、个人中心

功能简介：

    来源：景区、景点、个人中心（我的发布）；
    客户端发送请求，服务器根据来源的类型，判断应该如何筛选。
    从景区、景点、个人中心来的都根据Id作为筛选的依据，根据类型来判断应该到哪张数据表查询。
    将筛选结果返回给客户端。

客户端（POST）：

    URL:localhost:3001/get/release_list
    {
      "type":"0710",                     // string  0710:景区；0720：景点；0730：个人中心
      "id"："134123",                    // string
      "token":"10104",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                      // string
      "tripList":                     // string
        [
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，      // string
            "oriSchool":"始发学校",         // string
            "oriCity":"始发城市",           // string
            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "content":"附加说明",            // string
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，      // string
            "oriSchool":"始发学校",         // string
            "oriCity":"始发城市",           // string
            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "content":"附加说明",
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          ......
        ]
    }
    Failure:
    {
      "status": 0711 ,                 // string
      "message":"该景点没有出行记录"          //  string 错误信息
    }



8、我的私信————待定

功能简介：

    用户点击请求验证码，客服端向服务器发送请求，服务器收到请求后，生成验证码，然后短信下发到用户手机。

客户端（POST）：

    URL:localhost:3001/request_code
    {
      "phone":"123553",                // string
      "type":"1010",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "code":"4451",                   // string
    }
    Failure:
    {
      "status": 1011 ,                 // string
      "message":"手机号码错误"          //  string 错误信息
    }


9、获取评论

功能简介：

    1、来源：景区、景点、个人中心
    2、以上来源发布获取评论请求，服务器收到请求，根据类别来进行筛选
      然后返回。
    3、若来源为景区、景点，则选取spotId作为筛选依据，若来源为个人中心，则选取uid作为筛选依据
    4、返回评论最多50条

客户端（POST）：

    URL:localhost:3001/get/comment_list
    {
      "type":0910,                     // string  9010:景区、9020：景点，9030：个人中心
      "token":"123553",                // string
      "uid":"1010",                    // string
      "spotId"："13412",               // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "commentId":"4451",              // string
      "comList":
         [
           {
             "uid":"123412",            // string
             "name":"张三",              // string
             "imageUrl":"头像Url"，      // string
             "date":"2013-02-12-17",    // string
             "content":"不错哦",         // string
           },
           {
             "uid":"123412",            // string
             "name":"张三",              // string
             "date":"2013-02-12-17",    // string
             "content":"不错哦",         // string
           },
           .......
         ]
    }
    Failure:
    {
      "status": 0911 ,                 // string
      "message":"没有评论"          //  string 错误信息
    }

10、获取首页推荐信息

功能简介：
     
    进入该软件时，生成首页的推荐信息：
    1、首页轮播图
    2、推荐景区

客户端（POST）：

    URL:localhost:3001/get/home_recommend
    {
      "token":"",                      // string
      "type":"1010",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "bannerList":                    // string
         [
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
             .....
         ],
       "spotList":                      // string
        [
          {
            "spotId":"23421",           // string
            "spotName":"乐山市-峨眉山",   // string
            "imageUrl":"封面图",         // string
            "focusNum":"关注数",         // string
            "childSpotNum":"子景点数",   // string
            "describe":"描述"，          // string
          },
          {
            "spotId":"23421",           // string
            "spotName":"乐山市-峨眉山",   // string
            "imageUrl":"封面图",         // string
            "focusNum":"关注数",         // string
            "childSpotNum":"子景点数",   // string
            "describe":"描述"，          // string
          },
          .....
        ]
    }
    Failure:
    {
      "status": 1011 ,                 // string
      "message":"服务器正在维护"          //  string 错误信息
    }

11、获取省份、城市推荐列表

功能简介：

    用户搜索省份、城市时，服务器返回该省份、城市的景区推荐列表
    返回数目：小于十个；按照热度递减

客户端（POST）：

    URL:localhost:3001/get/spot_promote
    {
      "type":"1110",                    // string 1010:省份，1020：城市
      "token":"4213",                   // string
      "name"："四川（或者成都）"，         // string

    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "spotList":                   // string
        [
          {
            "spotId":"23421",           // string
            "spotName":"乐山市-峨眉山",   // string
            "imageUrl":"封面图",         // string
            "focusNum":"关注数",         // string
            "childSpotNum":"子景点数",   // string
            "describe":"描述"，          // string
          },
          {
            "spotId":"23421",           // string
            "spotName":"乐山市-峨眉山",   // string
            "imageUrl":"封面图",         // string
            "focusNum":"关注数",         // string
            "childSpotNum":"子景点数",   // string
            "describe":"描述"，          // string
          },
          .....
        ]
    }
    Failure:
    {
      "status": 1111 ,                 // string
      "message":"搜索结果不存在"          //  string 错误信息
    }


12、获取景区详情（by ID）

功能简介：

    1、用户点击景区列表中的某项，生成请求。服务器收到请求后，查找景区详情，并返回给客户端。

客户端（POST）：

    URL:localhost:3001/get/spot_detail_id
    {
      "spotId":"123553",                // string
      "token":"12341",                 // string 
      "type":"1210",                    // string
     
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "bannerList":                    // string--轮播图
         [
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
             .....
         ],
      "info":                          // string
         {
           "name":"成都",               // string
           "describe":"描述",           // string
           "commentNum":"23",          // string
           "goneNum":"34",             // string
           "wantNum":"42",             //  string           
         }
       "viewList":
         [
           {
             "viewName":"锦里",
             "imageUrl":"封面URL"，
             "focusNum":"234",
             "describe":"成都的清明上河图",
             "price"："32",
             "address":"四川省成都市武侯区武侯祠。。。"，
           }，
           {
             "viewName":"锦里",
             "focusNum":"234",
             "imageUrl":"封面URL"，
             "describe":"成都的清明上河图",
             "price"："32",
             "address":"四川省成都市武侯区武侯祠。。。"，
           }
           ......
         ]
    }
    Failure:
    {
      "status": 1211 ,                  // string
      "message":"获取景区详情失败"        //  string 错误信息
    }




13、获取搜索景区详情（by Name）

功能简介：
    
    用户通过搜索栏，搜索景区时，给用户推荐该景点的景点列表。
    如：用户搜索峨眉山，则服务器生成峨眉山的景点列表
   

客户端（POST）：

    URL:localhost:3001/get/spot_detail_name
    {
      "name":"峨眉山",                  // string
      "token":"12341",                 // string 
      "type":"1310",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "bannerList":                    // string--轮播图
         [
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
             .....
         ],
      "info":                          // string
         {
           "name":"成都",               // string
           "describe":"描述",           // string
           "commentNum":"23",          // string
           "goneNum":"34",             // string
           "wantNum":"42",             //  string           
         }
       "viewList":
         [
           {
             "viewName":"锦里",
             "imageUrl":"封面URL"，
             "focusNum":"234",
             "describe":"成都的清明上河图",
             "price"："32",
             "address":"四川省成都市武侯区武侯祠。。。"，
           }，
           {
             "viewName":"锦里",
             "focusNum":"234",
             "imageUrl":"封面URL"，
             "describe":"成都的清明上河图",
             "price"："32",
             "address":"四川省成都市武侯区武侯祠。。。"，
           }
           ......
         ]
    }
    }
    Failure:
    {
      "status": 1311 ,                 // string
      "message":"搜索失败，没有该景区"          //  string 错误信息
    }


14、获取搜索景点详情(by name)

功能简介：

    用户从搜索框里面搜索某个景点，服务器收到请求后，根据搜索的景点名到数据库里面匹配，并返回匹配结果。

客户端（POST）：

    URL:localhost:3001/get/view_detail_name
    {
      "name":"锦里",                   // string
      "type":"1410",                   // string
      "token":"3242",                  // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "bannerList":                    // string--轮播图
         [
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
             .....
         ],
      "info":                          // string
        {
           "name":"锦里"，
           "describe":"锦里成都的清明上河图",
           "comNum":"32",
           "goneNum":"52",
           "wantNum":"89",
           "address":"四川省成都市武侯区....."，
           "openTime":"5:00-23:00",
           "playTime":"建议3小时",
           "phone":"2345112",
           "brief":"传说中......",
        }
    }
    Failure:
    {
      "status": 1411 ,                 // string
      "message":"获取详情错误"           //  string 错误信息
    }


15、获取景点详情(by ID)

功能简介：

    用户点击某一个景点项，通过向服务器传输该景点的ID来获取景点详情。

客户端（POST）：

    URL:localhost:3001/get/view_detail_id
    {
      "viewId":"1242",                   // string
      "type":"1510",                     // string
      "token":"3242",                    // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "bannerList":                    // string--轮播图
         [
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
           "picUrl":"url",
             .....
         ],
      "info":                          // string
        {
           "name":"锦里"，
           "describe":"锦里成都的清明上河图",
           "comNum":"32",
           "goneNum":"52",
           "wantNum":"89",
           "address":"四川省成都市武侯区....."，
           "openTime":"5:00-23:00",
           "playTime":"建议3小时",
           "phone":"2345112",
           "brief":"传说中......",
        }
    }
    Failure:
    {
      "status": 1511 ,                 // string
      "message":"获取详情错误"           //  string 错误信息
    }



16、获取出行记录

功能简介：

    用户点击出行模块，服务器收到请求生成出行记录推荐。
    有两种情况:
    1、该用户是登录用户，则向服务器传输用户ID，服务器查找该用户
       的数据表，如果该用户信息表中有学校信息，则生成该学校的出
       行记录，如果没有学校信息，则选取所在城市（客户端提供）的
       出行记录。
    2、该用户未登录，则向服务器传输的仅仅是当前所在城市的城市名
       然后服务器根据城市名，查找来筛选出该城市的发布记录。

客户端（POST）：

    URL:localhost:3001/get/release_trip
    {
      "type":"1610",                     // string 1610:登录用户； 1620 ：未登录用户
      "token":"10110",                   // string
      "uid":"421545",                    // string 如果是未登录用户，该字段为空
      "city"："成都",                     // string 该字段不为空，直接由客户端获取当前城市名称。
    }    

服务器：

    Success:
    {
      "status": 0 ,                    // string
      "tripList":                     // string
        [
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，           // string
            "oriSchool":"始发学校",         // string
            "oriCity":"始发城市",           // string
            "content":"附加说明",
            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，      // string
            "oriCity":"始发城市",
            "oriSchool":"始发学校",
            "content":"附加说明",
            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          ......
        ]
    }
    Failure:
    {
      "status": 1611 ,                 // string
      "message":"未搜素到该用户所在校园（城市）的出行记录"    //  string 错误信息
    }


17、获取搜索推荐（所有景区、热门景点）-

功能简介：

    服务器生成搜索推荐的列表，包括所有景区、以及热门景点；便于
    用户在点击搜索框、或者点击右上角的城市切换图标时，将所有景
    区结果，以及热门景点推荐给用户。

客户端（POST）：

    URL:localhost:3001/get/search_recommend
    {
      "type":"1710",                   // string
      "token":"10101",                 // string--该token为空
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      "provinceList":                   // string
         [
           {
             "name":"四川",
             "spotList":
               [
                {
                 "spotId":"12412",         // string
                 "spotName":"峨眉山",       // string
                },
                {
                 "spotId":"12412",         // string
                 "spotName":"峨眉山",       // string
                },
                ...
               ],
           },
           {
             "name":"武汉",
             "spotList":
               [
                {
                 "spotId":"12412",         // string
                 "spotName":"户部巷",       // string
                },
                {
                 "spotId":"12412",         // string
                 "spotName":"户部巷",       // string
                },
                ...
               ],
           },
           ...
         ]
    }
    Failure:
    {
      "status": 1711 ,                    // string
      "message":"获取搜索推荐失败"          //  string 错误信息
    }


18、搜索出行记录（学校、始发地、目的地）

功能简介：

    用户从出行模块的搜索框键入学校、始发地、目的地名。服务器收到
    请求后，在出行记录表中将键入的名称与学校、始发地、目的地进
    行，返回全部的相关记录。

客户端（POST）：

    URL:localhost:3001/get/search_trip
    {
      "name":"四川大学（或成都等）",          // string
      "type":"1810",                       // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                        // string
      "releaseList":                     // string
        [
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，           // string
            "oriSchool":"始发学校",         // string
            "oriCity":"始发城市",           // string
            "content":"附加说明",            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          {
            "type":1,                     // number [1,2] 1:游玩  2：拼车
            "destName":"目的地"，      // string
            "oriCity":"始发城市",
            "oriSchool":"始发学校",
            "content":"附加说明"，
            "goDate":"2014-02-13-19",      // string
            "releaseDate":"2014-02-11-10", // string
            "fromId":"发起人ID",            // string
            "fromName":"发起人名字"，        // string
            "phone":"13345521"，           // string
            "wantNum":5,                   // number
            "joinNum":3,                   // number
            "joinList":                    // string--加入人列表
               [
                 {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                   {
                  "uid":"2342",            // string
                  "name":"李四"，           // string
                 },
                 ....
               ]
          },
          ......
        ]
    }
    Failure:
    {
      "status": 1811 ,                 // string
      "message":"没有该搜索记录"          //  string 错误信息
    }

19、发表评论

功能简介：

    用户对景区、景点发表评论。
    服务器接收到请求之后，根据类型判断是景区、还是景点的评论。
    生成评论时间，之后将评论记录到数据表中。
客户端（POST）：

    URL:localhost:3001/comment
    {
      "uid":"1710",                    // string
      "name":"李四"，                   // string
      "token":"10101",                 // string
      "type":"1910",                   // string 1910:景区； 1920：景点
      "spotID":"12341",                // string -景点、景区的ID
      "content":"评论内容"，            // string 
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      "commentID":"12341",              // string
    }
    Failure:
    {
      "status": 1911 ,                    // string
      "message":"评论失败"          //  string 错误信息
    }

20、去过

功能简介：

    用户从景区、景点中，点击去过。服务器收到请求之后，根据类型
    在相应的景区、景点的去过数目中加一，同时关注数加一。

客户端（POST）：

    URL:localhost:3001/gone
    {
      "type":"2010";                     // string 2010:景区； 2020：景点，
      "spotId":"1710",                   // string
      "token":"10101",                   // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      
    }
    Failure:
    {
      "status": 2011 ,                    // string
      "message":"请求失败"                 //  string 错误信息
    }

21、想去

功能简介：

    用户点击想去某个景区（或者某个景点）。
    服务器根据类型区分是想去景区或是景点，服务器收到请求后生成
    想去的时间，然后根据ID来将信息存放至想去的数据表中。
    该操作主要是把想去用户的GPS信息存入服务器便于在地图上标注
    想去的用户.

    注：该操作必须是登录用户 

客户端（POST）：

    URL:localhost:3001/want_go
    {
      "uid":"171034",                    // string
      "type":"2110",                     // string 2110:景区； 2120： 景点  
      "token":"234",                     // string
      "spotId":"23415"，                 // string
      "date"："2015-07-12-20",           // string
      "gps":"143.2244/21.24412",         // string 前面是经度，后面是纬度      
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
    }
    Failure:
    {
      "status": 2111 ,                    // string
      "message":"请求失败"          //  string 错误信息
    }

22、获取想去的人GPS信息列表

功能简介：

    客户端向服务器发送获取想去的人GPS信息列表请求。服务器收到请
    球后，根据类型判断是景点还是景区。
    然后根据ID查找想去的表，生成筛选信息。原则上说，返回给客户端的应该
    是根据想去的时间筛选出当天的想去列表返回（这样该功能的实用性更强）。
    不过目前因为人数比较少,直接返回所有想去的人就可以了.
客户端（POST）：

    URL:localhost:3001/get/gps_list
    {
      "type":"2210",                   // string  2210：景区； 2220：景点
      "token":"10101",                 // string
      "spotId":"124415",               // string
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      "list":
        [
          {
           "uid":"124553",              // string
           "name":"23415",              // string 
           "gps":"42.3311/44.2213",     // string 前面经度，后面纬度
           "imageUrl":"头像路径",        // string
           "date":"2015-06-12-21"，     // string 点击想去的时间
          }，
          {
           "uid":"124553",              // string
           "name":"23415",              // string 
            "gps":"42.3311/44.2213",     // string 前面经度，后面纬度
           "imageUrl":"头像路径",        // string
           "date":"2015-06-12-21"，     // string 点击想去的时间
          }，
          ....
        ]
    
    }
    Failure:
    {
      "status": 2211 ,                    // string
      "message":"获取gps列表失败"          //  string 错误信息
    }

23、获取用户信息-

功能简介：

    用户本人、或者其他用户点击用户头像，进入浏览用户信息的界面。
    服务器收到请求之后返回用户信息。

客户端（POST）：

    URL:localhost:3001/get/user_info
    {
      "uid":"17120",                   // string 
      "token":"10101",                 // string
      "type":"2310"
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      "info":
        {
          "name":"张思",
          "phone":"手机",
          "sex":"女",
          "school":"四川大学",
          "compus":"江安校区",
          "grade":"大二",
          "address":"四川省成都市双流县",
        }
    }
    Failure:
    {
      "status": 2311 ,                    // string
      "message":"获取用户信息失败"          //  string 错误信息
    }
24、消息推送--（后面再做）

功能简介：

    当用户发布了出行（拼车）或者用户加入了出行（拼车）时，服务器
    要建立一个服务，在固定的时间内给用户发布出行（拼车）消息。

客户端（POST）：

    URL:localhost:3001/msg_promote
    {
      "type":"1710",                   // string
      "token":"10101",                 // string--该token为空
    }    

服务器：

    Success:
    {
      "status": 0 ,                     // string
      "provinceList":                   // string
         [
           {
             "name":"四川",
             "spotList":
               [
                {
                 "spotId":"12412",         // string
                 "spotName":"峨眉山",       // string
                },
                {
                 "spotId":"12412",         // string
                 "spotName":"峨眉山",       // string
                },
                ...
               ],
           },
           {
             "name":"武汉",
             "spotList":
               [
                {
                 "spotId":"12412",         // string
                 "spotName":"户部巷",       // string
                },
                {
                 "spotId":"12412",         // string
                 "spotName":"户部巷",       // string
                },
                ...
               ],
           },
           ...
         ]
    }
    Failure:
    {
      "status": 1711 ,                    // string
      "message":"获取搜索推荐失败"          //  string 错误信息
    }
25、头像上传-

功能简介：

    用户在个人中心里面，选择头像上传，服务器将用户的投降图片保存
    到服务器里面，同时生成头像的URL，便于以后用户重新登录的时候
    获取头像URL。

客户端（POST）：

    URL:localhost:3001/upload_image
    {
      "type":"2510",                      // string
      "token":"10101",                    // string
      "uid":"234523",                     // string
      "imageInfo":"图片流"                 // byte 字节
    }    

服务器：

    Success:
    {
      "status": 0 ,                      // string
      "imageUrl":"头像的URL地址"，         // string
      
    }
    Failure:
    {
      "status": 2511 ,                    // string
      "message":"上传失败"          //  string 错误信息
    }
