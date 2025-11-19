<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Responsive Styled Clock</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:"Poppins",sans-serif}

    body{
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      background:linear-gradient(135deg,#1e3c72,#2a5298);
      padding:20px;
    }

    .clock-container{
      width: min(92vw, 600px);
      /* height removed so container can grow on small screens */
      background: rgba(255,255,255,0.08);
      backdrop-filter: blur(12px);
      border-radius: 20px;
      box-shadow:
        0 8px 32px rgba(0,0,0,0.25),
        0 0 18px rgba(0,212,255,0.15);
      padding: clamp(18px, 3.5vw, 28px);
      display:flex;
      flex-direction:column;
      align-items:center;
      gap: 6px;
      text-align:center;
    }

    /* Large time, but responsive */
    #time{
      font-weight:700;
      letter-spacing:2px;
      text-shadow:
        0 0 10px #00eaff,
        0 0 20px #00eaff;
      color:#00eaff;
      /* responsive font-size */
      font-size: clamp(38px, 9vw, 80px);
      line-height:1;
    }

    /* AM/PM small badge */
    #ampm{
      font-size: clamp(10px, 1.8vw, 16px);
      vertical-align: super;
      margin-left:8px;
      font-weight:600;
      opacity:0.95;
    }

    /* Date style */
    #date{
      margin-top:4px;
      font-size: clamp(12px, 3vw, 18px);
      color: rgba(255,255,255,0.9);
      font-weight:600;
      text-shadow: 0 1px 2px rgba(0,0,0,0.3);
    }

    /* Subtle smaller shadow + radius on very small screens */
    @media (max-width:360px){
      .clock-container{
        border-radius:14px;
        padding:14px;
      }
    }

    /* Optional: darken background a bit on very wide screens */
    @media (min-width:900px){
      body{
        background: linear-gradient(135deg,#11203a,#1b3b6a);
      }
    }

    /* Accessibility focus ring if container is focusable (not required) */
    .clock-container:focus{
      outline: 3px solid rgba(0,234,255,0.14);
      outline-offset: 4px;
    }
  </style>
</head>
<body>

  <div class="clock-container" role="group" aria-label="Clock">
    <div id="time" aria-live="polite">
      <span id="time-text">00:00:00</span>
      <span id="ampm">AM</span>
    </div>
    <div id="date" aria-hidden="false">Loading date...</div>
  </div>

  <script>
    // Helper to add leading zero
    function pad(n){ return n < 10 ? '0' + n : String(n); }

    function updateClock(){
      const d = new Date();

      // Use 12-hour format with AM/PM. If you prefer 24-hour, set use12hour = false
      const use12hour = true;

      let hr = d.getHours();
      const ampm = hr >= 12 ? 'PM' : 'AM';

      if(use12hour){
        hr = hr % 12;
        if(hr === 0) hr = 12; // midnight/noon correction
      }

      const min = d.getMinutes();
      const sec = d.getSeconds();

      const timeText = `${pad(hr)}:${pad(min)}:${pad(sec)}`;
      document.getElementById('time-text').textContent = timeText;
      document.getElementById('ampm').textContent = use12hour ? ampm : '';

      // Date string — localized and compact
      const options = { weekday: 'short', year: 'numeric', month: 'short', day: 'numeric' };
      const dateStr = d.toLocaleDateString(undefined, options); // undefined => user's locale
      document.getElementById('date').textContent = dateStr;
    }

    // Update immediately and then every second. Use setInterval (works fine on mobile)
    updateClock();
    setInterval(updateClock, 1000);

    // If user prefers reduced motion, you could reduce updates — not implemented here
  </script>

</body>
</html>
