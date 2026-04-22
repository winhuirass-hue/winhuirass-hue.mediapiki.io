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

   
root.querySelectorAll("a[href]").forEach(a => {
  const href = a.getAttribute("href");

  if (href.startsWith("/wiki/")) {
    const t = href.slice(6);
    a.onclick = e => {
      e.preventDefault();
      location.hash = decodeURIComponent(t);
    };
  }

  if (href.startsWith("/w/index.php?title=")) {
    const t = href.split("title=")[1].split("&")[0];
    a.onclick = e => {
      e.preventDefault();
      location.hash = decodeURIComponent(t);
    };
  }
});

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
