<h1>📋 ABC Lab: Log Entry</h1>

<form id="logForm">
  <label>Date</label>
  <input type="date" name="date" required>

  <label>Participant ID</label>
  <input type="text" name="pid" required>

  <label>RA Name</label>
  <input type="text" name="ra" required>

  <label>Task Order</label>
  <input type="text" name="tasks" placeholder="e.g. th, tm, socialRA" required>

  <label>Bonus</label>
  <input type="text" name="bonus" placeholder="$4 or None">

  <label>Session Notes</label>
  <textarea name="notes" rows="5" placeholder="Adverse events, issues, behavior..."></textarea>

  <button type="submit">Submit Log</button>
</form>

<script>
document.getElementById("logForm").addEventListener("submit", async (e) => {
  e.preventDefault();
  const data = Object.fromEntries(new FormData(e.target));
  const filename = `notebooks/${data.date}_${data.pid}.md`;
  const markdown = `# Session Log – ${data.date} (${data.pid})\n\n**RA**: ${data.ra}  \n**Task Order**: ${data.tasks}  \n**Bonus**: ${data.bonus}\n\n---\n\n## Session Notes\n\n${data.notes}`;
  const encoded = btoa(unescape(encodeURIComponent(markdown)));

  const res = await fetch("https://api.github.com/repos/gugutries/abc-eln/contents/" + filename, {
    method: "PUT",
    headers: {
      Authorization: "Bearer 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",  // 🔒 use Netlify or env-secure backend ideally
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      message: `Add log for ${data.pid} on ${data.date}`,
      content: encoded
    })
  });

  alert(res.ok ? "✅ Log submitted!" : "❌ Submission failed. Check console.");
});
</script>
