# Git 分支策略與環境說明

## 分支流程圖

```
dev    A---B---C---D---E---F---G---H    (開發分支)
           \         \       \
uat         B'--------D'------G'         (測試分支)
                       \       \
staging                 D''-----G''      (預發布分支)
                               \
prod                            G'''     (生產分支)

A,B,C... : 代表各種 commits
B',D',G' : 代表合併後的 commits
```

## 分支策略說明

### 1. Dev 分支 (主要開發分支)
- 所有功能開發的起點
- 包含最多的 commits：
  ```
  feature1: A -> B -> C
  feature2: D -> E
  feature3: F -> G -> H
  ```
- commit 訊息詳細，保留所有開發細節

### 2. UAT 分支 (測試分支)
- 合併自 dev 分支的穩定版本
- commits 經過整理：
  ```
  B' = Squash(A,B,C)   // 第一次功能測試
  D' = Squash(D,E)     // 第二次功能測試
  G' = Squash(F,G,H)   // 第三次功能測試
  ```
- 主要進行功能驗證

### 3. Staging 分支 (預發布分支)
- 來自 UAT 的驗證版本
- commits 更加精簡：
  ```
  D'' = Polish(D')    // 第一次預發布
  G'' = Polish(G')    // 第二次預發布
  ```
- 進行性能測試和最終驗證

### 4. Production 分支 (生產分支)
- 最終穩定版本
- commits 最少且整理過：
  ```
  G''' = Release(G'') // 正式發布版本
  ```
- 每個 commit 都有完整的版本標記

## Commit 數量比較

```
Dev      : *********************  (大量 commits)
UAT      : ********                (較少 commits)
Staging  : ****                    (精簡 commits)
Prod     : **                      (最少 commits)
```

## 合併規則

1. **Dev -> UAT**
   ```
   - 需要完成單元測試
   - 功能完整可測試
   - 代碼審查通過
   ```

2. **UAT -> Staging**
   ```
   - 功能測試通過
   - 使用者驗收完成
   - 合併多個測試修復
   ```

3. **Staging -> Prod**
   ```
   - 性能測試達標
   - 完整測試通過
   - 版本文件準備好
   ```

## 最佳實踐建議

1. **Commit 管理**
   ```
   dev     : 保留詳細 commit
   uat     : 整理功能相關 commit
   staging : 整理成版本 commit
   prod    : 只保留發布 commit
   ```

2. **部署檢查項目**
   ```
   dev     : [ ] 程式碼風格
            [ ] 單元測試
   uat     : [ ] 功能完整性
            [ ] 使用者測試
   staging : [ ] 性能測試
            [ ] 整合測試
   prod    : [ ] 版本確認
            [ ] 部署計劃
   ```

3. **版本標記規則**
   ```
   dev     : feature/xxx
   uat     : test/vX.X.X
   staging : rc/vX.X.X
   prod    : v1.0.0
   ```
