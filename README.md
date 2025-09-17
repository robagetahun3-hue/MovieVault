<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MovieVault - Your Personal Movie Trailer Hub</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box;font-family:Arial,sans-serif;}
  body{background:#121212;color:#fff;line-height:1.6;}
  a{text-decoration:none;color:inherit;}
  header{background:#1f1f1f;padding:20px;position:sticky;top:0;z-index:1000;display:flex;justify-content:space-between;align-items:center;}
  header h1{font-size:2rem;}
  nav a{margin-left:20px;font-weight:bold;transition:0.3s;}
  nav a:hover{color:#f39c12;}
  .hero{display:flex;align-items:center;justify-content:center;height:80vh;background:url('https://images.unsplash.com/photo-1517602302552-471fe67acf66?auto=format&fit=crop&w=1470&q=80') no-repeat center/cover;text-align:center;}
  .hero h2{font-size:3rem;background:rgba(0,0,0,0.5);padding:20px;border-radius:10px;}
  section h2{text-align:center;margin:40px 0 20px;color:#f39c12;}
  .trailers{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:20px;padding:0 20px;}
  .card{background:#1f1f1f;border-radius:10px;overflow:hidden;box-shadow:0 5px 15px rgba(0,0,0,0.5);transition:0.3s;}
  .card img{width:100%;height:250px;object-fit:cover;}
  .card-content{padding:15px;}
  .card-content h3{margin-bottom:10px;}
  .card-content p{font-size:0.9rem;color:#bbb;}
  .card-content button{display:inline-block;margin-top:10px;background:#f39c12;padding:8px 15px;border-radius:5px;color:#121212;font-weight:bold;border:none;cursor:pointer;transition:0.3s;}
  .card-content button:hover{background:#e67e22;}
  .about,.contact,.social-feeds{padding:40px 20px;text-align:center;}
  .about p,.contact p{max-width:800px;margin:20px auto;color:#ccc;}
  .contact form{max-width:500px;margin:0 auto;display:flex;flex-direction:column;}
  .contact input,.contact textarea{margin-bottom:15px;padding:10px;border:none;border-radius:5px;}
  .contact button{background:#f39c12;color:#121212;font-weight:bold;padding:10px;border:none;border-radius:5px;cursor:pointer;transition:0.3s;}
  .contact button:hover{background:#e67e22;}
  .modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.9);justify-content:center;align-items:center;z-index:2000;}
  .modal-content{position:relative;width:80%;max-width:800px;}
  .modal-content iframe{width:100%;height:450px;border:none;}
  .close-btn{position:absolute;top:-15px;right:-15px;background:#f39c12;color:#121212;border:none;font-size:1.5rem;border-radius:50%;width:40px;height:40px;cursor:pointer;}
  footer{background:#1f1f1f;text-align:center;padding:20px;margin-top:40px;font-size:0.9rem;color:#bbb;}
  .social-links{margin:20px 0;}
  .social-links a{margin:0 10px;font-weight:bold;color:#f39c12;transition:0.3s;}
  .social-links a:hover{color:#e67e22;}
  .social-feeds iframe{margin:20px 0;border-radius:10px;}
  html{scroll-behavior:smooth;}
</style>
</head>
<body>

<header>
  <h1>MovieVault</h1>
  <nav>
    <a href="#trailers">Trailers</a>
    <a href="#about">About</a>
    <a href="#social">Social Feeds</a>
    <a href="#contact">Contact</a>
  </nav>
</header>

<section class="hero">
  <h2>Your Hub for the Latest Movie Trailers</h2>
</section>

<section id="trailers">
  <h2>Latest Trailers</h2>
  <div class="trailers" id="trailerGrid"></div>
</section>

<section id="about" class="about">
  <h2>About MovieVault</h2>
  <p>MovieVault is your personal hub for the newest and most exciting movie trailers. We bring you sneak peeks, fan-favorites, and upcoming blockbusters in one place.</p>
</section>

<section id="social" class="social-feeds">
  <h2>Follow Our Social Media</h2>
  <!-- TikTok embed -->
  <iframe src="https://www.tiktok.com/embed/@YOUR_USERNAME" width="325" height="550" allowfullscreen scrolling="no"></iframe>
  <!-- Instagram embed -->
  <iframe src="https://www.instagram.com/p/CODE/embed" width="400" height="480" allowfullscreen scrolling="no"></iframe>
</section>

<section id="contact" class="contact">
  <h2>Contact Us</h2>
  <form id="contactForm">
    <input type="text" placeholder="Your Name" required>
    <input type="email" placeholder="Your Email" required>
    <textarea rows="5" placeholder="Your Message" required></textarea>
    <button type="submit">Send Message</button>
  </form>
  <p id="thankyouMsg" style="color:#f39c12; margin-top:15px;"></p>
  <div class="social-links">
    <a href="https://www.youtube.com/YOUR_CHANNEL" target="_blank">YouTube</a>
    <a href="https://www.tiktok.com/@YOUR_USERNAME" target="_blank">TikTok</a>
    <a href="https://www.instagram.com/YOUR_USERNAME" target="_blank">Instagram</a>
    <a href="https://www.facebook.com/YOUR_PAGE" target="_blank">Facebook</a>
  </div>
</section>

<footer>
  &copy; 2025 MovieVault. All rights reserved.
</footer>

<div class="modal" id="videoModal">
  <div class="modal-content">
    <button class="close-btn" id="closeModal">&times;</button>
    <iframe id="videoFrame" src="" allowfullscreen></iframe>
  </div>
</div>

<script>
// Contact Form
const form = document.getElementById('contactForm');
const msg = document.getElementById('thankyouMsg');
form.addEventListener('submit', e => {
  e.preventDefault();
  msg.textContent = "Thank you! Your message has been sent.";
  form.reset();
});

// Trailer Grid & Modal
const trailerGrid = document.getElementById('trailerGrid');
const modal = document.getElementById('videoModal');
const videoFrame = document.getElementById('videoFrame');
const closeModal = document.getElementById('closeModal');

// Replace with your YouTube API Key and Channel ID
const apiKey = "YOUR_YOUTUBE_API_KEY";
const channelId = "YOUR_CHANNEL_ID";
const maxResults = 6;

async function fetchTrailers() {
  const res = await fetch(`https://www.googleapis.com/youtube/v3/search?key=${apiKey}&channelId=${channelId}&part=snippet,id&order=date&maxResults=${maxResults}`);
  const data = await res.json();
  data.items.forEach(video => {
    if(video.id.kind === "youtube#video"){
      const card = document.createElement('div');
      card.className = 'card';
      card.innerHTML = `
        <img src="${video.snippet.thumbnails.high.url}" alt="${video.snippet.title}">
        <div class="card-content">
          <h3>${video.snippet.title}</h3>
          <p>${video.snippet.description.substring(0,100)}...</p>
          <button data-url="https://www.youtube.com/embed/${video.id.videoId}">Watch Trailer</button>
        </div>
      `;
      trailerGrid.appendChild(card);
    }
  });
}

fetchTrailers();

trailerGrid.addEventListener('click', e => {
  if(e.target.tagName === 'BUTTON'){
    videoFrame.src = e.target.getAttribute('data-url') + "?autoplay=1";
    modal.style.display = "flex";
  }
});

closeModal.addEventListener('click', () => { videoFrame.src = ""; modal.style.display = "none"; });
window.addEventListener('click', e => { if(e.target === modal){ videoFrame.src = ""; modal.style.display = "none"; } });
</script>
</body>
</html>