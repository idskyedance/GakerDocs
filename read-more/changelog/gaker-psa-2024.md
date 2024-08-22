# 集客云 PSA 2024

### `v4.34.0` 2024.08.20

<mark style="color:orange;">**自定义应用校验规则**</mark>

在创建和编辑数据时，通过设定校验规则，来屏蔽一些不合法的操作，然后提醒给操作人员。这样，可以更加灵活的支撑各种场景下的表单校验，你可以在「应用设置」的「表单校验」中进行相关配置。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/object_validate_rule.jpg" alt=""><figcaption><p>表单校验配置</p></figcaption></figure>

<mark style="color:orange;">**公海池增加新的手动分配规则**</mark>

公海池分配规则中新增手动分配规则：公海成员可见不可领取，管理员主动分配。在这种分配规则下，公海成员可以看到公海池中的所有数据，但不能领取。当成员需要其中的数据时，需要主动向负责人申请，然后由负责人分配给他。

你可以在「扩展插件」->「公海池」配置中开启此分配规则。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/pool_alloacte.jpg" alt=""><figcaption><p>公海池分配规则设置</p></figcaption></figure>

### `v4.33.2` 2024.08.10

<mark style="color:orange;">**增强富文本**</mark>

富文本增加了“插入跟进记录”和“插入业绩指标”两个功能，用于支持各种工作汇报的场景。具体的应用场景，可以参考：[从此，让销售爱上工作汇报](../use-cases/make-sales-fall-in-love-with-work-reporting.md)

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/rich_text_strengthen.png" alt=""><figcaption><p>工作汇报示例</p></figcaption></figure>

### `v4.33.0` 2024.08.06

<mark style="color:orange;">**目标管理支持自上而下的 KPI 管理**</mark>

目标设置有两种应用场景

第一种：每个用户自主设置OKR添加对齐，强调员工自主性，缺点是不太适合自上而下的KPI考核模式，当公司希望针对某个考核角色，例如销售设置统一的考核标准时，每个人自主设置目标就很容易设置错误难以保证考核标准的一致性，并且无法提供上级或管理员的全局视角来支持查看和管理下级或全部用户的目标。

第二种：支持管理员后上级自上而下为考核对象制定目标，通常针对某类角色例如销售提前设置目标模板，添加目标时可根据模板中预设的指标，只需填写目标值即可快速完成添加，有些公司需要限制员工自主添加和修改。

这个版本我们做了大量功能来同时支持这两个场景，你既可以把目标管理当成 OKR 来用，可以把它当成 KPI 来用。

首先，我们支持目标模板，方便管理员和用户快速创建目标

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-01.png" alt=""><figcaption><p>目标模板配置</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-02.png" alt=""><figcaption><p>新建目标模板</p></figcaption></figure>

其次，创建目标时，既支持给自己创建目标，也支持给自己的下级设置目标

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-03.png" alt=""><figcaption></figcaption></figure>

最后，除了之前的对齐页面，我们还增加了表格视图，方便管理者从全局视角查看和管理团队的目标和业绩

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-04.png" alt=""><figcaption></figcaption></figure>

### `v4.32.16` 2024.07.20

<mark style="color:orange;">**新增助手功能**</mark>

在日常实践中，我们发现几乎 99% 的人遇到系统操作问题时，是不会去查看我们的帮助中心和产品操作手册的。这个懒没关系，主要是产品的复杂度导致文档内容太多太多，很难快速掌握到有效的信息。为了解决这个问题，我们在这个版本中增加了「助手」功能。我们在后台将所有与产品相关的手册和知识库丢给了字节的大模型“扣子(Coze)”，然后对接了它的 API，以问答的方式来解决用户在日常中遇到的产品使用上的问题。

你可以在点击首页的助手按钮，尝试这个功能：

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-05.png" alt=""><figcaption><p>首页导航菜单</p></figcaption></figure>

这个版本中，我们还对应用端的很多 UI 细节做了优化，希望能给你更好的体验。

### `v4.32.10` 2024.07.10

<mark style="color:orange;">**业务流程增加“SOP 执行动作”配置**</mark>

在业务流程状态列表增加一列“SOP设置”入口，以支持流程达到特定状态时，系统通过自动化工作流，执行“数据操作、消息通知、创建代办任务”等动作，强大的业务流程自动化从这个功能开启！

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-06.png" alt=""><figcaption><p>状态定义</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-07.png" alt=""><figcaption><p> SOP 配置</p></figcaption></figure>

<mark style="color:orange;">**功能优化**</mark>

