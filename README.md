# Dote-de-fuego
Ropa nueva 
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Dote de Fuego — Tienda</title>
<style>
  :root{
    --accent:#c41f1f;
    --bg:#0f0f10;
    --card:#121215;
    --text:#efefef;
    --muted:#bdbdbd;
    font-family: Inter, system-ui, Arial, sans-serif;
  }
  body{
    margin:0;
    background:linear-gradient(180deg,#0b0b0c 0%, #141417 100%);
    color:var(--text);
    -webkit-font-smoothing:antialiased;
  }
  header{
    padding:18px 20px;
    display:flex;
    align-items:center;
    gap:12px;
    border-bottom:1px solid rgba(255,255,255,0.03);
    position:sticky; top:0; background:rgba(11,11,12,0.6); backdrop-filter: blur(4px);
  }
  .logo{
    width:48px; height:48px; border-radius:10px;
    background:linear-gradient(135deg,var(--accent),#ff7a3d);
    display:flex; align-items:center; justify-content:center; font-weight:700;
    color:white; font-size:18px;
  }
  .brand {font-weight:700; font-size:18px}
  .subtitle {font-size:12px; color:var(--muted)}
  main{padding:20px; max-width:1100px; margin:0 auto;}
  .grid{
    display:grid;
    grid-template-columns: repeat(auto-fill,minmax(220px,1fr));
    gap:16px;
    margin-top:18px;
  }
  .card{
    background:var(--card);
    border-radius:10px;
    padding:12px;
    box-shadow: 0 6px 18px rgba(0,0,0,0.4);
    display:flex; flex-direction:column; gap:10px;
  }
  .thumb{height:200px; border-radius:8px; background:#222; background-size:cover; background-position:center;}
  .title{font-weight:700}
  .meta{font-size:13px; color:var(--muted)}
  .price{color:var(--accent); font-weight:800; font-size:16px}
  .btn-row{display:flex; gap:8px; margin-top:auto}
  .btn{
    padding:8px 10px; border-radius:8px; cursor:pointer; border:0; font-weight:700;
    background:linear-gradient(180deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
    color:var(--text);
  }
  .btn-primary{background:var(--accent); color:white}
  .badge{font-size:12px; padding:6px 8px; border-radius:999px; background:rgba(255,255,255,0.04); color:var(--muted)}
  footer{padding:20px; text-align:center; color:var(--muted); font-size:13px}
  /* mobile tweaks */
  @media (max-width:520px){
    .thumb{height:160px}
  }
  /* small product detail modal */
  .modal{position:fixed; inset:0; display:none; align-items:center; justify-content:center; z-index:40}
  .modal.open{display:flex}
  .modal .box{width:95%; max-width:760px; background:#0b0b0c; border-radius:10px; padding:16px;}
  .row{display:flex; gap:12px; align-items:flex-start}
  .col{flex:1}
  .small{font-size:13px;color:var(--muted)}
</style>
</head>
<body>
<header>
  <div class="logo">DF</div>
  <div>
    <div class="brand">Dote de Fuego</div>
    <div class="subtitle">Ropa nueva y 2a mano • Envíos a todo Colombia</div>
  </div>
</header>

<main>
  <section>
    <h2>Productos disponibles</h2>
    <p class="small">Reglas: el <strong>primero que comenta y paga</strong> se lleva el producto. Lee envíos y pagos abajo.</p>

    <div class="grid" id="products">
      <!-- Product cards se inyectan desde JS -->
    </div>
  </section>

  <section style="margin-top:26px;">
    <h3>Reglas de venta / Envíos</h3>
    <ul class="small">
      <li>Reservas solo con comprobante de pago (no se reservan por comentario).</li>
      <li>Envíos por Servientrega/EnviaCol/Coordinadora — costo a cargo del comprador.</li>
      <li>Pago por Nequi / Daviplata / Bancolombia (transferencia) / Efectivo en entrega (solo Tunja).</li>
      <li>Devoluciones: no aplican en ropa usada; en ropa nueva reportar en 24 horas.</li>
      <li>Contactos: WhatsApp: <strong>+57 300 000 0000</strong> (ejemplo), Instagram: <strong>@dotedefuego</strong></li>
    </ul>
  </section>
</main>

<footer>
  © Dote de Fuego — Tunja • Política: primer comentario + pago = venta. Preguntas a DM o WhatsApp.
</footer>

<!-- modal detalle -->
<div class="modal" id="modal">
  <div class="box">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
      <strong id="m-title">Título</strong>
      <button onclick="closeModal()" class="btn">Cerrar</button>
    </div>
    <div class="row">
      <div style="flex:0 0 45%; min-height:220px; background:#222; border-radius:8px; background-size:cover; background-position:center" id="m-thumb"></div>
      <div class="col">
        <div style="margin-bottom:8px;"><span class="price" id="m-price">$0</span> <span class="badge" id="m-condition">Nuevo</span></div>
        <div class="small" id="m-desc">Descripción más larga...</div>
        <div style="margin-top:12px;">
          <div class="small"><strong>Tallas:</strong> <span id="m-sizes">S / M / L</span></div>
          <div class="small"><strong>Estado:</strong> <span id="m-estado">Usado buen estado</span></div>
          <div style="margin-top:12px;">
            <button class="btn btn-primary" onclick="openContact()">¡Lo quiero! (Comentar y pagar)</button>
            <button class="btn" onclick="openWhatsapp()">Enviar WhatsApp</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
  const PRODUCTS = [
    {
      id:1,
      title:"Camiseta negra vintage - talla M",
      price:"$35.000",
      thumb:"https://images.unsplash.com/photo-1520975698894-5f2f3b8b7c33?w=800&q=60",
      condition:"Usada - buen estado",
      desc:"Camiseta negra con estampado sutil. Ideal para streetwear. Marca: desconocida.",
      sizes:"M",
      estado:"Usada"
    },
    {
      id:2,
      title:"Chaqueta impermeable nueva - talla L",
      price:"$120.000",
      thumb:"https://images.unsplash.com/photo-1541099649105-f69ad21f3246?w=800&q=60",
      condition:"Nueva con etiqueta",
      desc:"Chaqueta ligera impermeable, perfecta para lluvia y moto. Color: verde oliva.",
      sizes:"L",
      estado:"Nueva"
    }
  ];

  const container = document.getElementById('products');
  PRODUCTS.forEach(p=>{
    const el = document.createElement('div');
    el.className='card';
    el.innerHTML = `
      <div class="thumb" style="background-image:url('${p.thumb}')"></div>
      <div class="title">${p.title}</div>
      <div class="meta">${p.sizes} • ${p.condition}</div>
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div class="price">${p.price}</div>
        <div class="badge">ID ${p.id}</div>
      </div>
      <div class="btn-row">
        <button class="btn btn-primary" onclick="openModal(${p.id})">Ver</button>
        <button class="btn" onclick="openWhatsQuick('${p.title}')">WhatsApp</button>
      </div>
    `;
    container.appendChild(el);
  });

  function openModal(id){
    const p = PRODUCTS.find(x=>x.id===id);
    if(!p) return;
    document.getElementById('m-title').innerText = p.title;
    document.getElementById('m-price').innerText = p.price;
    document.getElementById('m-thumb').style.backgroundImage = `url('${p.thumb}')`;
    document.getElementById('m-desc').innerText = p.desc;
    document.getElementById('m-sizes').innerText = p.sizes;
    document.getElementById('m-estado').innerText = p.estado;
    document.getElementById('m-condition').innerText = p.condition;
    document.getElementById('modal').classList.add('open');
  }
  function closeModal(){ document.getElementById('modal').classList.remove('open'); }
  function openContact(){ window.open('https://wa.me/573000000000?text=Estoy%20interesado%20en%20este%20producto','_blank'); }
  function openWhatsapp(){ openContact(); }
  function openWhatsQuick(title){ window.open('https://wa.me/573000000000?text=Hola,%20estoy%20interesado%20en%20'+encodeURIComponent(title),'_blank') }
</script>
</body>
</html>
