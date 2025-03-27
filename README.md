<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Conecta</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" rel="stylesheet">
  <style>
    :root {
      --primary: #ff4d8c;
      --secondary: #7e57ff;
      --gradient: linear-gradient(135deg, #ff6b6b, #fc5c7d, #e86af0);
    }
    * {margin: 0; padding: 0; box-sizing: border-box; font-family: -apple-system, sans-serif;}
    body {background: #f5f5f5;}
    .container {max-width: 420px; margin: 0 auto; background: white; min-height: 100vh; position: relative; overflow: hidden;}
    
    /* Fondo animado */
    .animated-bg {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      overflow: hidden;
    }
    
    .bg-shape {
      position: absolute;
      border-radius: 50%;
      animation: float 15s infinite;
      opacity: 0.4;
    }
    
    .bg-shape:nth-child(1) {
      width: 300px;
      height: 300px;
      background: #ff6b6b;
      top: -100px;
      left: -100px;
      animation-delay: 0s;
    }
    
    .bg-shape:nth-child(2) {
      width: 200px;
      height: 200px;
      background: #fc5c7d;
      top: 50%;
      right: -80px;
      animation-delay: 3s;
    }
    
    .bg-shape:nth-child(3) {
      width: 150px;
      height: 150px;
      background: #e86af0;
      bottom: -50px;
      left: 20%;
      animation-delay: 7s;
    }
    
    .bg-shape:nth-child(4) {
      width: 120px;
      height: 120px;
      background: #7e57ff;
      top: 20%;
      left: -40px;
      animation-delay: 5s;
    }
    
    /* Pantallas */
    #intro-screen, #main-screen, #match-screen, #chat-screen, #profile-creation {display: none;}
    .active {display: block !important;}
    
    /* Animaciones */
    @keyframes pulse {
      0% {transform: scale(1);}
      50% {transform: scale(1.05);}
      100% {transform: scale(1);}
    }
    
    @keyframes float {
      0% {transform: translateY(0) rotate(0deg);}
      50% {transform: translateY(-20px) rotate(5deg);}
      100% {transform: translateY(0) rotate(0deg);}
    }
    
    @keyframes fadeIn {
      from {opacity: 0; transform: translateY(20px);}
      to {opacity: 1; transform: translateY(0);}
    }
    
    .pulse {animation: pulse 1.5s infinite;}
    .fade-in {animation: fadeIn 0.5s forwards;}
    
    /* Estilos comunes */
    .bg-gradient {background: var(--gradient);}
    .text-primary {color: var(--primary);}
    .btn {
      display: block; width: 85%; margin: 0.7rem auto; padding: 0.8rem;
      border-radius: 50px; text-align: center; font-weight: bold; cursor: pointer;
      border: none; transition: all 0.3s;
    }
    .btn-primary {background: white; color: var(--primary);}
    .btn-outline {background: transparent; border: 1px solid white; color: white;}
    .btn:hover {transform: translateY(-3px); box-shadow: 0 5px 15px rgba(0,0,0,0.1);}
    
    /* Header */
    .header {
      display: flex; justify-content: space-between; align-items: center; 
      padding: 1rem; border-bottom: 1px solid #eee;
    }
    
    /* Pantalla de inicio */
    .intro-container {
      display: flex; flex-direction: column; align-items: center; 
      justify-content: center; min-height: 100vh; padding: 1rem;
      color: white; text-align: center; position: relative;
    }
    .logo {font-size: 3rem; font-weight: bold; margin: 1rem 0;}
    
    .card-demo {position: relative; height: 320px; width: 100%; margin: 1rem 0;}
    .profile-card-demo {
      position: absolute; width: 160px; height: 220px; background: white;
      border-radius: 12px; box-shadow: 0 5px 15px rgba(0,0,0,0.2); overflow: hidden;
      transition: transform 0.3s ease;
    }
    .profile-card-demo:hover {transform: translateY(-5px);}
    .profile-card-demo img {width: 100%; height: 120px; object-fit: cover;}
    .profile-card-demo:nth-child(1) {top: 0; left: 30px; transform: rotate(-5deg);}
    .profile-card-demo:nth-child(2) {top: 20px; right: 20px; transform: rotate(5deg); z-index: 2;}
    .profile-card-demo:nth-child(3) {bottom: 0; left: 80px; z-index: 3;}
    .profile-card-demo:nth-child(1):hover {transform: rotate(-5deg) translateY(-5px);}
    .profile-card-demo:nth-child(2):hover {transform: rotate(5deg) translateY(-5px);}
    
    .card-info {padding: 10px;}
    .card-info h3 {font-size: 15px; margin-bottom: 3px;}
    .card-info p {font-size: 12px; color: #666;}
    
    .stats {display: flex; justify-content: space-around; margin: 1rem 0; color: white;}
    .stat {text-align: center;}
    .stat-num {font-size: 1.5rem; font-weight: bold;}
    .stat-text {font-size: 0.7rem;}
    
    .testimonial {
      background: rgba(255,255,255,0.2); padding: 1rem; border-radius: 10px;
      margin: 1rem auto; width: 85%; font-size: 0.9rem; backdrop-filter: blur(5px);
    }
    .testimonial-text {font-style: italic; margin-bottom: 0.5rem;}
    .testimonial-author {display: flex; align-items: center;}
    .author-pics {display: flex; margin-right: 0.5rem;}
    .author-pics img {width: 20px; height: 20px; border-radius: 50%; border: 1px solid white;}
    .author-pics img:nth-child(2) {margin-left: -10px;}
    
    .footer-text {text-align: center; color: white; opacity: 0.7; font-size: 0.7rem; margin: 1rem 0;}
    
    /* Pantalla principal */
    .profile-card {
      border-radius: 15px; margin: 1rem; box-shadow: 0 4px 15px rgba(0,0,0,0.1);
      overflow: hidden; position: relative;
      transition: transform 0.4s ease, opacity 0.4s ease;
    }
    .profile-card img {width: 100%; height: 350px; object-fit: cover;}
    
    .swipe-left {
      transform: translateX(-150%) rotate(-30deg);
      opacity: 0;
    }
    
    .swipe-right {
      transform: translateX(150%) rotate(30deg);
      opacity: 0;
    }
    
    .badge {
      position: absolute; border-radius: 15px; font-size: 12px;
      font-weight: bold; padding: 3px 8px; z-index: 2;
    }
    .badge-premium {top: 10px; right: 10px; background: gold; color: #8B4513;}
    .badge-verified {top: 10px; left: 10px; background: #4299e1; color: white;}
    
    .photo-nav {
      position: absolute; top: 175px; width: 30px; height: 30px;
      background: rgba(255,255,255,0.7); border-radius: 50%; display: flex;
      align-items: center; justify-content: center; cursor: pointer;
      transition: background 0.3s;
    }
    .photo-nav:hover {background: rgba(255,255,255,0.9);}
    .prev-photo {left: 10px;}
    .next-photo {right: 10px;}
    
    .profile-info {padding: 15px;}
    .profile-name {font-size: 1.5rem; font-weight: bold; margin-right: 5px;}
    .profile-age {font-size: 1.2rem; color: #666;}
    
    .online-status {
      display: flex; align-items: center; font-size: 12px; color: #48bb78;
      margin-top: 5px;
    }
    .online-dot {
      width: 8px; height: 8px; border-radius: 50%; 
      background: #48bb78; margin-right: 3px;
    }
    
    .location {display: flex; align-items: center; font-size: 12px; color: #666; margin: 5px 0 10px;}
    
    .profile-bio {margin-bottom: 15px; font-size: 14px; color: #333;}
    
    .prompt-box {
      background: #f8f8f8; padding: 10px; border-radius: 10px; 
      margin-bottom: 15px; transition: all 0.3s;
    }
    .prompt-box:hover {background: #f0f0f0;}
    .prompt-question {font-size: 13px; font-weight: bold; color: #333; margin-bottom: 3px;}
    .prompt-answer {font-size: 14px; color: #666;}
    .prompt-nav {display: flex; justify-content: center; margin-top: 8px;}
    .prompt-nav button {
      background: none; border: none; color: #999; 
      cursor: pointer; padding: 5px 10px;
    }
    .prompt-nav button:hover {color: var(--primary);}
    
    .interests-label {font-size: 13px; font-weight: bold; color: #333; margin-bottom: 8px;}
    .interests {display: flex; flex-wrap: wrap; gap: 5px; margin-top: 10px;}
    .interest {
      background: #ffedf2; color: var(--primary); border-radius: 20px;
      padding: 5px 10px; font-size: 12px; transition: all 0.3s;
    }
    .interest:hover {background: var(--primary); color: white;}
    
    .ai-suggestion {
      background: white; padding: 12px; border-radius: 10px; 
      box-shadow: 0 2px 8px rgba(0,0,0,0.1); margin: 15px 0;
      transition: all 0.3s;
    }
    .ai-suggestion:hover {transform: translateY(-3px); box-shadow: 0 5px 15px rgba(0,0,0,0.1);}
    .ai-header {display: flex; align-items: center; margin-bottom: 8px;}
    .ai-icon {
      width: 30px; height: 30px; background: #e6f7ff; color: #1890ff; 
      border-radius: 50%; display: flex; align-items: center; 
      justify-content: center; margin-right: 10px;
    }
    .ai-title {font-weight: bold; font-size: 14px;}
    .ai-subtitle {font-size: 10px; color: #999;}
    .ai-message {
      background: #f0f7ff; padding: 10px; border-radius: 8px; 
      font-size: 13px; font-style: italic;
    }
    
    .action-buttons {
      display: flex; justify-content: center; gap: 15px;
      margin: 20px 0; padding-bottom: 60px;
    }
    .action-btn {
      width: 60px; height: 60px; border-radius: 50%; display: flex;
      align-items: center; justify-content: center; cursor: pointer;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1); font-size: 1.5rem;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    .action-btn:hover {transform: scale(1.1); box-shadow: 0 5px 15px rgba(0,0,0,0.15);}
    .dislike-btn {background: white; color: var(--primary); border: 1px solid #eee;}
    .superlike-btn {
      background: white; color: #38b2f8; border: 1px solid #eee;
      font-size: 1.2rem; width: 50px; height: 50px;
    }
    .like-btn {background: white; color: #48bb78; border: 1px solid #eee;}
    
    /* Barra de navegación inferior */
    .bottom-nav {
      display: flex; justify-content: space-around; padding: 10px;
      background: white; border-top: 1px solid #eee;
      position: fixed; bottom: 0; width: 100%; max-width: 420px;
      box-shadow: 0 -2px 10px rgba(0,0,0,0.05);
    }
    .nav-item {
      display: flex; flex-direction: column; align-items: center;
      color: #999; font-size: 10px; cursor: pointer;
      transition: all 0.3s;
    }
    .nav-item:hover {color: var(--primary);}
    .nav-item.active {color: var(--primary);}
    .nav-icon {font-size: 20px; margin-bottom: 3px;}
    
    /* Match screen */
    .match-container {
      display: flex; flex-direction: column; align-items: center;
      justify-content: center; min-height: 100vh; padding: 2rem;
      color: white; text-align: center; position: relative;
    }
    .match-title {font-size: 2.5rem; font-weight: bold; margin-bottom: 1.5rem;}
    .match-photos {display: flex; align-items: center; justify-content: center; margin-bottom: 2rem;}
    .match-photo {
      width: 120px; height: 120px; border-radius: 50%;
      border: 3px solid white; overflow: hidden;
    }
    .match-photo img {width: 100%; height: 100%; object-fit: cover;}
    .match-heart {font-size: 2rem; margin: 0 1rem; animation: pulse 1.5s infinite;}
    
    /* Chat screen */
    .chat-header {
      display: flex; align-items: center; padding: 1rem;
      border-bottom: 1px solid #eee; background: white;
      position: sticky; top: 0; z-index: 10;
    }
    .back-btn {
      margin-right: 1rem; font-size: 1.2rem; color: #666;
      cursor: pointer; transition: color 0.3s;
    }
    .back-btn:hover {color: var(--primary);}
    .chat-photo {
      width: 40px; height: 40px; border-radius: 50%;
      overflow: hidden; margin-right: 0.8rem; position: relative;
    }
    .chat-photo img {width: 100%; height: 100%; object-fit: cover;}
    .chat-online-dot {
      position: absolute; bottom: 0; right: 0; width: 12px; height: 12px;
      background: #48bb78; border-radius: 50%; border: 2px solid white;
    }
    
    .chat-messages {padding: 1rem; height: calc(100vh - 130px); overflow-y: auto; display: flex; flex-direction: column;}
    .message {margin-bottom: 1rem; max-width: 75%;}
    .message-received {
      align-self: flex-start; background: #f1f1f1; color: #333;
      border-radius: 15px; border-bottom-left-radius: 5px; padding: 0.8rem;
    }
    .message-sent {
      align-self: flex-end; background: var(--primary); color: white;
      border-radius: 15px; border-bottom-right-radius: 5px; padding: 0.8rem;
      margin-left: auto;
    }
    .message-time {font-size: 0.7rem; opacity: 0.7; text-align: right; margin-top: 0.3rem;}
    
    .chat-input {
      display: flex; padding: 0.8rem; border-top: 1px solid #eee;
      position: fixed; bottom: 0; width: 100%; max-width: 420px; background: white;
      align-items: center;
    }
    .chat-input input {
      flex: 1; border: 1px solid #ddd; border-radius: 20px;
      padding: 0.8rem; margin: 0 0.5rem; transition: border 0.3s;
    }
    .chat-input input:focus {
      outline: none; border-color: var(--primary);
    }
    .chat-input button {
      width: 40px; height: 40px; border-radius: 50%; 
      background: var(--primary); color: white; border: none; 
      display: flex; align-items: center; justify-content: center;
      cursor: pointer; transition: all 0.3s;
    }
    .chat-input button:hover {
      background: #ff366e; transform: scale(1.05);
    }
    
    /* Formulario de perfil */
    .progress-container {
      width: 100%; background-color: #f0f0f0; border-radius: 10px;
      position: relative; height: 8px; overflow: hidden;
    }
    
    .progress-bar {
      position: absolute; top: 0; left: 0; height: 100%; 
      background: var(--gradient); transition: width 0.5s ease-in-out;
    }
    
    .profile-form {padding: 1rem;}
    .form-header {margin-bottom: 1.5rem;}
    .form-title {font-size: 1.5rem; font-weight: bold; margin-bottom: 0.5rem;}
    .form-subtitle {color: #666; font-size: 0.9rem;}
    
    .form-group {margin-bottom: 1rem;}
    .form-label {font-weight: bold; margin-bottom: 0.5rem; display: block;}
    .form-control {
      width: 100%; padding: 0.8rem; border: 1px solid #ddd;
      border-radius: 8px; transition: border 0.3s;
    }
    .form-control:focus {
      outline: none; border-color: var(--primary);
    }
    
    .form-buttons {display: flex; margin-top: 2rem; gap: 1rem;}
    .form-btn-back {
      flex: 1; background: #f1f1f1; color: #333; padding: 0.8rem;
      border: none; border-radius: 5px; cursor: pointer;
      transition: all 0.3s;
    }
    .form-btn-back:hover {background: #e5e5e5;}
    .form-btn-continue {
      flex: 2; background: var(--gradient); color: white; padding: 0.8rem;
      border: none; border-radius: 5px; cursor: pointer;
      transition: all 0.3s;
    }
    .form-btn-continue:hover {transform: translateY(-3px); box-shadow: 0 5px 15px rgba(0,0,0,0.1);}
    
    /* Pasos del formulario */
    #step1, #step2, #step3, #step4, #completion {display: none;}
    #step1.active, #step2.active, #step3.active, #step4.active, #completion.active {display: block;}
    
    /* Subida de fotos */
    .photo-upload-container {
      display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px;
      margin-bottom: 1rem;
    }
    
    .photo-upload {
      aspect-ratio: 1; border: 2px dashed #cbd5e0; border-radius: 10px;
      display: flex; align-items: center; justify-content: center;
      color: #a0aec0; cursor: pointer; transition: all 0.3s ease;
      position: relative; overflow: hidden;
    }
    
    .photo-upload:hover {
      border-color: var(--primary); color: var(--primary);
    }
    
    .photo-upload.has-image {border: none;}
    
    .photo-upload img {
      width: 100%; height: 100%; object-fit: cover; border-radius: 8px;
    }
    
    /* Intereses */
    .interest-checkbox {display: none;}
    
    .interest-label {
      display: inline-block; padding: 8px 16px; margin: 5px;
      background-color: #f0f0f0; border-radius: 50px;
      color: #4a5568; cursor: pointer; transition: all 0.3s ease;
    }
    
    .interest-checkbox:checked + .interest-label {
      background: var(--gradient); color: white;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Fondo animado -->
    <div class="animated-bg">
      <div class="bg-shape"></div>
      <div class="bg-shape"></div>
      <div class="bg-shape"></div>
      <div class="bg-shape"></div>
    </div>
    
    <!-- Pantalla de inicio -->
    <section id="intro-screen" class="bg-gradient active">
      <div class="intro-container">
        <div class="logo pulse">Conecta</div>
        <p>Donde las conexiones reales suceden</p>
        
        <div class="card-demo">
          <div class="profile-card-demo">
            <img src="https://randomuser.me/api/portraits/women/44.jpg" alt="Laura">
            <div class="card-info">
              <h3>Laura, 27</h3>
              <p>Fotógrafa</p>
            </div>
          </div>
          <div class="profile-card-demo">
            <img src="https://randomuser.me/api/portraits/men/32.jpg" alt="Carlos">
            <div class="card-info">
              <h3>Carlos, 30</h3>
              <p>Músico</p>
            </div>
          </div>
          <div class="profile-card-demo">
            <img src="https://randomuser.me/api/portraits/women/66.jpg" alt="Sofia">
            <div class="card-info">
              <h3>Sofia, 25</h3>
              <p>Diseñadora</p>
            </div>
          </div>
        </div>
        
        <div class="stats">
          <div class="stat">
            <div class="stat-num">2M+</div>
            <div class="stat-text">Usuarios</div>
          </div>
          <div class="stat">
            <div class="stat-num">100K</div>
            <div class="stat-text">Matches/día</div>
          </div>
          <div class="stat">
            <div class="stat-num">95%</div>
            <div class="stat-text">Satisfacción</div>
          </div>
        </div>
        
        <button class="btn btn-primary" id="startBtn">Comenzar</button>
        <button class="btn btn-outline" id="loginBtn">Ya tengo cuenta</button>
        
        <div class="testimonial">
          <div class="testimonial-text">"Conecta nos ayudó a encontrarnos. Después de nuestro primer match, ya sabíamos que era algo especial."</div>
          <div class="testimonial-author">
            <div class="author-pics">
              <img src="https://randomuser.me/api/portraits/women/67.jpg" alt="María">
              <img src="https://randomuser.me/api/portraits/men/67.jpg" alt="Javier">
            </div>
            <div class="author-name">María & Javier, juntos 1 año</div>
          </div>
        </div>
        
        <p class="footer-text">Al registrarte, aceptas nuestros Términos y Política de Privacidad</p>
      </div>
    </section>

    <!-- Pantalla principal -->
    <section id="main-screen" class="hidden">
      <div class="header">
        <div class="text-primary" style="font-size: 1.5rem; font-weight: bold;">Conecta</div>
        <div>
          <i class="fas fa-sliders-h" style="margin-right: 15px; cursor: pointer;"></i>
          <i class="fas fa-bars" style="cursor: pointer;"></i>
        </div>
      </div>
      
      <div style="padding-bottom: 60px;">
        <div style="display: flex; justify-content: space-between; padding: 15px;">
          <div style="font-size: 18px; font-weight: bold;">Descubrir</div>
          <div>
            <i class="fas fa-filter" style="color: #666; cursor: pointer;"></i>
          </div>
        </div>
        
        <div class="profile-card">
          <div class="badge badge-premium"><i class="fas fa-crown"></i> Premium</div>
          <div class="badge badge-verified"><i class="fas fa-check-circle"></i> Verificado</div>
          
          <img src="https://randomuser.me/api/portraits/women/23.jpg" id="profileImage" alt="Elena">
          
          <div class="photo-nav prev-photo" id="prevPhotoBtn"><i class="fas fa-chevron-left"></i></div>
          <div class="photo-nav next-photo" id="nextPhotoBtn"><i class="fas fa-chevron-right"></i></div>
          
          <div class="profile-info">
            <div>
              <div style="display: flex; align-items: baseline;">
                <div class="profile-name">Elena</div>
                <div class="profile-age">28</div>
              </div>
              <div class="online-status">
                <div class="online-dot"></div>
                En línea
              </div>
              <div class="location">
                <i class="fas fa-map-marker-alt"></i> 3 km
              </div>
            </div>
            
            <div class="profile-bio">
              Fotógrafa profesional y viajera. Amo los gatos y el café. He visitado más de 15 países y siempre estoy buscando mi próxima aventura.
            </div>
            
            <div class="prompt-box">
              <div class="prompt-question">Una habilidad que quiero aprender</div>
              <div class="prompt-answer">Fotografía submarina</div>
              <div class="prompt-nav">
                <button id="prevPromptBtn"><i class="fas fa-chevron-left"></i></button>
                <button id="nextPromptBtn"><i class="fas fa-chevron-right"></i></button>
              </div>
            </div>
            
            <div>
              <div class="interests-label">Intereses</div>
              <div class="interests">
                <div class="interest">Fotografía</div>
                <div class="interest">Viajes</div>
                <div class="interest">Café</div>
                <div class="interest">Animales</div>
                <div class="interest">Naturaleza</div>
              </div>
            </div>
            
            <div class="ai-suggestion">
              <div class="ai-header">
                <div class="ai-icon"><i class="fas fa-robot"></i></div>
                <div>
                  <div class="ai-title">Sugerencia AI</div>
                  <div class="ai-subtitle">Basada en intereses comunes</div>
                </div>
              </div>
              <div class="ai-message">
                "Hola Elena! Vi que te gusta la fotografía. Yo también disfruto capturar momentos especiales. ¿Cuál es tu lugar favorito para tomar fotos?"
              </div>
            </div>
          </div>
        </div>
        
        <div class="action-buttons">
          <div class="action-btn dislike-btn" id="dislikeBtn"><i class="fas fa-times"></i></div>
          <div class="action-btn superlike-btn" id="superlikeBtn"><i class="fas fa-star"></i></div>
          <div class="action-btn like-btn" id="likeBtn"><i class="fas fa-heart"></i></div>
        </div>
      </div>
      
      <div class="bottom-nav">
        <div class="nav-item active" id="discoverTab">
          <div class="nav-icon"><i class="fas fa-fire"></i></div>
          <div>Descubrir</div>
        </div>
        <div class="nav-item" id="messagesTab">
          <div class="nav-icon"><i class="fas fa-comments"></i></div>