* 优化业务流程状态新增和删除操作交互，减少业务流程配置保存失败的可能性
* 业务流程锁定配置支持复制操作
* 优化富文本工具栏样式
* 修复工作流程中周期性时间触发任务配置白屏的问题
* 数据列表最后一列如果是数字类型字段，设置 ICON 会挡住部分信息，这个版本优化了这个问题

### `v4.32.5` 2024.06.30

<mark style="color:orange;">**新增操作指引功能**</mark>

在日常工作中我们经常会遇到客户问这样的问题：

* 销售发货单已经在审批通过了，接下来该由谁操作，流转到什么节点？
* 一个刚入职的同事在系统上收到不少审批，但具体应该关注审批上的什么内容，哪些审批该通过哪些不该通过？
* 我收到不同同事给我创建的待办任务，但这些任务应该怎么处理，我需要不停的问题相关同事，费时费力

为了解决上述问题，我们在业务流程、审批流程、待办任务三个最常用的功能中增加了操作指引功能，管理员可以将公司积累的最佳实践设置为操作指引，在工领取任务时提供操作指引。这样会比企业正式的文档知识库更方便，也可能将企业的最佳实践用于日常的任务执行中。为了方便实用，我们将操作指引分别抽象为：状态说明、审批要求、任务攻略三个功能点，其设置与效果如下图所示(点击图片放大查看)。

**状态说明**

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-08.png" alt=""><figcaption><p>状态说明配置</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-09.png" alt=""><figcaption><p>状态说明展示效果</p></figcaption></figure>

**审批要求**

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-10.png" alt=""><figcaption><p>审批要求配置</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-11.png" alt=""><figcaption><p>审批要求展示效果</p></figcaption></figure>

**任务攻略**

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-12.png" alt=""><figcaption><p>任务攻略配置</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-13.png" alt=""><figcaption><p>任务攻略展示效果</p></figcaption></figure>

<mark style="color:orange;">**功能优化**</mark>

* 优化管理后台导航菜单，配置更方便
* 优化审批卡片样式

### `v4.32.0` 2024.06.20

<mark style="color:orange;">**待办任务节点支持设置完成条件**</mark>

线下完成的任务需要到系统中记录并反馈，如果担心员工完成任务后忘记填写、找不到操作入口或者填错位置，可以设置任务完成条件，用户只需点击【完成任务】按钮，就能直接弹出需要填写的表单及字段，既不会遗忘也不会填错。比如，在【完成发货并反馈发货信息】任务设置中，添加【任务完成条件】，必填销售发货表单中的“实际发货时间、物流单号、物流凭证”字段。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-14.png" alt=""><figcaption><p>任务完成条件配置</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-15" alt=""><figcaption><p>完成任务时弹出表单让用户填写</p></figcaption></figure>

当用户勾选完成任务时，则会弹出数据表单让用户填写实际发货时间、物流单号、物流凭证等数据。

<mark style="color:orange;">**功能优化**</mark>

* 首页的审批卡片支持各种快捷操作
* 文件字段现在支持上传 20 个文件
* 工商查询插件支持查重、限制输入的功能
* 业务流程的触发条件支持布局字段

### `v4.31.7` 2024.06.15

<mark style="color:orange;">**工作流增加创建待办任务节点**</mark>

在实际的业务场景下，我们经常会遇到需要多人协作的场景。比如，在发货审批通过后，需要通知出库负责人去线下完成出库并在系统上提交出库信息。发货完成后，需要提醒销售去线下找客户完成验收并在系统上提交验收信息。这时候，我们可以在销售发货单进入某些状态后，给对应的处理人创建待办任务，提醒到应该做什么事。相较于消息通知，待办任务通知更及时，更方便。你可以在「管理后台」中配置：

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-16.png" alt=""><figcaption><p>新建待办任务节点</p></figcaption></figure>

<mark style="color:orange;">**功能优化**</mark>

* 审批流程节点支持修改名称
* 导出数据格式修改为 xlsx
* 优化慢网环境下重复录入的问题
* 子表单中字段支持字段说明
* 富文本中的图片可直接点击预览

### `v4.31.6` 2024.05.30

<mark style="color:orange;">**工作流增加分支节点**</mark>

在复杂业务场景下，为满足一个需求，往往需要配置多个工作流程。为了减少配置的复杂程度，这个版本为工作流增加了分支节点。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-17" alt=""><figcaption><p>工作流程节点分支</p></figcaption></figure>

<mark style="color:orange;">**审批支持飞书消息卡片**</mark>

