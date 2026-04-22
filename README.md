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

    root.querySelectorAll("a[href^='/wiki/']").forEach(a => {
      const t = a.getAttribute("href").replace("/wiki/", "");
      a.onclick = e => {
        e.preventDefault();
        location.hash = decodeURIComponent(t);
      };
    });

    document.title = title;
  } catch {
    document.getElementById("wiki").innerHTML = "";
  }
}

window.addEventListener("hashchange", () => load(page()));
load(page());
</script>
