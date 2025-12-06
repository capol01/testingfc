# testingfc
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Roblox Commands (GitHub)</title>
</head>
<body>
  <h1>Send command to Roblox (via GitHub Issues)</h1>

  <label>GitHub PAT (only paste for sending; do NOT store publicly):</label><br>
  <input id="pat" type="password" style="width:400px"><br><br>

  <label>Repository (owner/repo):</label><br>
  <input id="repo" placeholder="username/reponame" style="width:400px"><br><br>

  <label>Příkaz (např. <code>;kill 12345</code>):</label><br>
  <input id="cmd" style="width:400px"><br><br>

  <button id="send">Odeslat</button>

  <pre id="out"></pre>

<script>
document.getElementById('send').addEventListener('click', async () => {
  const pat = document.getElementById('pat').value.trim();
  const repo = document.getElementById('repo').value.trim();
  const cmd = document.getElementById('cmd').value.trim();
  const out = document.getElementById('out');

  if (!pat || !repo || !cmd) return alert('Vyplň PAT, repo a příkaz.');

  const url = `https://api.github.com/repos/${repo}/issues`;
  const body = {
    title: `command: ${new Date().toISOString()}`,
    body: cmd,
    labels: ['command']
  };

  try {
    const res = await fetch(url, {
      method: 'POST',
      headers: {
        'Authorization': 'token ' + pat,
        'Accept': 'application/vnd.github+json',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(body)
    });
    const j = await res.json();
    if (!res.ok) throw new Error(j.message || 'Chyba');
    out.innerText = 'Vytvořeno issue: ' + j.html_url;
  } catch (e) {
    out.innerText = 'Chyba: ' + e.message;
  }
});
</script>
</body>
</html>