目前系统中衔接用户完成任务调度主要通过“审批、任务、消息提醒”推送“飞书 / 钉钉 / 系统消息”来完成；引导用户的操作都通过提供查看详情链接，引导用户跳转到侧弹窗由用户主动选择按钮完成操作。但不少用户仍然觉得这种方式比较麻烦，因此，在这个版本中为审批增加了消息卡片的功能。

审批消息卡片：底部展示“通过、驳回、查看详情”三个按钮，点击“通过、驳回”按钮，无需侧弹窗，直接在消息窗口完成操作，点击查看详情在侧弹窗操作“通过、驳回”以及评论等其他操作。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-18.png" alt=""><figcaption><p>审批卡片示例</p></figcaption></figure>

### `v4.31.5` 2024.05.10

<mark style="color:orange;">**创建人与归属人同权**</mark>

一直以来，数据创建人所具有的权限都很低，很多时候，自己创建的数据如果不是归属人的话，连自己都看不到。但很多业务场景又需要创建人和归属人具有同样的权限，比如销售助理帮销售创建的数据，他们两人都要能够查看和编辑，甚至删除。

在这个版本中，你可以在管理后台的「基础数据权限」中进行设置：

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-19.png" alt=""><figcaption><p>基础权限设置</p></figcaption></figure>

<mark style="color:orange;">**优化业务流程锁定**</mark>

* 业务流程锁定规则，包括锁定、解锁、设置白名单用户、白名单字段、流程管理员等所有涉及锁定相关规则，修改发布后即对全部数据生效，无需重置流程
* 业务流程状态流转配置的审批，可以暂时禁用。在导入历史数据时，大量的数据在状态流转时，并不需要在审批了，这时，可以把审批流程暂时禁用，数据处理完成后，再启用

<mark style="color:orange;">**优化工作流“修改数据”节点**</mark>

如果工作流修改的字段是多选用户时，支持选择当前应用的关联应用的不同用户字段，以适应更多的业务场景，具体的配置如下图所示。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-20.png" alt=""><figcaption><p>修改数据时用户列表字段支持设置关联应用用户类型字段</p></figcaption></figure>

### `v4.31.0` 2024.04.07

<mark style="color:orange;">**扩展时间类型字段**</mark>

为时间类型字段增加了「日期格式」属性，支持多个纬度对时间进行编辑和展示，比如当你设置日期格式为“年-季度”时，那么字段在编辑时，仅需要选择季度即可。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-21.png" alt=""><figcaption><p>时间字段配置</p></figcaption></figure>

**优化审批流程**

* 审批详情对数据的创建人、参与人、归属人可见
* 超级管理员可以看到系统的所有审批，并可对审批进行非审批(通过、驳回)外的所有操作
* 审批增加自动催办设置，超过多少时间未审批，自动催办，你可以在审批节点中进行配置

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-22.png" alt=""><figcaption><p>审批时效设置</p></figcaption></figure>

### `v4.30.0` 2024.03.21

<mark style="color:orange;">**数据字段支持校验**</mark>

数字、金额、百分比类型的字段支持“限定数值范围”设置，数值范围不仅支持设置纯数字，也支持设置变量，比如下面的报价单明细中的「数量」应当小于「产品可用库存」。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-23.png" alt=""><figcaption><p>字段校验配置</p></figcaption></figure>

<mark style="color:orange;">**新增周期性日期函数**</mark>

`PERIODICDATE`函数会在每天凌晨更新为当天的日期。当你想要计算某个日期距离今天的天数，配置如下图中的公式即可，然后系统会每天更新计算结果，得到最新的值。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-24.png" alt=""><figcaption><p>PERIODICDATE 函数</p></figcaption></figure>

<mark style="color:orange;">**列表筛选支持在管理后台统一设置**</mark>

应用的筛选组件的字段以及顺序支持在管理后台统一设置了，你可以为公司统一设置每个应用使用哪些字段来过滤。你可以在「应用设置」中的「筛选条件」中进行配置。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-25.png" alt=""><figcaption><p>列表筛选条件配置</p></figcaption></figure>

<mark style="color:orange;">**审批变量支持选择上个抄送节点的人员**</mark>

审批人、抄送人节点的变量均支持“上一抄送节点执行人、上一抄送节点的直属上级、上一审批节点执行人的部门负责人”

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-26.png" alt=""><figcaption><p>审批人设置</p></figcaption></figure>

### `v4.29.0` 2024.03.08

<mark style="color:orange;">**管理后台组织架构设置优化**</mark>

