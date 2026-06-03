# auto-delete-weibo
用於全自動批量刪除個人微博。
# 微博自動刪除助手 (瀏覽器書籤極速版) 

這是一個強大、高效且絕對安全的瀏覽器書籤（Bookmarklet）解決方案，專為自動批量刪除個人發佈的新浪微博而設計。

本專案利用瀏覽器原生的 **Bookmarklet** API 運作，能完美繞過新浪微博嚴格的內容安全策略（CSP）阻斷，有效防止腳本被瀏覽器底層攔截或引發網頁卡死。

該專案在chrome瀏覽器中測試通過，其他瀏覽器未知可否使用。

## ✨ 專案特點
* **完全免安裝插件**：純原生瀏覽器書籤觸發，不需要安裝任何第三方擴充功能。
* **強力穿透 CSP 封鎖**：直接繞過微博後端針對自動化腳本的安全性攔截機制。
* **動態文本深度匹配**：直接由網頁底層掃描含有「刪除」或「刪除博文」字樣的節點，不受微博頻繁更改後端 CSS 類名（Class Name）的影響。
* **安全防封隨機延遲**：內建模擬真人操作的隨機冷卻間隔（2-3 秒），大幅降低觸發新浪帳號安全風控（如封號、禁言）的風險。
* **智能例外處理機制**：全自動識別並跳過官方置頂廣告、無權限刪除的轉發、或已被夾的失效微博，絕不在原地卡死。

---

## 🛠️ 安裝與使用指南

### 第一步：製作「一鍵銷毀」魔法書籤
1. 顯示您的chrome瀏覽器書籤列（Windows/Linux 快捷鍵為 `Ctrl + Shift + B`；Mac 快捷鍵為 `Cmd + Shift + B`）。
2. 在書籤列的任意空白處**按右鍵**，選擇 **「新增網頁」**（或「新增書籤」）。
3. 根據以下欄位進行設定：
   * **名稱（Name）**：輸入 `一鍵刪微博`
   * **網址 / 鏈結（URL）**：將下方這**完整的程式碼**全部複製，並黏貼到該輸入框中：

javascript:(async function(){const d=m=>new Promise(r=>setTimeout(r,m));console.log("%c⚡ 极速销毁模式激活...", "color:#ff0055;font-weight:bold;");let c=0,s=0,fail=0;while(fail<3){let b=document.querySelectorAll('i.woo-font--angleDown,[title="更多"],[aria-label="More"]');if(b.length<=s){window.scrollBy({top:600,behavior:'smooth'});await d(1500);b=document.querySelectorAll('i.woo-font--angleDown,[title="更多"],[aria-label="More"]');if(b.length<=s){fail++;continue;}}let a=b[s];try{a.scrollIntoView({block:'center',behavior:'auto'});await d(300);a.click();await d(400);let items=document.querySelectorAll('.woo-pop-item-main,.woo-box-flex,div[class*="pop"],li,span');let del=Array.from(items).find(el=>{let t=el.textContent?el.textContent.trim():'';return t==='删除'||t==='删除博文';});if(del){del.click();await d(400);let dbtns=document.querySelectorAll('.woo-dialog-ctrl button,.woo-button-main,.woo-dialog-btn,button');let conf=Array.from(dbtns).find(el=>{let t=el.textContent?el.textContent.trim():'';return t==='确定'||t==='确认';});if(conf){conf.click();c++;console.log(%60✅ 已删第 ${c} 条%60);await d(2200+Math.random()*1000);continue;}}document.body.click();await d(300);s++;}catch(e){document.body.click();await d(500);s++;}}console.log("🔄 换页重载...");await d(500);location.reload();})();


4. 點擊 **儲存**。此時，您的瀏覽器書籤列上就會多出一個隨點隨用的快捷按鈕。

### 第二步：啟動自動清理引擎
1. 使用電腦瀏覽器登入新浪微博，並進入您的 **「個人主頁」**（例如：`https://weibo.com` 或您的個性域名主頁，點擊您的頭像進入你的“微博”頁面）。
2. 用滑鼠左鍵直接點擊一下剛才建立的 **`🔥 一鍵刪微博`** 書籤。
3. 腳本會立刻在當前視窗中自動聚焦、展開並逐條銷毀微博。當清理完當前頁面的快取後，網頁會**全自動刷新重載**。
4. 如果當新一頁的微博加載出來後，該清理引擎沒有工作，您只需要**再次點擊一下該書籤**，它就會立刻續接火力和進度，繼續清除下一頁。

---

## ⚠️ 核心安全與掛機須知

1. **操作不可逆轉**：批量刪除操作是完全不可逆的。除非您是微博付費會員，否則無法透過 24 小時資源回收筒救回。請在操作前務必確認是否有重要內容需要手動備份。
2. **單日伺服器限額**：新浪微博官方對單一帳號每天調用「刪除 API」設有嚴格的上限（通常每日上限為幾百條到一千條不等）。如果腳本運作一段時間後，發現它自動刷新頁面卻無法成功刪除微博，說明您已達到當日限額。**請立即停止點擊，等待 24 小時後再繼續。**
3. **多視窗隔離掛機（不影響工作）**：如果您需要同時做其他工作，請將這個微博分頁單獨拉成一個**獨立的瀏覽器新視窗**。將其縮小並放置在螢幕邊角（注意：**千萬不能點擊最小化 `-` 號**，否則系統會強行讓網頁 JS 進入休眠模式）。您可以直接用 Word、Excel 或其他工作網頁**在視覺上把它完全擋在後面覆蓋住**，它依然會在後台自動刪除與換页。

## 📄 授權條款
本專案基於 MIT 授權條款開放原始碼。歡迎任何人進行 Fork 分支、優化或二次分發。
