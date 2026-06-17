# 社区活动奖品层：奖品与发放状态流转图

日期：2026-06-16

## 1. 奖励主状态流转

```mermaid
stateDiagram-v2
  [*] --> Triggered: 活动层触发发奖
  Triggered --> StockHeld: 占用共享库存成功
  Triggered --> Failed: 库存占用失败
  StockHeld --> PendingReview: 待审核
  PendingReview --> ReviewApproved: 审核通过
  PendingReview --> ReviewRejected: 审核拒绝
  PendingReview --> ReviewTimeout: 审核超时
  ReviewRejected --> StockReleased: 释放库存
  ReviewTimeout --> StockReleased: 按规则释放库存
  ReviewApproved --> Issuing: 发放中
  Issuing --> Issued: 发放成功
  Issuing --> IssueFailed: 发放失败
  IssueFailed --> Retrying: 自动重试
  Retrying --> Issued: 重试成功
  Retrying --> ManualRequired: 重试失败
  IssueFailed --> ManualRequired: 进入人工补发
  ManualRequired --> Issued: 人工补发成功
  ManualRequired --> Failed: 人工处理失败
  StockReleased --> [*]
  Issued --> [*]
  Failed --> [*]
```

## 2. 软件码状态流转

```mermaid
stateDiagram-v2
  [*] --> Held: 占用码池库存
  Held --> PendingReview: 待审核
  PendingReview --> Released: 审核拒绝/超时释放
  PendingReview --> AssigningCode: 审核通过分配软件码
  AssigningCode --> CodeIssued: 分配成功
  AssigningCode --> IssueFailed: 分配失败
  IssueFailed --> Retrying: 自动重试
  IssueFailed --> ManualReissue: 人工补发
  Retrying --> CodeIssued
  ManualReissue --> CodeIssued
  CodeIssued --> Viewable: 用户可查看/复制/再次查看
```

## 3. 优惠券状态流转

```mermaid
stateDiagram-v2
  [*] --> Held: 占用券发放额度
  Held --> PendingReview: 待审核
  PendingReview --> Released: 审核拒绝/超时释放
  PendingReview --> Approved: 审核通过
  Approved --> DirectIssuing: 直接到账模式
  Approved --> QualificationCreated: 领取资格模式
  DirectIssuing --> CouponArrived: 已到账
  DirectIssuing --> IssueFailed: 发券失败
  QualificationCreated --> WaitingClaim: 待领取
  WaitingClaim --> CouponArrived: 用户领取成功
  WaitingClaim --> ClaimFailed: 领取失败
  IssueFailed --> Retrying: 自动重试
  ClaimFailed --> Retrying
  Retrying --> CouponArrived
  Retrying --> ManualReissue: 人工补发
  ManualReissue --> CouponArrived
```

## 4. 实物商品状态流转

```mermaid
stateDiagram-v2
  [*] --> Held: 占用商城库存
  Held --> PendingReview: 待审核
  PendingReview --> Released: 审核拒绝/超时释放
  PendingReview --> WaitingAddress: 审核通过，待填写地址
  WaitingAddress --> WaitingShipment: 用户提交地址
  WaitingShipment --> Shipped: 后台标记已发货
  Shipped --> Completed: 后台标记已完成
  WaitingShipment --> Cancelled: 取消发放
  Cancelled --> Released: 释放库存
```

## 5. 状态说明表

| 状态 | 含义 | 用户端是否展示 | 后台是否展示 |
| --- | --- | --- | --- |
| 待审核 | 已占用库存，等待活动层审核/风控结果 | 展示审核中或按活动规则展示 | 展示 |
| 审核拒绝 | 活动层判定不可发放 | 由活动层决定展示 | 展示 |
| 发放中 | 审核通过，系统正在发放权益 | 可展示待发放 | 展示 |
| 发放失败 | 技术性发放失败 | 不直接展示技术失败 | 展示 |
| 自动重试中 | 系统按策略重试发放 | 不展示 | 展示 |
| 待人工补发 | 自动重试未解决或需人工处理 | 不展示技术原因 | 展示 |
| 已到账 | 优惠券或虚拟权益已到账 | 展示 | 展示 |
| 待领取 | 用户有领取资格但未领取 | 展示 | 展示 |
| 待填写地址 | 实物奖品需要用户填写地址 | 展示 | 展示 |
| 待发货 | 用户已填写地址，等待后台发货 | 展示 | 展示 |
| 已发货 | 后台已标记发货 | 展示 | 展示 |
| 已完成 | 奖励流程完成 | 展示 | 展示 |
