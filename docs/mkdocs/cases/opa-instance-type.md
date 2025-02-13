# 使用合规策略限制测试环境的机器规格
该教程演示通过合规策略限制环境使用的机器规则，可用于限制测试或者开发环境只允许使用指定规格的服务器实例，避免资源浪费。

## 创建策略组
### 1. 进入合规中心
使用平台管理员或者合规管理员账号登录平台, 点击页面左上角的“进入合规”链接进入合规中心页面：
![picture 2](../images/57cf21801ec04467247b7a7bb44da0299e71fd181efcf5a1a641d0c9b0544477.png)  

### 2. 新建策略组
点击的“策略组”菜单进入策略组管理，然后点击“新建策略组”:

![picture 3](../images/ce234a05180c0ad3953ac6dabc9447959e5cf71418e0c9c17c1e39e8a037601f.png)  


进入策略组创建页面后先选择策略组的来源的，我们这里选择使用 IaC Store 上已经发布的策略组，策略组名称和版本如下图，完成后点击“下一步”：

![picture 4](../images/99764dad4d3f03e3b6d95c3d903ac1c9877af1621a59b77bfa12f1d64d0a4864.png)  


配置策略组名称后“提交”创建：

![picture 5](../images/cc9b269fe721f90f410b42e4ffe8844d3d2644ba944a7abdee96682599d3a469.png)  

策略组创建完成：

![picture 6](../images/261f02361560b06432bfd4ecb24bdd5f20042efa7233e9c5a3276ee8d7cb3217.png)  

:::tip
这个策略组中只有一条策略，该策略用于检查创建的 aliyun ecs 实例是否为 small / nano 规格。
:::

## 创建Stack并绑定策略组
点击页面左上角的“进入执行”链接，重新进入执行中心，然后点击组织视图中的“Stack”菜单，进入Stack管理页面:

![picture 8](../images/202278d754d5156449aad09b14e661445ca48109607dc6a4f01d24c732e68416.png)  

点击“新建Stack”，然后按照如下配置创建Stack：

![picture 7](../images/92ddb868065e869a46ee9f62c27882ab8d8c1a19eca4c05973c34049a58cac98.png)  

在Stack的变量配置中选择 prod-env.tfvars 文件：

![picture 9](../images/ba1d63f6ec2404477bf63c2664d4f4f677e3f80e4f0099f8e55d440433fa4dc8.png)  

:::tip
prod-env.tfvars 是生产环境部署时使用的配置文件，其中 aliyun ecs 实例的规格为 ecs.g7.large。
:::

下一步，设置Stack名称，然后为Stack开启合规检测，并绑定“阿里云 ECS 实例规格检查”合规策略组：

![picture 10](../images/bc20e5edbfe1138d3f2d7a8604c04cf8550fe5ce20cd34d4085577f5a72f5769.png)  

最后将Stack关联到项目，完成创建：
![picture 11](../images/95fa52cb0dafe0353954e2caee73064a1dc12c466257194cfb651b103a6064d7.png)  

:::tip
这里我们选择将Stack关联到“演示项目”，后面创建环境也是在演示项目下进行。当然也可以选择任意项目关联，后续创建环境也在关联的项目中进行即可。
:::

Stack创建后会立即触发一次合规检测：

![picture 12](../images/934c16732117aa2dca985fff0f967239af55ea26aa33f6f9592fcc5eae2d3055.png)  

合规检测完成后可以看到Stack的合规状态为“不通过”：

![picture 13](../images/58b37e72370927544fa16bfa6c5ae1d1bac01b1479e1a01fc277244ad542dde5.png)  

点击合规状态可以打开检测详情页面，查看具体不通过的原因：

![picture 14](../images/f0c48db33fa8c87b8e2df2c2ee1db0c83f094bf4c6a52b8004b1d1ee9585a9ea.png)  


## 基于Stack部署环境
在上面的步骤中我们已经完成了Stack的创建，并绑定了合规策略。下面我们基于这个Stack创建一个环境，演示通过合规策略限制环境允许部署的实例规格。

进入“演示项目”，然后点击“Stack”菜单，在Stack列表中选择刚刚创建的Stack进行部署：

![picture 15](../images/adad5c412e9d0f52f11f118f4e4c615475853f197667eb2e1594bed37e92abda.png) 


点击部署后会进入部署新环境页面，如下图配置环境名称和分支，并添加资源账号：
![picture 16](../images/158b6e9efd27491338d3a7b9a3f0bfbdde68644d6a96d722fccbab2656d6f94b.png)  


打开高级设置中的执行设置，选择一个部署通道标签：

![picture 18](../images/64614107f15e966106fd1518a32748ac73a9d658fbb371baa1f05c03cd1e7577.png)  

打开高级设置中的合规设置，勾选“合规不通过时中止部署”:

![picture 17](../images/675b1c169509eb3ad057401ec5b0faf4c1dff6372c3bbd0a3c24701cc1d6ef03.png)  



然后点击“执行部署”，开始部署新环境：

![picture 20](../images/ce695a606a253ab27fb30701882f6e7aca322b51013998126569c4894d8624ae.png)  


当执行到 “OPA Scan” 步骤时因为实例规格与合规策略组中的策略冲突，导致未通过检查，部署过程中止：

![picture 19](../images/c9934e27899bcca2e609103f340ebf1c1abd77f13c5f8a011252609ae0e57109.png)  


:::tip
此时环境和任务都会变为失败状态，且环境也显示为“不合规”。
:::


通过应用合规策略，我们实现了针对测试或开发环境限制其使用的实例规格的目的，避免创建不必要的实例规格导致资源浪费。
