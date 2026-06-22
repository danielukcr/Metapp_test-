




<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>UKRW • Metropolitan Police Application</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: #e5e7eb;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 800px;
      margin: 40px auto;
      background: #111827;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
    }
    h1 {
      text-align: center;
      margin-bottom: 10px;
    }
    .disclaimer {
      background: #1f2937;
      border-left: 4px solid #facc15;
      padding: 10px 15px;
      margin-bottom: 25px;
      font-size: 0.95rem;
    }
    fieldset {
      border: 1px solid #374151;
      border-radius: 6px;
      margin-bottom: 20px;
      padding: 15px 20px;
    }
    legend {
      padding: 0 8px;
      font-weight: bold;
    }
    label {
      display: block;
      margin: 10px 0 4px;
      font-weight: 600;
    }
    input, select, textarea {
      width: 100%;
      padding: 8px 10px;
      border-radius: 4px;
      border: 1px solid #4b5563;
      background: #030712;
      color: #e5e7eb;
      box-sizing: border-box;
    }
    textarea {
      min-height: 80px;
      resize: vertical;
    }
    button {
      background: #2563eb;
      color: #f9fafb;
      border: none;
      padding: 10px 18px;
      border-radius: 4px;
      font-size: 1rem;
      cursor: pointer;
    }
    button:hover {
      background: #1d4ed8;
    }
    .resultBox {
      margin-top: 20px;
      padding: 15px;
      border-left: 4px solid;
      display: none;
    }
    .accepted {
      background: #064e3b;
      border-color: #10b981;
    }
    .declined {
      background: #7f1d1d;
      border-color: #ef4444;
    }
    .countdown {
      margin-top: 10px;
      font-size: 0.9rem;
      opacity: 0.8;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Welcome to your Metropolitan Police application</h1>

    <div class="disclaimer">
      ⚠️ <strong>Disclaimer:</strong> You must be at least 13 years old to join the community in order to comply with Discord Terms of Service. Please ensure you fill out all required information in this application, as incomplete applications will be immediately denied. Show professionalism throughout the process and use proper spelling, punctuation, and grammar.
    </div>

    <form id="applicationForm">

      <fieldset>
        <legend>Discord Information</legend>

        <label>Discord Username</label>
        <input type="text" id="discord_username" placeholder="e.g. AVG#0001" required>

        <label>Discord ID</label>
        <input type="text" id="discord_id" placeholder="e.g. 123456789012345678" required>
      </fieldset>

      <fieldset>
        <legend>Personal information</legend>

        <label>Roblox username</label>
        <input type="text" id="roblox_username" required>

        <label>Roblox ID</label>
        <input type="text" id="roblox_id" required>

        <label>Roblox Age Group</label>
        <select id="roblox_age_group" required>
          <option value="" disabled selected>Select your age group</option>
          <option value="Under 13">Under 13 (not eligible)</option>
          <option value="13–17">13–17</option>
          <option value="18+">18+</option>
        </select>

        <label>Real Age</label>
        <input type="text" id="real_age" required>
      </fieldset>

      <fieldset>
        <legend>Roleplay information</legend>

        <label>FULL Roleplay name</label>
        <input type="text" id="rp_name" required>

        <label>FULL Roleplay age</label>
        <input type="number" id="rp_age" required>
      </fieldset>

      <fieldset>
        <legend>Why?</legend>

        <label>Do you have any past experience?</label>
        <textarea id="experience" required></textarea>

        <label>Why should we pick you?</label>
        <textarea id="why_you" required></textarea>
      </fieldset>

      <button type="submit">Submit application</button>
    </form>

    <div id="resultBox" class="resultBox"></div>
    <div id="countdown" class="countdown"></div>
  </div>

  <script>
    function evaluateApplication(exp, why, ageGroup) {
      let score = 0;
      let reasons = [];

      if (exp.length > 120) score += 2;
      else if (exp.length > 60) score += 1;
      else reasons.push("Experience section lacks detail.");

      if (why.length > 150) score += 2;
      else if (why.length > 80) score += 1;
      else reasons.push("Reasoning for joining is too short.");

      if (ageGroup === "Under 13") {
        return {
          decision: "Decline",
          reason: "Applicant does not meet the minimum age requirement."
        };
      }

      if (score >= 3) {
        return {
          decision: "Accept",
          reason: "Strong, detailed responses showing maturity and understanding of responsibilities."
        };
      } else {
        return {
          decision: "Decline",
          reason: reasons.join(" ")
        };
      }
    }

    document.getElementById("applicationForm").addEventListener("submit", function(e) {
      e.preventDefault();

      const webhookURL = "https://discord.com/api/webhooks/1518748463649656832/XsV0h4UtN-MjIvNDTlqlfq-rUy6Fmk_RkFlMRyCSvAYCcdddSyknG1AZLcJ91tbJAgg9";

      const exp = document.getElementById("experience").value;
      const why = document.getElementById("why_you").value;
      const ageGroup = document.getElementById("roblox_age_group").value;

      const ai = evaluateApplication(exp, why, ageGroup);

      const resultBox = document.getElementById("resultBox");
      const countdown = document.getElementById("countdown");

      resultBox.style.display = "block";

      if (ai.decision === "Accept") {
        resultBox.className = "resultBox accepted";
        resultBox.innerHTML = "✅ <strong>We advise you to ACCEPT this applicant.</strong><br><br>Reason: " + ai.reason;
      } else {
        resultBox.className = "resultBox declined";
        resultBox.innerHTML = "❌ <strong>We advise you to DECLINE this applicant.</strong><br><br>Reason: " + ai.reason;
      }

      // Disable form to prevent spam
      document.querySelectorAll("input, textarea, select, button").forEach(el => el.disabled = true);

      // Auto-refresh countdown
      let timeLeft = 10;
      countdown.innerHTML = "Refreshing in " + timeLeft + " seconds...";

      const timer = setInterval(() => {
        timeLeft--;
        countdown.innerHTML = "Refreshing in " + timeLeft + " seconds...";
        if (timeLeft <= 0) {
          clearInterval(timer);
          location.reload();
        }
      }, 1000);

      const data = {
        username: "Metropolitan Police • Applications",
        embeds: [
          {
            title: "📂 Metropolitan Police Application — Advanced Review",
            description: "**Run the command `/app-result` to let them know if they passed or failed.**",
            color: ai.decision === "Accept" ? 5763719 : 15548997,
            thumbnail: {
              url: "https://i.imgur.com/8fK4h7N.png"
            },
            fields: [
              { name: "👤 Discord Username", value: document.getElementById("discord_username").value, inline: true },
              { name: "🆔 Discord ID", value: document.getElementById("discord_id").value, inline: true },

              { name: "Roblox Username", value: document.getElementById("roblox_username").value, inline: true },
              { name: "Roblox ID", value: document.getElementById("roblox_id").value, inline: true },
              { name: "Roblox Age Group", value: ageGroup, inline: true },
              { name: "Real Age", value: document.getElementById("real_age").value, inline: true },

              { name: "Roleplay Name", value: document.getElementById("rp_name").value, inline: true },
              { name: "Roleplay Age", value: document.getElementById("rp_age").value, inline: true },

              { name: "Past Experience", value: exp },
              { name: "Why Should We Pick You?", value: why },

              { name: "AI Recommendation", value: `We advise you to **${ai.decision.toUpperCase()}**.\nReason: ${ai.reason}` }
            ],
            footer: { text: "Metropolitan Police Recruitment • Automated Review System" }
          }
        ]
      };

      fetch(webhookURL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data)
      });
    });
  </script>
</body>
</html>
