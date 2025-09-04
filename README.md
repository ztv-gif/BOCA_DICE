# BOCA_DICE
<!--
Licencia MIT
Copyright (c) 2025 ZTV

Por la presente se concede permiso, sin cargo, a cualquier persona que obtenga una copia
de este código y los archivos asociados, para usar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y/o vender copias del código,
siempre y cuando se dé crédito al autor original.

EL CÓDIGO SE ENTREGA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O IMPLÍCITA.
-->

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Boca Dice</title>
<style>
  body { margin: 0; font-family: Arial, sans-serif; background: #0033a0; color: #ffcc00; }
  header { background: #002b80; text-align: center; padding: 1.5rem; border-bottom: 5px solid #ffcc00; }
  header h1 { margin: 0; font-size: 2.5rem; }
  header p { margin: 0.5rem 0 0; font-size: 1.2rem; }
  main { max-width: 1000px; margin: 2rem auto; padding: 0 1rem; }
  section { margin-bottom: 3rem; }
  section h2 { border-bottom: 3px solid #ffcc00; padding-bottom: 0.5rem; }
  form { background: white; color: #333; padding: 1rem; border-radius: 12px; margin-bottom: 1.5rem; box-shadow: 0 3px 6px rgba(0,0,0,0.2); }
  form input, form textarea { width: 100%; margin-bottom: 0.5rem; padding: 0.5rem; border-radius: 6px; border: 1px solid #ccc; }
  form button { background: #ffcc00; color: #0033a0; padding: 0.5rem 1rem; border: none; border-radius: 6px; cursor: pointer; }
  form button:hover { background: #e6b800; }
  .noticia { background: white; color: #333; border-radius: 12px; padding: 1rem; margin-bottom: 1.5rem; box-shadow: 0 3px 6px rgba(0,0,0,0.2); }
  .noticia img { max-width: 100%; border-radius: 10px; margin-bottom: 0.5rem; }
  .partidos { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1rem; }
  .partido { background: white; color: #0033a0; border-radius: 10px; padding: 1rem; text-align: center; box-shadow: 0 3px 6px rgba(0,0,0,0.2); }
  footer { background: #002b80; color: #ffcc00; text-align: center; padding: 1rem; }
</style>
</head>
<body>

<header>
  <h1>BOCA DICE</h1>
  <p>Noticias y próximos partidos del Xeneize</p>
</header>

<main>
  <!-- Formulario de login -->
  <section id="loginSection">
    <h2>Acceso de edición</h2>
    <form id="formLogin">
      <input type="password" id="password" placeholder="Ingrese la contraseña" required>
      <button type="submit">Entrar</button>
    </form>
  </section>

  <!-- Formulario de noticias (oculto) -->
  <section id="noticiaSection" style="display:none;">
    <h2>Agregar noticia</h2>
    <form id="formNoticia">
      <input type="text" id="titulo" placeholder="Título de la noticia" required>
      <input type="text" id="subtitulo" placeholder="Subtítulo (opcional)">
      <input type="file" id="imagen" accept="image/*">
      <textarea id="descripcion" rows="4" placeholder="Información de la noticia" required></textarea>
      <button type="submit">Agregar noticia</button>
    </form>
  </section>

  <!-- Contenedor de noticias -->
  <section>
    <h2>Noticias</h2>
    <div id="contenedorNoticias"></div>
  </section>

  <!-- Formulario de partidos (oculto) -->
  <section id="partidoSection" style="display:none;">
    <h2>Agregar próximo partido</h2>
    <form id="formPartido">
      <input type="text" id="equipos" placeholder="Ej: Boca vs River" required>
      <input type="text" id="fechaHora" placeholder="Fecha y hora" required>
      <button type="submit">Agregar partido</button>
    </form>
  </section>

  <!-- Contenedor de partidos -->
  <section>
    <h2>Próximos partidos</h2>
    <div class="partidos" id="contenedorPartidos">
      <div class="partido">
        
      </div>
    </div>
  </section>
</main>

<footer>
  <p>© 2025 Boca Dice - Sitio de noticias hecho por ZTV</p>
</footer>

<script>
  // Contraseña ofuscada
  const passwordCodes = [
    74,65,67,73,78,84,79,50,48,53,48,46,72,84,77,76,95,95,50,48,49,57
  ]; 
  const PASSWORD = String.fromCharCode(...passwordCodes);

  const loginSection = document.getElementById('loginSection');
  const noticiaSection = document.getElementById('noticiaSection');
  const partidoSection = document.getElementById('partidoSection');
  const formLogin = document.getElementById('formLogin');

  formLogin.addEventListener('submit', function(e){
    e.preventDefault();
    const pass = document.getElementById('password').value;
    if(pass === PASSWORD){
      noticiaSection.style.display = "block";
      partidoSection.style.display = "block";
      loginSection.style.display = "none";
    } else {
      alert("Contraseña incorrecta");
    }
  });

  // Agregar noticias
  const formNoticia = document.getElementById('formNoticia');
  const contenedorNoticias = document.getElementById('contenedorNoticias');

  formNoticia.addEventListener('submit', function(e){
    e.preventDefault();
    const titulo = document.getElementById('titulo').value;
    const subtitulo = document.getElementById('subtitulo').value;
    const descripcion = document.getElementById('descripcion').value;
    const imagenInput = document.getElementById('imagen');
    const archivo = imagenInput.files[0];

    const div = document.createElement('div');
    div.classList.add('noticia');

    if(archivo){
      const reader = new FileReader();
      reader.onload = function(event){
        div.innerHTML = `<img src="${event.target.result}" alt="${titulo}">
                         <h3>${titulo}</h3>
                         ${subtitulo ? `<h4>${subtitulo}</h4>` : ''}
                         <p>${descripcion}</p>`;
        contenedorNoticias.prepend(div);
      }
      reader.readAsDataURL(archivo);
    } else {
      div.innerHTML = `<h3>${titulo}</h3>
                       ${subtitulo ? `<h4>${subtitulo}</h4>` : ''}
                       <p>${descripcion}</p>`;
      contenedorNoticias.prepend(div);
    }

    formNoticia.reset();
  });

  // Agregar partidos
  const formPartido = document.getElementById('formPartido');
  const contenedorPartidos = document.getElementById('contenedorPartidos');

  formPartido.addEventListener('submit', function(e){
    e.preventDefault();
    const equipos = document.getElementById('equipos').value;
    const fechaHora = document.getElementById('fechaHora').value;

    const div = document.createElement('div');
    div.classList.add('partido');
    div.innerHTML = `<h3>${equipos}</h3><p>${fechaHora}</p>`;
    contenedorPartidos.prepend(div);

    formPartido.reset();
  });
</script>

</body>
</html>

<!--
Fin de la licencia MIT
-->
