
<%*
await tp.file.rename("dossier");
-%>

## Items

```dataviewjs
// For dossier: Automatically list items under current items/ folder and preview first few lines if Summary exists
// Sort order: Creation date (ascending). Change 'asc' to 'desc' if you want newest first.
const base = dv.current().file.folder + "/items";
const src  = `"${base}"`;
const pages = dv.pages(src);

// Function to get creation date according to priority order
function getCreatedDate(page) {
  // 1. file.day (automatically extracted from date-format filename)
  if (page.file.day) {
    return page.file.day;
  }
  
  // 2. "created_at" property in frontmatter
  if (page.created_at) {
    return page.created_at;
  }
  
  // 3. file.ctime (filesystem creation time)
  return page.file.ctime;
}

// Function to safely format DataView date objects
function formatDate(dateValue) {
  if (!dateValue) return "—";
  
  // For DataView date objects, convert to string using toString
  const dateString = dateValue.toString();
  
  // Extract only the date part from ISO format date string
  const match = dateString.match(/^(\d{4}-\d{2}-\d{2})/);
  if (match) {
    return match[1];
  }
  
  // Fallback: use moment
  try {
    return moment(dateValue).format("YYYY-MM-DD");
  } catch (e) {
    return dateString.substring(0, 10); // Get first 10 characters (YYYY-MM-DD)
  }
}

// Extract Summary section (header name can be either "Summary" or "要約")
function extractSummary(md) {
  const header = md.match(/^(#{1,6})\s*(Summary|要約)\b.*$/mi);
  if (!header) return "";
  const start = md.indexOf(header[0]) + header[0].length;
  const after = md.slice(start);
  const nextHeader = after.match(/^\s*#{1,6}\s+\S+/m);     // Extract until next header
  const sec = nextHeader ? after.slice(0, nextHeader.index) : after;
  const cleaned = sec.replace(/```[\s\S]*?```/g, "").trim(); // Remove code fences
  const lines = cleaned.split(/\r?\n/).map(s => s.trim()).filter(Boolean).slice(0, 3);
  return lines.join("<br>");
}

if (pages.length === 0) {
  dv.paragraph(`(items folder is empty or does not exist: ${base})`);
} else {
  const rows = [];
  // Sort by creation date (ascending)
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
