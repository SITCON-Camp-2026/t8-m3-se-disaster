# 第一階段 Prompts

## Prompt 1：先分析，不改 code

```text
請先閱讀 docs/student-context.md、docs/output-paths.md、docs/brief.md，以及 src/fixtures/phase-0/messy-reports.json。

目前是第一階段 AI 快速原型衝刺。請先不要修改程式碼，不要設計完整災害系統，不要修改 CommonRecord，也不要把未整理資料移到 shared fixtures。

請分析 src/fixtures/phase-0/messy-reports.json：
1. 每筆原始資訊可能是什麼候選類型？
2. 哪些資料品質較高？原因是什麼？
3. 哪些資料品質不足？缺了什麼？
4. 哪些資訊不能直接變成任務？
5. 哪些地方需要人工確認？
6. 哪些內容是你可能想補完、但原文其實沒有說的？

請不要把分析結果寫進 source code 裡當成 M-001 到 M-012 的標準答案表。這些只是下一步實作工作台時的人類討論材料。

請先只輸出分析與建議，不要修改檔案。
```

## Prompt 2：實作可操作的最小工作台

```text
請根據剛才的分析，幫我們直接實作一個最小可展示、可以操作的第一階段災害資訊整理工作台。

請先閱讀 docs/output-paths.md。可展示成果必須能從 GitHub Pages 首頁看到或操作。

限制：
- 只處理 src/fixtures/phase-0/messy-reports.json
- 不新增後端
- 不使用 localStorage
- 不呼叫外部 API
- 不使用 runtime LLM
- 不修改 CommonRecord
- 不把未整理資料放進 src/fixtures/shared
- 不把 needs_review / unverified 顯示成 verified / confirmed
- 不要 hardcode 每筆 M-ID 的完整整理答案
- 不要把剛才的分析結果寫回 fixture

目標：
1. 顯示所有原始資訊列表
2. 每筆顯示 rawText、sourceType、verificationStatus、updatedAt
3. 讓使用者可以從選中的原始資訊建立整理草稿
4. 顯示已建立的整理草稿列表與目前進度
5. 整理草稿至少要能記錄：這筆資訊目前可能被理解成什麼、小組有多確定、判斷依據來自原文哪裡、目前卡住或不能直接使用的原因、下一步應該補問 / 保留 / 人工確認 / 暫時不要使用、是否不應該讓志工直接採取行動
6. 讓使用者可以記錄至少一段人類質疑或修正
7. 讓使用者可以刪除或重設整理草稿
8. 明顯標示 needs_review / unverified
9. 至少讓 6 筆資料能被嘗試整理成可編輯草稿
10. 成果必須接進 src/app/App.tsx 或它匯入的 component

如果已有 Phase 0 component，請優先擴充或重用既有 component，不要新增平行且未被首頁引用的版本。6 筆可編輯草稿可以先用保守安全預設或空白欄位建立，讓人類在 UI 中編輯；不要把 Prompt 1 的逐筆分析結果、候選分類或 M-ID 標準答案預填進 source code 或 fixture。

`sourceType` 只代表資料從哪裡來，不代表已被查核；即使來源看起來像官方公告，也要看 `verificationStatus` 與原文是否說明清楚。

請用 React state 實作前端-only CRUD，不要使用 localStorage。可以參考 src/features/phase-0/phase0-types.ts 的 Phase0JudgementDraft，但不需要把 UI 做成完全照欄位名稱排列的表單；如果需要人類修正欄位，可使用 humanReviewNote。

請先用不超過 5 行說明你要改哪些檔案，然後直接實作。完成後請說明如何操作：建立、查看、編輯、刪除或重設一筆整理草稿。
```

## Prompt 3：請 agent 自我審查

```text
請根據目前的第一階段整理工作台做自我審查。

請檢查：
1. 哪些整理結果可能是你根據常識補出來的，而不是原文有寫？
2. 哪些資料不能直接變成任務？
3. 哪些資料需要人工確認？
4. UI 哪些地方可能讓使用者誤會資料已經確認？
5. 如果真的用在災害現場，最危險的誤導是什麼？
6. 是否不小心把每筆 M-ID 的分析寫成 source code 裡的標準答案？

請把結果整理到 docs/phase0-observations.md，並更新 docs/ai-log.md。不要把觀察寫成已確認事實。
```
