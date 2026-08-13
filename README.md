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
| 锁定版本 | `7cbb885`（2026-08-13；发布通道定形前以 commit 锁定，定形后改用 tag/镜像） |
| 锁定依据 | 升级纪律：只锁**整机验证通过**的基线。本次基线含六链 runtime（分类+清关异常/敏感拦截/换渠道/轨迹停滞/附加费稽核）、平台化守卫注册表、`runtime-compose-verify` 全套绿（7cbb885 实证：含六链配置并集加法门与共享 Actor 裁决） |
| 已知待办 | 平台正式发布通道（tag + 镜像 + semver）尚未定形——本仓第一张工程票 |

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
