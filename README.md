# gamedev-skills

ywwwtseng 的遊戲開發 Claude Code plugins。結構與更新方式跟 [ywwwtseng/skills](https://github.com/ywwwtseng/skills) 一樣：一個分類資料夾就是一個 plugin，skill 的呼叫名稱是 `/<plugin>:<skill>`。

## 安裝

在 Claude Code 中：

```
/plugin marketplace add ywwwtseng/gamedev-skills
/plugin install game-ui@gamedev-skills
```

本機開發時可直接用路徑：

```
/plugin marketplace add ~/.claude/skills/gamedev-skills
```

## Plugins

| Plugin | Skill | 呼叫 | 做什麼 |
|---|---|---|---|
| game-ui | classic-ro-game-ui | `/game-ui:classic-ro-game-ui`（描述符合時也會自動載入） | 照早期經典 Ragnarok Online PC 版的視覺語言設計與實作介面：視窗框、按鈕、道具格、狀態／裝備／技能視窗、像素圖示、角色與怪物 sprite 的畫法與逐格動畫，design token 在 `assets/design-tokens.css`。只借視覺語言，不複製官方美術 |

## 結構

```
.
├── .claude-plugin/marketplace.json   # 列出所有 plugin，source 指向分類資料夾
├── game-ui/                          # 遊戲介面：classic-ro-game-ui
└── <plugin>/
    ├── .claude-plugin/plugin.json    # plugin 名稱、版本
    ├── skills/<skill-name>/SKILL.md  # skill 本體
    ├── skills/<skill-name>/references/
    └── skills/<skill-name>/assets/
```

不進 git 的東西（見 `.gitignore`）：

- `**/feedback.md`：`/skills-feedback` 記下的使用者回饋，整理進 SKILL.md 之前只放本機。
- `**/assets/reference-images/` 底下的圖片：有版權的參考截圖（例如官方 RO 的 sprite sheet），這個 repo 是公開的，只留在有圖的那台電腦。SKILL.md 已經把從圖裡觀察到的規則寫成文字，沒有圖也能用。

## 新增 skill

1. 選一個分類 plugin（沒有合適的就新增一個分類資料夾 + `.claude-plugin/plugin.json`，並在 `.claude-plugin/marketplace.json` 的 `plugins` 陣列加入一筆，`source` 寫 `./<plugin>`）
2. 在 `<plugin>/skills/<skill-name>/SKILL.md` 寫 skill
3. 升 `<plugin>/.claude-plugin/plugin.json` 與 `marketplace.json` 的 `version`，執行 `/plugin update <plugin>@gamedev-skills` 即可取得

## 更新 skill 與本地安裝

plugin 依版本號建 cache 目錄（`~/.claude/plugins/cache/gamedev-skills/<plugin>/<version>/`），**版號不變會被視為已安裝而不更新**。

1. 改 `<plugin>/skills/<skill>/` 底下的檔案
2. 升版：`<plugin>/.claude-plugin/plugin.json` 與 `.claude-plugin/marketplace.json` **兩處**的 `version` 都要改
3. commit → `git push origin main`
4. 在 Claude Code：

   ```
   /plugin marketplace update gamedev-skills
   /plugin update <plugin>@gamedev-skills
   ```

5. **重啟 Claude Code**（或開新 session）才會載入新的 SKILL.md
