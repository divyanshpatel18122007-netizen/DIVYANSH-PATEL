<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student Attendance Calculator</title>

  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #4f46e5, #7c3aed);
    }

    .container {
      width: 90%;
      max-width: 450px;
      background: white;
      padding: 30px;
      border-radius: 18px;
      box-shadow: 0 15px 40px rgba(0,0,0,0.2);
    }

    h1 {
      text-align: center;
      color: #333;
      margin-bottom: 25px;
    }

    label {
      display: block;
      margin-top: 15px;
      margin-bottom: 7px;
      font-weight: bold;
      color: #444;
    }

    input {
      width: 100%;
      padding: 12px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 13px;
      margin-top: 22px;
      border: none;
      border-radius: 8px;
      background: #4f46e5;
      color: white;
      font-size: 17px;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      background: #4338ca;
    }

    .result {
      margin-top: 25px;
      padding: 18px;
      border-radius: 10px;
      background: #f3f4f6;
      text-align: center;
      display: none;
    }

    .percentage {
      font-size: 32px;
      font-weight: bold;
      margin: 10px 0;
    }

    .good {
      color: #16a34a;
    }

    .warning {
      color: #dc2626;
    }

    .info {
      color: #2563eb;
      margin-top: 10px;
    }
  </style>
</head>

<body>

  <div class="container">
    <h1>📚 Attendance Calculator</h1>

    <label for="total">Total Classes</label>
    <input type="number" id="total" min="0" placeholder="Enter total classes">

    <label for="attended">Classes Attended</label>
    <input type="number" id="attended" min="0" placeholder="Enter attended classes">

    <label for="required">Required Attendance (%)</label>
    <input type="number" id="required" min="0" max="100" value="75">

    <button onclick="calculateAttendance()">
      Calculate Attendance
    </button>

    <div class="result" id="result">
      <div>Your Attendance</div>
      <div class="percentage" id="percentage"></div>
      <div id="status"></div>
      <div class="info" id="extra"></div>
    </div>
  </div>

  <script>
    function calculateAttendance() {
      const total = Number(document.getElementById("total").value);
      const attended = Number(document.getElementById("attended").value);
      const required = Number(document.getElementById("required").value);

      const result = document.getElementById("result");
      const percentage = document.getElementById("percentage");
      const status = document.getElementById("status");
      const extra = document.getElementById("extra");

      if (total <= 0 || attended < 0 || attended > total) {
        alert("Please enter valid class numbers.");
        return;
      }

      if (required < 0 || required > 100) {
        alert("Required attendance must be between 0% and 100%.");
        return;
      }

      const attendance = (attended / total) * 100;

      percentage.textContent = attendance.toFixed(2) + "%";

      if (attendance >= required) {
        percentage.className = "percentage good";
        status.textContent = "✅ Attendance requirement met.";

        // Maximum classes that can be missed while staying above requirement
        const maxMissed = Math.floor(
          total - (required / 100) * total
        );

        extra.textContent =
          "You can miss approximately " + maxMissed +
          " more class(es) based on the current total.";
      } else {
        percentage.className = "percentage warning";
        status.textContent = "⚠️ Attendance requirement not met.";

        // x = additional classes needed
        // (attended + x) / (total + x) >= required/100
        const needed = Math.ceil(
          ((required / 100) * total - attended) /
          (1 - required / 100)
        );

        extra.textContent =
          "You need to attend " + needed +
          " consecutive class(es) to reach " +
          required + "%.";
      }

      result.style.display = "block";
    }
  </script>

</body>
</html>
