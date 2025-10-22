<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>7OOP Repository - Purple Theme</title>
  <style>
    /* 🎨 Theme Colors */
    :root {
      --bg-color: #f8f5fc;       /* light lavender background */
      --accent: #8a2be2;         /* purple accent */
      --text-color: #2d033b;     /* dark purple text */
      --card-bg: #ffffff;        /* card background */
    }

    body {
      font-family: Arial, sans-serif;
      background: var(--bg-color);
      color: var(--text-color);
      margin: 0;
      padding: 20px;
    }

    h1, h2, h3, h4 {
      color: var(--accent);
    }

    /* Center Image */
    .image-container {
      text-align: center;
      margin-bottom: 20px;
    }

    .image-container img {
      width: 2in;
      height: 2in;
      border-radius: 12px;
      border: 3px solid var(--accent);
      box-shadow: 0px 4px 12px rgba(0,0,0,0.2);
    }

    /* Card Style for Sections */
    .section {
      background: var(--card-bg);
      padding: 15px;
      margin: 15px auto;
      border-radius: 12px;
      max-width: 800px;
      box-shadow: 0px 6px 15px rgba(0,0,0,0.1);
    }

    a {
      color: var(--accent);
      text-decoration: none;
      font-weight: bold;
    }

    a:hover {
      text-decoration: underline;
    }
  </style>
</head>
<body>
html_content = """
<div class="image-container" style="text-align:center;">
    <img src="https://raw.githubusercontent.com/jmiclat24-1168-rgb/7OOP-Repository-Laboratory-and-Projects/d62becdc82f2b499582850bb7dafcb393e43b026/Images/IMG20231212105232.jpg"
         alt="Centered 2x2 Image"
         style="width:300px; border-radius:15px;">
</div>
"""

with open("display_image.html", "w") as f:
    f.write(html_content)

print("✅ HTML file created — open 'display_image.html' in your browser to view the photo.")
  
  <div class="section">
    <h1>7OOP-Repository-Laboratory-and-Projects</h1>
    <p>My repository, a collection of my all activities and projects in 7OOP using Python language. All my activities and projects are stored here and you can all see here. I created this repository to collect all of my activities, putting everything together to see and review what I've done.</p>
  </div>

  <div class="section">
    <h2>B. ABOUT ME</h2>
    <p>Hi I am Joana, your optimistic person you know, I love watching k-drama, I am also a family oriented. My favorite food is sinigang and my sport is badminton. And lastly i'm believing that every problem has a solution and every challenge is an opportunity to grow. Because of this, people often see me as someone who can uplift and encourage them.</p>
  </div>

  <div class="section">
    <h2>C. AREA OF INTEREST</h2>
    <p>C language and Java language</p>
  </div>

  <div class="section">
    <h2>D. PROJECT LINKS</h2>
    <p>Coming soon...</p>
  </div>

  <div class="section">
    <h2>E. FUN FACTS About Me</h2>
    <p>I'm patience and understanding, I'm a girl you can count on. Lavender is my favorite color 🌸</p>
  </div>

  <div class="section">
    <h2>F. CONTACTS</h2>
    <p>Fb: <a href="https://www.facebook.com/share/15LnUMWCtB/">My Facebook</a></p>
  </div>

</body>
</html>
