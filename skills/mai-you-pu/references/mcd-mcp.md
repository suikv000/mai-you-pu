# 麦当劳 MCP 接入

官方接入指南：https://open.mcd.cn/mcp/doc

2026-10-18 已核对官方指南页面内容：远程地址 `https://mcp.mcd.cn`，传输 Streamable HTTP，鉴权头 `Authorization: Bearer <MCD_MCP_TOKEN>`。凭证从安全环境或会话内存取得，不写进技能、前端、快照或命令参数；不在日志中打印。

实测 initialize 协议版本 2024-11-05 成功，服务名 mcd-mcp / 1.0.0；tools/list 返回35个工具。工具目录可能变化，每次新接入先查目录和 inputSchema。HTTP 200 不代表业务成功，检查 result.isError 及 structuredContent.success/code。

本次成功读取：
- now-time-info：服务器时间。
- list-nutrition-foods：160行常见餐品资料；不等于160种独立商品，更不证明当前在售。无需营养计算时不必调用。
- campaign-calendar：支持 specifiedDate（yyyy-MM-dd）。当日及邻近活动可能混在结果中，保留具体日期。用于新品和回归线索，不能证明门店库存。结果可能带账户订阅状态，公开摘要仅保留白名单字段：活动标题、内容、日期。

后续门店核实：用户给出门店或地点与取餐方式后，检查 query-nearby-stores 参数，获取真实门店编码，再用 query-meals、query-meal-detail 查指定门店、餐段和渠道。不能把参考仓库的示例门店当成用户门店。价格需要时使用 calculate-price 的当前输入契约；字段单位依工具响应判断。

此 Skill 的接入范围是只读食物/活动查询。不要顺带访问历史订单、账户积分、配送地址，也不要调用 create-order、auto-bind-coupons、draw-lottery 等写操作。用户要求点餐时说明这不属于本 Skill 的功能。

认证失败时报告失败并停止重试；429时尊重服务端等待时间，不循环轰炸。接入只在会话调用时发生，不代表已经安装持久 MCP 连接器或设置后台监控。
