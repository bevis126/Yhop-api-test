# 测试方法

**获取Token**  

+ 先在主控服务器直接测试slat-api接口（8000端口是salt-api服务端口）  

> curl --location --request POST 'http://127.0.0.1:8000/login' --header 'Content-Type: application/x-www-form-urlencoded' --data-urlencode 'username=saltapi' --data-urlencode 'password=fe1673dd2f25c5f351da81e7f1f684c4' --data-urlencode 'eauth=sharedsecret'
  
*测试结果： (返回json格式)*  

{"return": [{"token": "3857debd63f3ff29895922881231452dabdc7790", "expire": 1688664495.164579, "start": 1688621295.164579, "user": "saltapi", "eauth": "sharedsecret", "perms": [".*", "@runner", "@wheel", "@jobs"]}]}

+ 通过主控服务器webserverr的ops接口测试（9080为webserver服务端口）  

> curl --location --request POST 'http://127.0.0.1:9080/ops/login' --header 'Content-Type: application/x-www-form-urlencoded' --data-urlencode 'username=saltapi' --data-urlencode 'password=fe1673dd2f25c5f351da81e7f1f684c4' --data-urlencode 'eauth=sharedsecret'  

 *通过ops测试，正常是返回的是403（因为webserver开启了加密）*

+ 通过返回的token获取事件（这步可以省略）  
  
>curl -SsNr GET http://127.0.0.1:9080/ops/events --header "X-Auth-Token: a11aef1c7db42d8f8faaee267c4629b0b063ecf7"


  
+ 云端服务器测试外网webserver接口（需要把x-upstream的地址替换成你要测试项目的地址）
  
> curl -X POST http://127.0.0.1:9002/login --header "x-upstream: http://www.yhop-test.com:9080/ops/" --header "Content-Type: application/x-www-form-urlencoded" --data-urlencode "username=saltapi" --data-urlencode "password=fe1673dd2f25c5f351da81e7f1f684c4" --data-urlencode "eauth=sharedsecret"

> curl -X POST http://172.16.16.11:9002/login --header "x-upstream: http://www.yhop-test:9080/ops/" --header "Content-Type: application/x-www-form-urlencoded" --data-urlencode "username=saltapi" --data-urlencode "password=fe1673dd2f25c5f351da81e7f1f684c4" --data-urlencode "eauth=sharedsecret"

![image](https://github.com/bevis126/Yhop-api-test/assets/27944125/db9848ac-6c73-4959-8ba0-8a62b91a620d)

测试结果如上图为测试正常，可以到运维平台中添加主控节点及应用部署
