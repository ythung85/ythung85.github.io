# Claude Prompt: 高維基因資料的 unified higher-order detection 研究提案版

你是一位統計方法論、RKHS / kernel test、以及高維生物資料分析的專家。  
請幫我撰寫一份「研究提案版」而不是 methods-final 版的研究設計草案。  

## 背景
我正在研究 high-dimensional gene data 的 higher-order detection and modeling，但目前的 screening pipeline 有明顯問題：

- screening 容易被 subnetwork complexity 限制
- ECM / group lasso / SpAM 在樣本數少、訊號弱時，常常連 main effect 都選不出來
- 一旦 main effect 沒被選到，higher-order effect 更難被發現
- 我希望找到一種不依賴 residualization 的方法
- 我希望方法能同時處理 continuous 和 binary response `y`

## 我想要的核心方向
我希望你提出一個以 kernel trick 為核心的統計方法提案，重點是：

- 用 unified RKHS framework 同時處理 continuous / binary `y`
- 透過 algebraic operator 找出 higher-order marginal effect
- 不依賴 residual
- 用統計檢定辨識 higher-order effect，而不是只做 prediction
- 小樣本時也盡量可行
- 最後能輸出哪些 gene sets / modules / hyperedges 形成顯著 interaction

我目前偏好的 conceptual 方向包括：

- partition lattice
- Möbius inversion / inclusion-exclusion 類型的代數消去
- interaction-only kernel operator
- order-specific hypothesis testing
- non-permutation 的 null approximation

## 這份提案版需要完成的內容

請不要直接跳到完整 methods theorem proof。  
請先幫我產出一份「研究提案版」的內容，包含以下部分：

1. **Research motivation**
   - 為什麼現有 screening / sparse modeling 在這個問題上不足
   - 為什麼 kernel-based higher-order detection 是合理方向

2. **Problem statement**
   - 定義我要找的 signal 是什麼
   - 定義 continuous / binary response 共用的研究目標

3. **Proposed conceptual framework**
   - 統一的 kernel representation
   - interaction-only / higher-order-only 的 conceptual decomposition
   - 如何把 main effect、pairwise effect、higher-order effect 放在同一 framework 下

4. **Candidate methodology directions**
   - 請提出 2 到 3 個可行方向
   - 每個方向說明優點、缺點、風險
   - 最後推薦一個最值得做成論文的方法方向

5. **Why this is novel**
   - 相對於 ECM / group lasso / SpAM / standard HSIC / generic dependence tests 的差異
   - novelty 要具體，不要只寫「更好」

6. **What the final paper could claim**
   - 可能的 contribution bullets
   - 可能的 paper title
   - 可能的 abstract skeleton

7. **Open technical questions**
   - 哪些地方現在還不能直接定義好
   - 哪些理論需要之後再補
   - 哪些地方最容易失敗

8. **Next-step experimental plan**
   - 提案階段應該先做哪些 simulation
   - 應該先比較哪些 baselines
   - 應該先驗證哪些 failure modes

## 嚴格要求

- 這份輸出要像「研究提案」而不是完整論文 methods
- 不要假裝所有 theorem 都已經證明
- 如果某些細節還不確定，請明確標註為 open problem 或 assumption
- 不要寫成 residual-based pipeline
- 不要只講直覺，請盡量有統計方法論的語氣
- 不要把方法做成黑盒 prediction model
- 不要假設 response 一定是 continuous；binary 也要共用同一框架

## 我希望你的輸出格式

請用以下結構輸出：

1. Executive summary
2. Motivation and gap
3. Proposed framework
4. Candidate approaches
5. Recommended direction
6. Novelty statement
7. Open problems
8. Experimental plan
9. Draft paper title options
10. Draft abstract skeleton

## 參考脈絡
你可以參考近年的 kernel interaction / high-order testing 脈絡，但請不要照抄：

- Interaction Measures, Partition Lattices and Kernel Tests for High-Order Interactions
  https://arxiv.org/abs/2306.00904
- Permutation-Free High-Order Interaction Tests
  https://arxiv.org/abs/2506.05963
- A fast and accurate kernel-based independence test with applications to high-dimensional and functional data
  https://arxiv.org/abs/2301.00967
- Extending Kernel Testing To General Designs
  https://arxiv.org/abs/2405.13799
- Kernel-based Joint Independence Tests for Multivariate Stationary and Non-stationary Time Series
  https://arxiv.org/abs/2305.08529
- Kernel-based independence tests for causal structure learning on functional data
  https://arxiv.org/abs/2311.08743

## 最後
如果你認為我目前的方向還不夠精準，請先幫我指出最少必要的釐清問題，再繼續寫提案。
