# OneDelphi 
## OneDelphi是叫兽(FLM)QQ:378464060基于Delphi IDE开发的三层中间件，开源免费，支持MVC及传统DataSet框架，使用的是Mormot2的HTTP通讯
>                                                 叫兽(FLM)出品
>                                                 QQ:378464060

------OneDelphi基于Delphi IDE开发的

------OneLaz基于Lazarus+fpc相当于D7开发的

------OneUniApp基于uniApp开发的快速对接OneDelphi与OneLaz    
  
------OneFastCleint快速开发框架,或报表或原型快速演示  
                                           
环境是基于 D11的，其它低版本，可能系统库 部份不兼容
### 1.控件包mormot2下载，群文件里面也有直接到群下载也行
  https://github.com/synopse/mORMot2
### 2.控件包mormot2源码路径加到Lib

----以上多是基础操作不会的不要问，没时间回答-----
### 3.打开 OneService.dpr 工程
### 4.编译，运行 即可
### 5.目前做好了MVC基础功能看源码单元httpServer->Controller->Demo-> DemoController.pas
   // 注册到路由 DemoController.initialization部份，路由如何注册
   // 注意，路由名称 不要一样，否则会判定已注册过，跳过
   
  // 多例模式注册
  OneHttpRouterManage.GetInitRouterManage().AddHTTPPoolWork('DemoA',TDemoController, 100, CreateNewDemoController);
  
  // 单例模式注册
  OneHttpRouterManage.GetInitRouterManage().AddHTTPSingleWork('DemoB',TDemoController, 100, CreateNewDemoController);
  
  // 方法注册
 OneHttpRouterManage.GetInitRouterManage().AddHTTPEvenWork('DemoEven',HelloWorldEven, 10);
 
### 6.直接用http页面输入地址请求相关url或者相关HTTP请求工具
  例:  http://127.0.0.1:9090/DemoA/GetPersonListT



## 目前传统客户端基本已完成;
### 1.数据打开保存,执行DML执行存储过程-对应Demo->OneClientDemo.dproj
### 2.客户端事务自由控制-对应Demo->OneCleintDemoCustTran.dproj
### 3.多个数据批量打开，批量保存-对应Demo->OneCleintDemoDatas.dproj
### 4.客户端post,get请求-对应Demo->OneCleintDemoPostGet.dproj
### 5.异步打开数据及保存-对应Demo->OneCleintDemoAsync.dproj
### 6.虚拟文件上传下载-对应Demo->OneClientDemoVirtualFile.dproj
### 7.大文件上传下载-对应Demo->OneClientDemoVirtualFile.dproj

## 更新日志
************2023-03-16***********  
服务端:  
	1.增加FastApi功能，无需写任何一句代码只需写SQL，即可获取相关账套数据  
	支持SQL查询数据，支持存储过程，格式如下  
	接口单元:OneFastApiController  
	接口地址:http://127.0.0.1:9090/OneServer/FastApi/DoFastApi  
	apiCode:FastApi接口代码  
	apiData:FastApi请求数据,只能是Json对象或数组  
	apiParam:FastApi请求条件参数,只能是Json数组  
	{
    	"apiCode":"TEST",  
    	"apiData":{},
    	"apiParam":{"FBillID":"1AAE0AFFE4E649E7A5EE8E0899AFB81C"}
	}

客户端:  
	1.OneFastClient增加FastApi设置界面  

************2023-03-16***********
服务端:  
	1.增加UrlPath风格的请求,单元示例 DemoUrlPathController  
    		// 请求 url xxxx/DemoUrlPath/OnePathTest/flm123  
    		function OnePathTest(id: string): string;  
    		// 请求 url xxxx/DemoUrlPath/OnePathTest/flm123/18  
    		function OnePathTest2(id: string; age: integer): string;  
	2. OneHttpRouterManage中的类TOneRouterItem改成TOneRouterWorkItem  

客户端:
	1.OneClientConnect post数据增加zlib压缩,以及zlib解压
	
************2023-03-13***********  
服务端:  
	1.主要增加OneFastCleint相关对接单元  
	2.增加TOneTokenManage.TokenTimeOutSec Token失效时间功能处理  
	3.增加 ZTManageController开放获取账套信息  
	4.以及一些优化修正  
