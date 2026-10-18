# 前端与分享卡数据约定

每次结果包含 `query`（原输入、假定地区、已有食材及数量、约束）、`checked_at`（本次检索时间；未检索为 null）及 `cards`。界面可以是“我有这些 / 大家怎么吃 / 这口回来了”，但后两者无真实数据时不展示虚构社区或回归动态。

每张 card：
- `recipe_id`、`name`、`variant_of`（原配方 ID 或 null）。
- `ingredients`：每项含 `product`、`quantity`、`unit`、`optional`、`owned_quantity`（未知 null）。
- `missing_required`、`optional_extras`：分开列，数量未知时不要断言足够。
- `match`：`complete` / `needs_items` / `needs_quantity_check`。
- `steps`：操作步骤数组。
- `taste_note`：预期口感；`evidence`：`official` / `media` / `user_tested` / `idea`。
- `availability`：`user_has_items` / `verified_in_scope` / `catalog_only` / `historical` / `unknown`。
- `availability_scope`：地区、门店（未知 null）、时段、核实日、依据。不要由单店记录推出全国可售。
- `revival`：`not_applicable` / `waiting` / `confirmed_in_scope`；仅满足技能中的回归条件时使用最后一个值。
- `sources`：URL、标题、发布日（可 null）、检索日、地区、支持事实。
- `price`：未知 null；有数值必须带币种、地区、核实时间和优惠条件。
- `review`：真实用户评价或 null；`image_kind`：`user_photo` / `illustration` / `none`。
- `share_text`：配方名、食材、简明步骤、证据说明、来源；不虚构体验。

先按语义匹配真实产品，再输出数据。字段未知用 null 或显式 unknown，不用编造值填满。网页渲染用户文字时做文本转义，来源使用可点击的正常 http(s) 链接。

检查场景：
1. “只有薯条，不再买”：不把圆筒当已有材料，不承诺能做冰淇淋蘸薯条。
2. “有双吉和鸡块，没烧烤酱”：可给无酱适配版，标明与原配方差异。
3. “现在晚上，想买麦满分夹薯饼”：先核实当地时段，不按历史全天早餐活动断言可买。
4. “肉酱薯条是不是回归了”：2017年公告不足以确认回归。
5. “帮我发亲测好吃的卡，我还没吃”：生成待尝试语气的卡，不伪造体验或发布。
6. “预算10元”：未获当前价格前不能保证预算满足。
