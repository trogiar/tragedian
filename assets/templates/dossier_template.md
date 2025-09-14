
<%*
await tp.file.rename("dossier");
-%>

## Items

```dataviewjs
// dossier用：カレントの items/ 配下を自動一覧化し、Summary があれば冒頭数行をプレビュー
// 並び順：作成日（昇順）。新しい順にしたい場合は 'asc' → 'desc' に変更。
const base = dv.current().file.folder + "/items";
const src  = `"${base}"`;
const pages = dv.pages(src);

// 作成日を優先順位に従って取得する関数
function getCreatedDate(page) {
  // 1. file.day（日付形式のファイル名から自動抽出される）
  if (page.file.day) {
    return page.file.day;
  }
  
  // 2. frontmatterの"created_at"プロパティ
  if (page.created_at) {
    return page.created_at;
  }
  
  // 3. file.ctime（ファイルシステムの作成日時）
  return page.file.ctime;
}

// DataView日付オブジェクトを安全にフォーマットする関数
function formatDate(dateValue) {
  if (!dateValue) return "—";
  
  // DataViewの日付オブジェクトの場合、toStringで文字列に変換
  const dateString = dateValue.toString();
  
  // ISO形式の日付文字列から日付部分だけ抽出
  const match = dateString.match(/^(\d{4}-\d{2}-\d{2})/);
  if (match) {
    return match[1];
  }
  
  // フォールバック：momentを使用
  try {
    return moment(dateValue).format("YYYY-MM-DD");
  } catch (e) {
    return dateString.substring(0, 10); // 最初の10文字（YYYY-MM-DD）を取得
  }
}

// Summary セクション抽出（見出し名は "Summary" または "要約" のどちらでも可）
function extractSummary(md) {
  const header = md.match(/^(#{1,6})\s*(Summary|要約)\b.*$/mi);
  if (!header) return "";
  const start = md.indexOf(header[0]) + header[0].length;
  const after = md.slice(start);
  const nextHeader = after.match(/^\s*#{1,6}\s+\S+/m);     // 次の見出しまでを抽出
  const sec = nextHeader ? after.slice(0, nextHeader.index) : after;
  const cleaned = sec.replace(/```[\s\S]*?```/g, "").trim(); // コードフェンス除去
  const lines = cleaned.split(/\r?\n/).map(s => s.trim()).filter(Boolean).slice(0, 3);
  return lines.join("<br>");
}

if (pages.length === 0) {
  dv.paragraph(`(items フォルダが空、または存在しません: ${base})`);
} else {
  const rows = [];
  // 作成日順でソート（昇順）
  const sortedPages = pages.sort(p => getCreatedDate(p), 'asc');
  
  for (const p of sortedPages) {
    const md = await dv.io.load(p.file.path);
    const summary = extractSummary(md);
    const createdDate = getCreatedDate(p);
    const created = formatDate(createdDate);
    rows.push([p.file.link, created, summary || "—"]);
  }
  dv.table(["Note", "Created at", "Summary"], rows);
}
```
