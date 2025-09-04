<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Purple Theme Image Viewer</title>
  <style>
    :root{
      --bg: #0f0a12;
      --accent1: #8a2be2;
      --accent2: #9400d3;
      --glass: rgba(255,255,255,0.04);
    }
    html,body{height:100%;margin:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial; background: radial-gradient(1200px 600px at 10% 10%, rgba(138,43,226,0.06), transparent), var(--bg); color:#fff}
    .app{
      min-height:100%;display:flex;align-items:center;justify-content:center;padding:28px;box-sizing:border-box;gap:18px;flex-direction:column
    }
    .card{
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      padding:18px;border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,0.6);display:flex;flex-direction:column;align-items:center;gap:12px
    }
    .frame{
      position:relative;width:520px;max-width:88vw;aspect-ratio:1/1;border-radius:12px;overflow:hidden;display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border:1px solid rgba(255,255,255,0.04)
    }
    img.purple{
      width:100%;height:100%;object-fit:cover;display:block;transform-origin:center center;transition:filter 300ms ease, transform 300ms ease;
      filter: contrast(1.02) saturate(1.25) brightness(1.02) hue-rotate(-20deg) drop-shadow(0 20px 40px rgba(64,0,64,0.25));
    }
    /* gradient overlay to tint purple without killing detail */
    .frame::after{
      content:"";position:absolute;inset:0;pointer-events:none;
      background: linear-gradient(135deg, rgba(138,43,226,0.14), rgba(148,0,211,0.12) 40%, rgba(58,0,86,0.12));
      mix-blend-mode: overlay;
    }
    /* soft vignette */
    .frame::before{
      content:"";position:absolute;inset:0;border-radius:inherit;box-shadow:inset 0 40px 80px rgba(0,0,0,0.35), inset 0 -40px 80px rgba(0,0,0,0.22);
    }.controls{display:flex;gap:8px;align-items:center}
button{background:linear-gradient(180deg,var(--accent1),var(--accent2));border:0;padding:10px 14px;border-radius:10px;color:#fff;font-weight:600;cursor:pointer;box-shadow:0 6px 18px rgba(148,0,211,0.12)}
button.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 12px;font-weight:600}
.hint{font-size:13px;opacity:0.85}
@media (max-width:560px){.frame{width:92vw}}

  </style>
</head>
<body>
  <div class="app">
    <div class="card">
      <div class="frame" id="frame">
        <img id="photo" class="purple" alt="Purple themed" src="https://raw.githubusercontent.com/jmiclat24-1168-rgb/7OOP-Repository-Laboratory-and-Projects/d62becdc82f2b499582850bb7dafcb393e43b026/Images/IMG20231212105232.jpg">
      </div><div class="controls">
    <button id="toggleSquare">Toggle 1:1 (Square)</button>
    <button id="download">Download PNG</button>
    <button class="secondary" id="reset">Reset</button>
  </div>
  <div class="hint">This single-file HTML adds a soft purple overlay, hue shift, vignette and a download button that composites the tint into the exported image.</div>
</div>

  </div>  <script>
    const frame = document.getElementById('frame');
    const img = document.getElementById('photo');
    const toggle = document.getElementById('toggleSquare');
    const download = document.getElementById('download');
    const reset = document.getElementById('reset');

    let square = true;
    function setSquare(s){
      if(s){ frame.style.aspectRatio = '1/1'; toggle.textContent = 'Toggle 16:9 (Landscape)'; }
      else{ frame.style.aspectRatio = '16/9'; toggle.textContent = 'Toggle 1:1 (Square)'; }
      square = s;
    }
    setSquare(true);

    toggle.addEventListener('click', ()=> setSquare(!square));
    reset.addEventListener('click', ()=>{
      img.style.filter = 'contrast(1.02) saturate(1.25) brightness(1.02) hue-rotate(-20deg) drop-shadow(0 20px 40px rgba(64,0,64,0.25))';
      frame.style.transform = 'none';
    });

    // draw image to canvas and apply purple overlay so download matches preview
    download.addEventListener('click', async ()=>{
      // create canvas sized to displayed pixel dimensions
      const rect = frame.getBoundingClientRect();
      const w = Math.round(rect.width);
      const h = Math.round(rect.height);
      const canvas = document.createElement('canvas');
      canvas.width = w; canvas.height = h;
      const ctx = canvas.getContext('2d');

      // draw image covering canvas (object-fit:cover)
      const image = new Image();
      image.crossOrigin = 'anonymous';
      image.src = img.src;
      image.onload = () => {
        // calculate cover scaling
        const ratio = Math.max(w / image.width, h / image.height);
        const iw = image.width * ratio;
        const ih = image.height * ratio;
        const ix = (w - iw) / 2;
        const iy = (h - ih) / 2;
        ctx.drawImage(image, ix, iy, iw, ih);

        // apply purple gradient overlay similar to CSS
        const grad = ctx.createLinearGradient(0,0,w,h);
        grad.addColorStop(0, 'rgba(138,43,226,0.14)');
        grad.addColorStop(0.4, 'rgba(148,0,211,0.12)');
        grad.addColorStop(1, 'rgba(58,0,86,0.12)');
        ctx.fillStyle = grad;
        ctx.globalCompositeOperation = 'overlay';
        ctx.fillRect(0,0,w,h);
        ctx.globalCompositeOperation = 'source-over';

        // subtle vignette
        ctx.fillStyle = 'rgba(0,0,0,0.12)';
        ctx.beginPath();
        ctx.ellipse(w/2, h/2, w*0.55, h*0.55, 0, 0, Math.PI*2);
        ctx.fill('evenodd');

        // export
        const url = canvas.toDataURL('image/png');
        const a = document.createElement('a');
        a.href = url; a.download = 'photo-purple.png';
        document.body.appendChild(a); a.click(); a.remove();
      };
      image.onerror = () => alert('Could not load image for export due to cross-origin restrictions. If that happens, save the preview image directly from the browser after right-clicking.');
    });
  </script></body>
</html>

<div style="text-align: center;">
  <img src="https://raw.githubusercontent.com/jmiclat24-1168-rgb/7OOP-Repository-Laboratory-and-Projects/d62becdc82f2b499582850bb7dafcb393e43b026/Images/IMG20231212105232.jpg" 
       alt="Centered 2x2 Image"
       style="width:2in; height:2in; display:block; margin:auto;">
</div>


# 7OOP-Repository-Laboratory-and-Projects
My repository, a collection of my all activities and projects in 7OOP using Python language. All my activities and projects are stored here and you can all see here. I created this repository to collect all of my activities, putting everything together to see and review what I've done. 
# B. ABOUT ME
Hi I am Joana, your optimistic person you know, I love watching k-drama, I am also a family oriented. My favorite food is sinigang and my sport is badminton. And lastly i'm believing that every problem has a solution and every challenge is an opportunity to grow. Because of this, people often see me as someone who can uplift and encourage them.
# C. AREA OF INTEREST
C language and Java language
# D. PROJECT LINKS
# E. FUN FACTS About Me
I'm patience and understanding, i'm a girl you can count on. Lavender is my favorite color
# F. CONTACTS
Fb: https://www.facebook.com/share/15LnUMWCtB/
