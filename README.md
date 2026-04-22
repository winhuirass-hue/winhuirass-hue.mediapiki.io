<head><title>Вікіпедія</title></head>
<div id="wiki"></div>
<script>
const API = "https://uk.wikipedia.org/api/rest_v1/page/html/";
function page() {
  return decodeURIComponent(location.hash.slice(1)) || "Головна_сторінка";
}
async function load(title) {
  try {
    const res = await fetch(API + encodeURIComponent(title));
    const html = await res.text();
    const root = document.getElementById("wiki");
    root.innerHTML = html;
    root.querySelector("base")?.remove();
    root.querySelectorAll("a[href]").forEach(a => {
      const href = a.getAttribute("href");
      if (!href) return;
      if (href.startsWith("https://uk.wikipedia.org/wiki/")) {
        const t = href.split("/wiki/")[1];
        a.onclick = e => { e.preventDefault(); location.hash = decodeURIComponent(t); };
        return;
      }
      if (href.startsWith("/wiki/")) {
        const t = href.slice(6);
        a.onclick = e => { e.preventDefault(); location.hash = decodeURIComponent(t); };
        return;
      }
      if (href.startsWith("/w/index.php?title=")) {
        const t = href.split("title=")[1].split("&")[0];
        a.onclick = e => { e.preventDefault(); location.hash = decodeURIComponent(t); };
      }
    });
    document.title = title;
  } catch {
    document.getElementById("wiki").innerHTML = "";
  }
}
window.addEventListener("hashchange", () => load(page()));
load(page());
</script>
<style>
body { margin: 0; }
html, body { height: 100%; }
footer, .site-footer, .github-footer { display: none !important; }
</style>
