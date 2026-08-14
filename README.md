# idp-eer-delivery-parcel — 跨境小包行业交付仓

| 项目 | 内容 |
| --- | --- |
| 定位 | EER 平台在跨境小包行业的**交付侧仓库**：客户档案、租户签字配置、验收实例。与平台仓（idp-eer）**首日隔离**——本仓内容是客户侧商业资产（签字参数即客户风控策略），平台仓的守卫门禁测试机械强制其不得回流 |
| 平台消费 | 平台以发布版被消费，本仓**锁定平台版本**（见下表）；交付人员只需本仓写权限 + 平台发布版，永不需要平台核心代码权限 |
| 方法论 | 全程遵循平台仓 `docs/delivery/00-METHODOLOGY.md` 与编号工具箱（01–07）；行业地图见平台仓 `docs/industries/parcel/` |

## 平台版本锁定

| 项 | 值 |
| --- | --- |
| 平台仓 | github.com/idpxyz/idp-eer |
| 锁定版本 | **`v0.1.0`**（首个发布 tag，2026-08-14；检出 `git fetch --tags && git checkout v0.1.0`） |
| 锁定依据 | 升级纪律：只锁**发布版**——tag 只打在整机验证绿的提交上（平台仓 `docs/operations/release-channel.md`）。v0.1.0 = `131f5d2`：六链 runtime、平台化守卫注册表、对接表单（轮询 L1 / webhook + CDC L2）、链 7 智能面 provider、方法论工具箱 00–09 与链工程技能包；发布门禁 2026-08-14 全绿（宿主门 + `runtime-compose-verify` 端到端 + AF-9 证据） |
| 已知待办 | ~~第一张工程票（发布通道 tag + 镜像 + semver）~~ **已关闭**（v0.1.0 首发，通道纪律见平台仓发布规范）。余项：ghcr 镜像推送待创始人配置凭据——凭据就绪前 tag 是唯一操作性通道，版本化镜像本地构建已实证（`eer-runtime:v0.1.0`） |

## 目录结构

```text
customers/
├── customer-001/         # 首客户真实档案（D0 准备态）：状态页 + D0 启动包
└── sim-demo-001/         # SYNTHETIC-DEMO 模拟演练档案：D0→G2→移交全旅程成品（五件）
config/classification/tenants/
└── sim-demo-001/guards.yaml   # 租户签字守卫实例（SYNTHETIC-DEMO 合成签字）
```

## 运行验证（用平台发布版驱动本仓配置）

在平台仓检出锁定版本后，用本仓的签字实例驱动归类演示：

```bash
cd <idp-eer 检出目录>
go run ./runtime/cmd/eer-classification-demo \
  -guards <本仓路径>/config/classification/tenants/sim-demo-001/guards.yaml \
  -item-name "硅胶手机壳 iPhone15 磁吸" -material silicone \
  -intended-use 手机保护 -declared-value 3.2
# 预期：头部标注本仓实例文件路径；五守卫按签字值评估；落 T1
```

换签字文件即换系统行为，平台代码零改动——这就是"实施=配置"的隔离形态。

## 诚信标注

- `sim-demo-001` 全部内容为 **SYNTHETIC-DEMO 合成演练**（模拟客户 AcmeParcel），示范方法论产出物完成态，禁止用于对外宣称;
- `customer-001` 为真实意向客户档案（代号，未获披露授权），当前 D0 准备态：合同未签、W1 时钟未起算;
- 真实签字件（扫描原件）到达后归档于对应客户目录 `signoff/`，与 yaml 实例互为对照。