1. “成员”、“管理员”、“部门与成员”三个菜单合并为“组织架构”菜单，结构更清晰，操作更方便

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-27.png" alt=""><figcaption><p>管理后台导航菜单</p></figcaption></figure>

2. 新增离职继承功能

在成员列表中，在“操作”栏点击“移除”按钮后(下图左)，将出现如下图(右)所示的提示框，你可以在这里选择把移除人员的数据交接给谁，点击确认后：

* 移除人员作为归属人的数据，其归属人将修改为交接人
* 移除人员作为参与人的数据，其参与人中的移除人员将替换成交接人
* 移除人员的待审批的流程将全部移交给交接人审批

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-28.png" alt=""><figcaption><p>移除用户</p></figcaption></figure>

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-29.png" alt=""><figcaption><p>移交数据</p></figcaption></figure>

<mark style="color:orange;">**优化工商查询**</mark>

1. 工商查询选中后，能直接提示重复，无需表单提交后再判断

使用工商查询模糊搜索公司信息后，系统会使用你选择的结果进行查重，如果发现有相同的客户，将直接提示。你可以在工商查询中开启此功能。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-30.png" alt=""><figcaption><p>数据查重</p></figcaption></figure>

2. 工商查询字段支持快捷编辑

你现在可以在数据列表页、详情页、编辑页修改工商查询绑定的字段，方便你更规范的填写相关数据。

3. 关联应用字段配置中，如果选了同步删除，则在应用端默认选中从应用且置灰不可修改，以免给用户造成误导，设置和应用界面如下图所示。

### `v4.28.10` 2024.03.02

<mark style="color:orange;">**优化审批流程**</mark>

审批增加了两个新的功能：

1. 审批人去重，即一个人在前序节点中已经审批通过了，后续的节点无需审批，自动通过；
2. 审批时效设置，你可以配置一个审批节点超过一定时限没有人审批，自动通过或自动驳回

想要启用新功能，你可以进入管理后台的审批流程服务中进行相关配置，如下图所示。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-31.png" alt=""><figcaption><p>审批配置</p></figcaption></figure>

<mark style="color:orange;">**优化归属人和参与人字段配置**</mark>

系统为每个应用预设了归属人和参与人两个字段，这两个字段与权限强绑定，不能随便修改。但现实业务场景下，主从对象的数据权限往往是一致的，比如一个人可以编辑订单，那么这个人就应该可以编辑订单明细。由于系统没有权限跟随的功能，之前只能手动将订单明细的归属人或参与人手动修改为主对象的参与人和归属人，很不方便。

这个版本中，我们支持将从对象的参与人和归属人修改为引用字段，引用主对象的参与人和归属人，那么从对象就可以自动跟随主对象的参与人和归属人，保持权限一致。你可以在应用配置中进行相关配置。

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-32.png" alt=""><figcaption><p>归属人配置</p></figcaption></figure>

<mark style="color:orange;">**优化Web端新建页**</mark>

当应用字段过多时，新建应用数据时的页面会显得很凌乱，对此，我们对新建页做了优化，字段支持分栏显示，比如：

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-33.png" alt=""><figcaption><p>新建页分栏展示</p></figcaption></figure>

目前新建页、编辑页、数据转换页均已支持，但整个功能仅支持Web端，移动端正在开发中，后续随其他功能更新。

### `v4.28.0` 2024.02.22

<mark style="color:orange;">**应用映射支持子应用**</mark>

为了让您更好地了解并利用我们的产品，我们在实际应用中建议您使用“从应用”功能代替子表单，这样做有助于以后更轻松地进行数据统计。然而，之前的「映射规则」不支持“从应用”，这导致在许多业务场景下使用起来并不方便。在这个版本中，我们对「映射规则」这个扩展服务进行了升级。现在，您可以在诸如以下场景中使用映射规则，从而更轻松地实现应用之间的相互转换。

* 报价 + 报价明细 → 销售合同 + 订单明细
* 销售订单 + 订单明细 → 服务单 + 服务明细
* 销售订单 + 订单明细 → 开票 + 开票明细

<figure><img src="//swstatic.saleswork.cn/docs/changelog/2023/psa-changelog-2024-34.png" alt=""><figcaption><p>应用映射配置</p></figcaption></figure>

本次更新主要包含：

* 优化映射规则的配置项，不再支持将多个映射规则绑定到一个按钮
* 映射规则增加“从应用映射”配置
* 按钮的权限不再需要到角色权限中配置，这里即为最终的配置

本次更新暂不支持移动端，移动端的相关适配还在开发中，稍后会随其它新功能一起上线。

