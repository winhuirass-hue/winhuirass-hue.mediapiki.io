<div id="wiki"></div>

<script>
(async () => {
  const title = "C++03"; // змінюй за потреби
  const url =
    "https://uk.wikipedia.org/api/rest_v1/page/html/" +
    encodeURIComponent(title);

  try {
    const res = await fetch(url);
    const html = await res.text();
    document.getElementById("wiki").innerHTML = html;
  } catch {
    document.getElementById("wiki").innerHTML = "";
  }
})();
</script>