客户端:  
	1.OneCleint包控件TOneDataSet增加  
                   跟据SQL检测某个字段是否重复  
	   function CheckRepeat(QSQL: string; QParamValues: array of Variant; QSourceValue: string): boolean;  
	   执行DML语句,update,insert,delete语句，依托于DataSet但不会影响本身DataSet任何东东  
                   function ExecDMLSQL(QSQL: string; QParamValues: array of Variant; QMustOneAffected: boolean = true): boolean;  
                   及一些功能增加  
                2.TOneConnection  
                    //验证失败回调事件,比如回调登陆界面  
                    FTokenFailCallBack  
                    获取账套信息能力  
                    function OneGetZTList(Var QErrMsg: string): TList<TZTInfo>;  
                    function OneGetZTStringList(Var QErrMsg: string): TStringList;  
                     //功能扩展  
                     function GetResultBytes(const QUrl: string): TOneResultBytes;  
                     及一些功能增加和修正  
                 3.OneFastClient  
                     是的，一个快速开发  
有的东东我增加了，我忘了大体功能或什么的。。。不在以上描述，自已对比大体代码  

************2023-02-26***********  
服务端:  
	1.修正 SaveData事务处理多个数据集前面提交后面出错，前面事务未回滚问题。在迁移代码，漏了个not  
客户端:  
	1.OneCleint包控件TOneDataSet增加  
	一次性打开多个数据集  
	   function OpenDatas(QOpenDatas: array of TOneDataSet): boolean;  
	示例  
	  if not qryModule.OpenDatas([qryModule, qryData, qryUI, qryControl, qryButton, qryButtonpop]) then  
	2.OneCleint包控件TOneDataSet增加  
	  function SaveDatas(QOpenDatas: array of TOneDataSet): boolean;  
	示例  
	  qryModule.SaveDatas([qryModule, qryData, qryUI, qryControl, qryButton, qryButtonpop])  

************2020-02-24***********  
服务端:  
	1.优化 OneMultipart 对multipart/form-data解析，以及BUG，此单元值得你拥有, 解析multipart函数,D基本没有  
	2.优化 THTTPCtxt.FRequestInContent: RawByteString; 由string改成RawByteString保持原本接收到的字符串  
	3.UniGoodsController增加  
    '  //文件上传示例
    	function PostGoodsImg(QFormData: TOneMultipartDecode): TActionResult<string>;
 	    //获取文件示例
    	function OneGetGoodsImg(imgid: string): TActionResult<string>;'  
	4.以及一些BUG和优化  
客户端:  
oneuniapp客户端:  
	1.增加商品编辑,文件上传  
	2.优化一些函数  

************2023-02-17***********  
服务端:  
	1.增加OneUniDemo与oneUniapp相关单元对接, 在线地址:http://house.callbaba.cn/#/
	 或安装OneUniApp.apk,在开源群里有  
	2.服务端增加删除虚拟文件的功能  
	
客户端:  
	1.增加Demo->OneClientDemoVirtualFile删除文件的功能  
	
OneUniApp客户端:  
	1.增加制作订单交互流程Demo  
	 在线地址:http://house.callbaba.cn/#/  
	或者下载Apk,去开源群下载  
QQ群：193878346  
	

*************2023-02-09**************  
服务端:  
	1.统一返回值类名称及参数大小写叫法与OneLaz,OneuniApp一至,	如果有需要OneUniapp联系**叫兽(FLM)QQ:378464060**一年399  
	2. OneLaz同步增加线程变量及参数结果释放优先问题  
	3.OneLaz同步支持Uos,depbian系统  
	4.OneLaz修复各种接口  
客户端:  
	1.统一返回值类名称及参数大小写叫法  
	
*************2023-02-06**************  
服务端:  
	1.优化MVC 参数和返回结果参数的释放优先问题  
	2.增加MVC 线程变量的使用,可以在无HTTP上下文参数，直接调用HTTP上下文参数  
	3.增加对接OneUniApp的Demo单元,服务端 UniDemoController  
客户端:  
	1.增加OneUniApp目前只对OneLaz VIP会员免费开放  
             
*************2023-02-01**************  
服务端: 修正文件名称是中文返回错误(oneweb,oneweb)虚拟路径输出静态文件  

*************2023-01-29**************  
服务端： 
	增强web表单上传，后台接收处理  
	参考服务端Demo->DemoWebFileController  
              // 解析 multipart/form-data提交的数据,只需要参数类型是 TOneMultipartDecode就行，                   其它的交给底程处理解析  
              ' function WebPostFormData(QFormData: TOneMultipartDecode): TResult<string>;'

*************2023-01-28**************  
服务端：   
	增强文件分块上传下载  
客户端:  
	增强文件分块上传下载,及批量文件上传功能  
	增强Demo-OneClientDemoUpDownChunt  
 
*************2023-01-03**************  
OneDelphi正式版,正式发布.  
服务端:  
	1.完善Token功能  
	2.服务端主界面增加Token查看管理  
	3.服务端主界面增加win一些启动功能  
	4.其它优化和修正  
