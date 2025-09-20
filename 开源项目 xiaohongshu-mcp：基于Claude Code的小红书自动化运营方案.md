# 一、安装go环境

官网：https://go.dev

或国内镜像网站：https://golang.google.cn/

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914174319743.png)

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914192912036.png)

# 二、下载项目源码

项目地址：

https://github.com/xpzouying/xiaohongshu-mcp

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914151520455.png)

# 三、登录小红书

解压上一步下载的压缩包，进入项目目录，执行以下命令，保存一下小红书登录状态：

```
go run cmd/login/main.go
```

注意，首次登录会自动下载go依赖![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914150803517.png)

过一会儿，依赖下载完成后，可以看到它会弹出一个新的浏览器窗口，是小红书登录页面，需要扫码登录后，本地会自动存储登录状态，下次MCP打开就不需要重复登录了：

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914151034863.png)

扫码完成登录后，命令行就会提示：登录成功，同时浏览器窗口会自动关掉，不用担心，此时登录状态已经被记录下，下次再打开就不要再需要登录了

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914151251874.png)



# 四、启动小红书MCP服务

启动模式分为两种：无头模式，非无头模式

由于本项目本质上属于RPA项目，原理是通过定位元素操作浏览器的方式实现自动化，所以操作浏览器是基于playwright，它支持无头模式和非无头模式。

无头模式就是在不打开浏览器的情况下操作浏览器页面，是完全运行在后台的，你也看不到它在操作浏览器

非无头模式就是把无头模式给显化了，可以看到网页在被点击操作，本教程为了演示方便，让大家看到效果，所以使用非无头模式，大家在自己使用的时候，可以使用无头模式吗，避免在任务执行期间影响正常使用电脑。

如果要使用无头模式，只需要把下面指令的 `false` 改为 `true` 即可

```
go run . -headless=false
```



![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914151825596.png)



可以看到，MCP服务启动使用了 `18060`  端口



# 五、使用claude code操作MCP

## 一）添加MCP到CLode Code

先添加MCP到`claude code` :

```
claude mcp add --transport http xiaohongshu-mcp http://localhost:18060/mcp
```

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914153126488.png)



## 二）可用 MCP 工具

连接成功后，可使用以下 MCP 工具：

- `check_login_status` - 检查小红书登录状态（无参数）
- `publish_content` - 发布图文内容到小红书（必需：title, content, images）
- `list_feeds` - 获取小红书首页推荐列表（无参数）
- `search_feeds` - 搜索小红书内容（需要：keyword）
- `get_feed_detail` - 获取帖子详情（需要：feed_id, xsec_token）
- `post_comment_to_feed` - 发表评论到小红书帖子（需要：feed_id, xsec_token, content）





# 六、工具测试

## 一）测试自动评论

自动评论，更多用在养号的时候，他从搜索到点击进入详情看帖子，再到评论点赞，都可以完全做到自动化

```
请使用 xiaohongshu-mcp ,帮我搜索：西贝事件，找到第一篇评论量最多的帖子，查看帖子详情和评论区内容，找到最热的一条评论，然后在该评论下发布一条评论，评论内容根据你的理解，自己编写，要求内容积极向上，不要有负面思想
```

第一步：找评论最多的帖子

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914173104010.png)



第二步：找到最热评论

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914173202078.png)



第三步：自己根据最热评论内容发布自己编写的评论

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914173300720.png)

查看小红书评论区自己的评论：

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914173901997.png)





## 二）全自动发布

一般我们账号是不敢全自动从搜索到采集再到发布的，发布之前必须要经过审核，但是如果对于想要测试选题的，就可以试试全自动开车，注意用小号测试！！！

```
请使用 xiaohongshu-mcp ,帮我搜索：claude code，找到最高评论的帖子，进入详情提取帖子内容，并查看评论区最热评论，并根据采集到的信息，写一篇小红书文案，帖子图片地址：C:\Users\Administrator\Desktop\rpa\xiaohongshu-mcp\claude-code.jpg，并将这篇帖子发布出去
```

第一步：找帖子

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914191332323.png)

第二步：进入详情并找到最热评论

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914191410096.png)



第三步：根据帖子内容自动创作内容

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914191504616.png)

第五步：发布

这里发布时候，提示标题长度超过限制，他会自动纠错，然后继续发布

![](https://wamplempic.oss-cn-beijing.aliyuncs.com/mdimage-20250914191656194.png)

