# Releases
------
## v0.11.0 20220530
#### Features
- 组织和项目支持批量邀请用户
- 环境的部署通道改为使用 runner tag 进行匹配
- 新增，支持 ldap 登陆
- 新增，环境锁定功能
- 新增，环境归档后名称添加后缀
- 新增，环境支持设置工作目录
- 新增，环境和系统设置中增加部署任务的步骤超时时间设置(默认 60 分钟)
- 组织资源查询支持搜索资源属性关键字及环境和 provider 过滤
- 新增，组织和项目概览页统计数据
- 新增，aliyun 资源费用采集
- 新增环境概览页面，展示环境详情和统计数据
- 新增，资源账号增加项目关联及 provider 和费用统计选项
- 新增，审批时展示资源变更数据
- 项目管理员新增组织用户邀请权限
- 新增价格预估功能，在审批部署任务时展示资源变更的预估费用情况
- 增加 IaC Store network mirror 支持，配置了 IaC Store 服务地址后会自动启用该地址作为 network mirror server
- 接入自研 cloudcost 询价服务，目前支持的产品 aliyun ecs/disk/nat/slb/eip/rds/redis/mongodb
- 支持 ldap ou 或用户预授权
- 支持 consul 开启 acl 认证

#### Enhancements
- vcs 创建或编辑保存时对 token 有效性做校验
- 漂移检测任务运行时会再次获取环境最新一次执行部署时使用的配置信息
- 优化环境可部署状态检查接口，现在只检查环境关联的Stack 和 vcs 是否有效
- 任务类型为 apply 但未执行 terraformApply 步骤的漂移检测任务在部署历史中不展示
- 优化自动纠偏任务执行完 plan 后会判断是否有漂移，若无漂移则提前结束任务
- 优化 vcs 连接失败时的报错
- 组织层级的Stack列表接口返回关联环境数量
- 环境增加已销毁状态
- consul 服务创建锁并自动重新注册

#### Fixes
- 修复设置工作目录后 tfvars 和 playbook 文件路径保存错误的问题
- 修复 playbook 中输出中文内容会乱码的问题
- 修复开启 terraform debug 时执行 playbook 会报错的问题
- 修复工作目录不支持二层以上子目录的问题
- 修复导入Stack时因为没有选择关联项目导致报错的问题
- 修复Stack中的敏感变量导入后变为乱码的问题
- 修复查询资源账号时敏感变量值没有返回为空的问题
- 修复项目列表默认展示了已归档项目的问题
- 修复创建Stack或者测试策略组不绑定项目会报错的问题
- 修复环境锁定后 plan 完成可以发起部署的问题

------
## v0.9.1 20220310
**Features**

- 环境支持设置及展示标签
- 环境创建、销毁、重新部署时都发送 kafka 消息，通知环境最新资源数据


------
## v0.9.0 20220307
**Enhancements**

- 优化 vcs 服务报错，将 vcs 错误进一步细分为连接错误、认证错误等
- 项目Stack列表的“活跃环境”字段改为关联环境，点击数字可跳转到环境列表页面

**Features**

- 合规策略组改用代码库进行管理，支持通过分支或 tag 来管理版本
- 重新实现的合规检测流程和检测引擎
- 执行界面增加Stack和环境的合规开关和合规策略组绑定功能
- 新增合规管理员角色
- 新增环境搜索功能，支持通过环境名称和Stack名称进行搜索
- 环境部署历史增加触发类型字段，记录部署任务的触发来源

**Fixes**

- 修复设置工作目录后无法选择工作目录下的 ansible plabyook 和 tfvars 文件的问题
- 修复组织管理员无权修改项目名称和描述的问题
- 修复任务 plan 失败后可能长时间不退出的问题
- 修复 gitee 私有仓库无法认证的问题
- 修复导出Stack时资源账号的敏感变量加解密处理错误的问题
- 修复任务驳回后状态显示为“失败”的问题
- 修复触发器触发归档环境部署问题
- 修复部分查询未正常处理软删除的问题


------
## v0.8.1 20211214
**Fixes**

- 修复新组织中创建环境时接口报错的问题
- 修复环境有敏感变量时执行部署报解密错误的问题
- 修复执行任务容器异常退出会导致任务一直处于执行中状态且环境的资源一直累积的问题


------
## v0.8.0 20211210
**Enhancements**

- 优化环境列表和环境详情的展示效果

**Features**

- 新增环境漂移检测功能
- 新增环境资源、模型可视化展示
- 新增Stack及其关联数据的导出导入功能
- 新增创建Stack时名称和工作目录有效性检查
- 新增加 VCS 编辑功能
- 新增执行 MR/PR 触发的 plan 任务时将日志回写到 review comment
- 任务通知消息中增加任务类型说明

**Fixes**

- 修复步骤超时后不显示日志的问题
- 修复编辑Stack时仓库名称可能显示为 id 的问题


------
## v0.7.1 20211116
**Features**

- 新增 runner 的 offline mode

**Fixes**

- 修复预置 provider 不生效的问题


------
## v0.7.0 20211105
**Enhancements**

- 优化从组织和项目中移除用户功能
- 组织中编辑用户时允许修改姓名和手机号
- 优化环境列表、环境详情展示样式

**Features**

- 新增自定义 pipeline 功能，并将任务执行过程分步展示
- 新增组织内资源查询功能
- 新增资源账号管理功能

**Fixes**

- 修复从组织中删除用户后用户在项目中依然存在的问题
- 修复设置环境自动触发 plan/apply 功能报错的问题


------
## v0.6.0 20210928
**Features**

- 新增合规检测功能，平台管理员可进行合规策略管理
- 新增消息通知功能，支持邮件、钉钉、微信、Slack 事件通知
- 新增任务重试功能，可在环境设置中开启执行失败自动重试
- 新增 tfvars 文件和 playbook 文件内容查看功能
- 新增 terraform 版本选择功能，并支持版本的自动匹配
- 新增环境的资源详情展示，点击资源名称可查看资源详情
- 新增选择型变量，添加环境和 terraform 变量时可下拉选择
- 任务增加审批驳回状态，审批驳回不再显示为“失败”

**Fixed**

- 修复环境部署过程中允许删除关联Stack的问题
- 修复存在活跃环境的Stack在列表中活跃资源数显示为 0 的问题


------
## v0.5.0 20210728
**Features**

- 全新 0.5 版本发布