客户端:  
	1.OneCelint包增加与服务端交互的验证的机制功能  
	       OneClientConnect.MakeUrl安全机制，有兴趣的去看，在其URL拼接 ?token=xx&time=xxx&sing=xxxx  
	       lSign := self.FToKenID + lTimeStr + self.FPrivateKey  MD5换算来的  
	       ToKenID和PrivateKey由DoConnect连接向服务端申请的安全码和秘钥，秘钥不传输，只能参与签名  


*************2023-01-01**************  
首先在这边祝大家新的一年合家欢乐，新年新气象  
服务端:  
	1.完善ORM功能  
	2.增加Demo-> DemoOrmController  

*************2022-12-29**************  
服务端:  
	1.增加静态站点功能  
	第一种:把文件放在运行目录 OnePlatform\oneWeb 目录下面  
	如下 oneweb特定标识，表明要访问运行目录  OnePlatform\oneWeb 下文件  
	http://127.0.0.1:9090/oneweb/admin/index.html  
                最终访问: D:\devTool\delphi\project\OneDelphi\OneServer\Win64\Debug\OnePlatform\oneWeb\admin\index.html  
	第二种:把文件放在虚拟目录下  
	例: 虚拟路径代码(TEST)---实际物理路径(D:\test)  
	如下 onewebv特定标识,表明要访问虚拟路径文件 /test/ 虚拟路径代码  admin/index.html 路径代码  
                http://127.0.0.1:9090/onewebv/test/admin/index.html  
	最终访问:D:\test\admin\index.html  
	2.增加文件访问Demo  
	 DemoWebFileController  

*************2022-12-28**************  
服务端:  
	1.界面增加路由情况查看,以及路由注册失败原因  
 	2.MVC增加方法能数为 TJsonObject,TJsonArray,TJSonValue参数，有且只能有一个参数系统单元system.json  
	例:DemoJsonController=> function GetJsonParam(QJsonObj: TJsonObject): string;  
	3.新增的ORM在11版本以下不兼容，进行了修正兼容。  
	4.优化 OneHttpRouterManage.TOneRouterItem相关方法及属性  
	5.其它修正及优化  
	
*************2022-12-27**************  
服务端:  
	1.完成orm查询测式 Demo：DemoOrmController  
	
*************2022-12-26**************  
有事外出处理事情  

*************2022-12-25**************  
有事外出处理事情  

*************2022-12-24**************  
在次感谢  
今天收到QQ群友的  蓝色  一万元赞助，算是今年中最幸运的一件事吧。倒霉了一年，希望有个新的开始,  
谢谢支持鼓励  

*************2022-12-23**************  
在次感谢  
今天收到QQ群友的  蓝色  一万元赞助，算是今年中最幸运的一件事吧。倒霉了一年，希望有个新的开始,  
谢谢支持鼓励  

*************2022-12-22**************  
今天收到QQ群友的  蓝色  一万元赞助，算是今年中最幸运的一件事吧。倒霉了一年，希望有个新的开始,  
谢谢支持鼓励  

*************2022-12-21**************  
搭建ORM基类  

*************2022-12-20**************  
搭建ORM基类  

*************2022-12-19**************  
搭建ORM基类  

*************2022-12-18**************  
服务端:  
	1.增加对接OneClient分块上传下载功能  
客户端:  
	1.增加对接服务端分块上传下载的功能  
	2.增加Demo文件分块上传下载  

今天收到QQ群友(790166332)的两箱橙子，挺好吃的,愉快的一天，谢谢对作者的支持和鼓励。  

*************2022-12-17**************  
休息  

*************2022-12-16**************  
服务端:  
	1.增加对接OneClient文件上传下载(小文件), 单元 VirtualFileController  
	 2.修正一些功能  
客户端:  
	1.增加对接服务端小文件上传下载，一次性上传下载  
	2.增加文件上传下载Demo  
	3.修正一些功能  

*************2022-12-15**************  
心累，休息。   

*************2022-12-14**************  
心累，休息。  

*************2022-12-13**************  
服务端:  
	1.加快openData取数据文件下载模式压缩，直接压缩流.  
	2.返回的JSON数据不在采用uncoide编码 \uxxxx 直接UTF8编码  
客户端:  
	1.修正自定义事务控制DemoBUG及异常，纠正DEMO写法  
        2.增加DataSet.OpenAsync,DataSet.SaveAsync  
        3.增加异步Demo	 
        4.增加Api使用Demo  
        5.优化一些功能  
### Delphi三层中间件 Dephi中间件 Delphi开源中间件
