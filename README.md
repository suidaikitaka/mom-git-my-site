<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WORKS | 髙橋 三保子</title>
    <style>
        :root { --main-color: #2d3748; --bg-color: #fafafa; }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; background-color: var(--bg-color); color: #333; margin: 0; padding: 40px 20px; line-height: 1.8; letter-spacing: 0.05em; }
        .container { max-width: 700px; margin: 0 auto; background: white; padding: 40px; border-radius: 4px; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
        h1 { font-size: 26px; font-weight: 500; text-align: center; color: var(--main-color); margin-bottom: 40px; letter-spacing: 0.1em; }
        .intro { font-size: 14px; color: #666; text-align: center; margin-bottom: 5px; }
        .work-list { margin-top: 40px; }
        .work-item { padding: 25px 0; border-bottom: 1px solid #eee; display: flex; flex-direction: column; }
        .work-item:last-child { border-bottom: none; }
        .date { font-size: 13px; color: #999; margin-bottom: 8px; font-family: Georgia, serif; }
        .title { font-size: 15px; font-weight: normal; color: #222; text-decoration: none; line-height: 1.6; }
        .title:hover { color: #0066cc; text-decoration: underline; }
        .link-tag { display: inline-block; align-self: flex-start; font-size: 11px; color: #0066cc; border: 1px solid #0066cc; padding: 2px 10px; border-radius: 2px; text-decoration: none; margin-top: 12px; transition: all 0.2s; }
        .link-tag:hover { background: #0066cc; color: white; }
        .admin-btn { display: block; text-align: center; margin-top: 60px; font-size: 11px; color: #ccc; text-decoration: none; }
        #admin-panel { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 1000; }
        .admin-box { background: white; padding: 30px; border-radius: 8px; width: 90%; max-width: 460px; box-shadow: 0 4px 20px rgba(0,0,0,0.15); box-sizing: border-box; }
        .admin-box h2 { margin-top: 0; font-size: 16px; color: #111; margin-bottom: 20px; border-left: 3px solid #0066cc; padding-left: 10px; }
        .admin-box label { display: block; margin-top: 15px; font-size: 13px; font-weight: bold; color: #444; }
        .admin-box input { width: 100%; padding: 10px; margin-top: 6px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box; font-size: 14px; }
        .admin-box input:focus { border-color: #0066cc; outline: none; }
        .btn-group { display: flex; justify-content: flex-end; gap: 10px; margin-top: 25px; }
        .btn { padding: 10px 20px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; font-size: 13px; }
        .btn-save { background: #0066cc; color: white; }
        .btn-close { background: #eee; color: #333; }
    </style>
</head>
<body>

<div class="container">
    <h1>WORKS</h1>
    <p class="intro">ライターとして手がけた仕事をご紹介します。</p>
    <p class="intro" style="margin-bottom: 30px;">取材や執筆だけでなく、企画、構成、編集作業も行います。</p>
    
    <div id="works-container" class="work-list">
        <!-- ここにお母さんが追加した記事が自動で並びます -->
    </div>
    
    <a href="#" class="admin-btn" onclick="openAdmin()">管理画面</a>
</div>

<div id="admin-panel">
    <div class="admin-box">
        <h2>新しい実績の追加</h2>
        <label>① 記事のタイトル・企業名</label>
        <input type="text" id="input-title" placeholder="例：【株式会社スタディスト】〇〇の使命と挑戦">
        
        <label>② 貼りたいURLリンク</label>
        <input type="url" id="input-url" placeholder="https://example.com">
        
        <label>③ パスワード（GitHubトークン）</label>
        <input type="password" id="input-token" placeholder="トークンを入力してください">

        <div class="btn-group">
            <button class="btn btn-close" onclick="closeAdmin()">閉じる</button>
            <button class="btn btn-save" onclick="saveToGitHub()">ホームページを更新する</button>
        </div>
    </div>
</div>

<script>
const REPO = "suidaikitaka/mom-git-my-site"; 
const FILE_PATH = "data.json";

async function loadWorks() {
    try {
        const res = await fetch(`https://githubusercontent.com{REPO}/main/${FILE_PATH}?t=${Date.now()}`);
        if (!res.ok) return;
        const data = await res.json();
        const container = document.getElementById('works-container');
        container.innerHTML = "";
        
        data.reverse().forEach(item => {
            container.innerHTML += `
                <div class="work-item">
                    <span class="date">${item.date}</span>
                    <a href="${item.url}" target="_blank" class="title">${item.title}</a>
                    <a href="${item.url}" target="_blank" class="link-tag">記事を読む ➔</a>
                </div>`;
        });
    } catch (e) { console.error("データ読み込み失敗", e); }
}

function openAdmin() { document.getElementById('admin-panel').style.display = 'flex'; }
function closeAdmin() { document.getElementById('admin-panel').style.display = 'none'; }

async function saveToGitHub() {
    const title = document.getElementById('input-title').value;
    const url = document.getElementById('input-url').value;
    const token = document.getElementById('input-token').value;
    if(!title || !url || !token) return alert("すべての項目を入力してください！");

    const today = new Date();
    const dateStr = `${today.getFullYear()}.${String(today.getMonth()+1).padStart(2,'0')}.${String(today.getDate()).padStart(2,'0')}`;
    
    let currentData = [];
    let sha = null;
    try {
        const res = await fetch(`https://github.com{REPO}/contents/${FILE_PATH}`);
        if(res.ok) {
            const json = await res.json();
            sha = json.sha;
            currentData = JSON.parse(atob(json.content));
        }
    } catch(e){}

    currentData.push({ date: dateStr, title: title, url: url });

    const putRes = await fetch(`https://github.com{REPO}/contents/${FILE_PATH}`, {
        method: "PUT",
        headers: { "Authorization": `token ${token}`, "Content-Type": "application/json" },
        body: JSON.stringify({
            message: "お母さんが記事を追加しました",
            content: btoa(unescape(encodeURIComponent(JSON.stringify(currentData, null, 2)))),
            sha: sha
        })
    });

    if(putRes.ok) {
        alert("ホームページの更新が成功しました！");
        closeAdmin();
        location.reload();
    } else {
        alert("更新に失敗しました。パスワード（トークン）が違う可能性があります。");
    }
}
window.onload = loadWorks;
</script>
</body>
</html>
