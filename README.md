<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#2a2540">
<title>Le Pendu</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Baloo+2:wght@600;700;800&family=Caveat:wght@700&display=swap" rel="stylesheet">
<style>
  @property --skyTop{syntax:"<color>";inherits:true;initial-value:#9ad7ff;}
  @property --skyBottom{syntax:"<color>";inherits:true;initial-value:#eaf7ff;}
  :root{
    --bg1:#3a2f5e; --bg2:#1f1b33; --card:#fffdf7; --ink:#2c2540;
    --wood:#a6713f; --wood-d:#7c4f28; --wood-l:#c79360;
    --rope:#caa46a; --skin:#f4c79c; --skin-d:#d99e6f;
    --shirt:#e8604c; --pants:#3b6ea5;
    --good:#2fae6b; --bad:#e8604c; --tile:#fff; --tile-edge:#e4ddc9;
  }
  *{box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
  html,body{margin:0;height:100%;}
  body{font-family:'Baloo 2',system-ui,sans-serif;color:var(--ink);
    background:linear-gradient(160deg,var(--bg1),var(--bg2));
    display:flex;justify-content:center;min-height:100dvh;}
  .wrap{width:100%;max-width:460px;padding:12px 14px 20px;display:flex;flex-direction:column;gap:10px;}

  header{display:flex;align-items:center;justify-content:space-between;gap:10px;}
  h1{font-family:'Caveat',cursive;color:#fff;font-size:2.9rem;margin:0;line-height:.9;text-shadow:0 2px 0 rgba(0,0,0,.25);}
  .coeurs{font-size:1.15rem;letter-spacing:1px;white-space:nowrap;line-height:1;}
  .coeurs .v{opacity:.28;filter:grayscale(1);}

  .topline{display:flex;align-items:center;justify-content:space-between;}
  .cat{background:rgba(255,255,255,.15);color:#fff;font-weight:700;padding:5px 12px;border-radius:999px;font-size:.95rem;}
  .cat b{color:#ffd45e;}
  .serie{color:#ffd45e;font-weight:700;font-size:.95rem;}

  .scene{position:relative;border-radius:20px;overflow:hidden;box-shadow:0 10px 30px rgba(0,0,0,.35);background:#9ad7ff;}
  .scene svg{display:block;width:100%;height:auto;
    --skyTop:#9ad7ff;--skyBottom:#eaf7ff;--sunOp:1;--cloudOp:0;--starOp:0;--rainOp:0;--birdOp:1;
    transition:--skyTop .7s ease,--skyBottom .7s ease;}
  .sky-stop-top{stop-color:var(--skyTop);} .sky-stop-bot{stop-color:var(--skyBottom);}
  #sun{opacity:var(--sunOp);transition:opacity .6s ease;transform-box:view-box;transform-origin:252px 46px;}
  #sun.burst{animation:sunburst .9s ease-out;}
  @keyframes sunburst{0%{transform:scale(.6)}55%{transform:scale(1.25)}100%{transform:scale(1)}}
  #clouds{opacity:var(--cloudOp);transition:opacity .6s ease;animation:drift 16s ease-in-out infinite;}
  @keyframes drift{0%,100%{transform:translateX(-6px)}50%{transform:translateX(14px)}}
  #stars{opacity:var(--starOp);transition:opacity .6s ease;}
  #stars circle{animation:twinkle 2.6s ease-in-out infinite;}
  @keyframes twinkle{0%,100%{opacity:.35}50%{opacity:1}}
  #birds{opacity:var(--birdOp);transition:opacity .6s ease;animation:fly 19s linear infinite;}
  @keyframes fly{0%{transform:translateX(-70px)}100%{transform:translateX(330px)}}
  #rain{opacity:var(--rainOp);transition:opacity .6s ease;}
  .drop{stroke:#d4ecff;stroke-width:2;stroke-linecap:round;}
  @keyframes fall{0%{transform:translateY(-24px);opacity:0}12%{opacity:.85}100%{transform:translateY(184px);opacity:0}}
  #bolt{opacity:0;}
  #bolt.zap{animation:zap .6s ease-out;}
  @keyframes zap{0%,100%{opacity:0}8%{opacity:1}22%{opacity:.1}38%{opacity:.9}55%{opacity:0}}

  .wood{fill:var(--wood);stroke:var(--wood-d);stroke-width:2;}
  .wood-l{fill:var(--wood-l);} .grain{stroke:var(--wood-d);stroke-width:1;opacity:.45;}
  .part{opacity:0;transform:scale(0);transform-box:fill-box;transform-origin:center;}
  .part.pop{opacity:1;animation:pop .42s cubic-bezier(.34,1.56,.64,1) forwards;}
  @keyframes pop{0%{transform:scale(0)}65%{transform:scale(1.18)}100%{transform:scale(1)}}
  #figure.swing{transform-box:view-box;transform-origin:201px 50px;animation:swing 2.4s ease-in-out .1s 2 alternate;}
  @keyframes swing{0%{transform:rotate(-7deg)}100%{transform:rotate(7deg)}}
  .lost-only{display:none;} #figure.lost .live-only{display:none;} #figure.lost .lost-only{display:block;}
  .sweat{opacity:0;transition:opacity .3s;} #figure.worried .sweat{opacity:1;}
  .flash{position:absolute;inset:0;background:#fff;opacity:0;pointer-events:none;}
  .flash.go{animation:flash .5s ease-out;}
  @keyframes flash{0%,100%{opacity:0}10%{opacity:.85}30%{opacity:.1}45%{opacity:.6}}
  canvas.confetti{position:absolute;inset:0;width:100%;height:100%;pointer-events:none;}

  .mot{display:flex;flex-wrap:wrap;justify-content:center;gap:6px;min-height:52px;}
  .slot{width:30px;height:42px;border-radius:8px;background:var(--tile);border:2px solid var(--tile-edge);
    box-shadow:0 3px 0 var(--tile-edge);display:flex;align-items:center;justify-content:center;font-weight:800;font-size:1.5rem;}
  .slot.filled{background:#fff7df;border-color:#f1d98a;box-shadow:0 3px 0 #f1d98a;}
  .slot .ch{display:inline-block;animation:drop2 .35s cubic-bezier(.34,1.56,.64,1);}
  @keyframes drop2{0%{transform:translateY(-10px) scale(.4);opacity:0}100%{transform:none;opacity:1}}
  .slot.miss{background:#ffe3df;border-color:var(--bad);box-shadow:0 3px 0 var(--bad);color:var(--bad);}
  .gap{width:12px;} .sep{width:18px;display:flex;align-items:center;justify-content:center;color:#fff;font-weight:800;font-size:1.5rem;}

  .msg{text-align:center;min-height:30px;font-family:'Caveat',cursive;font-size:2rem;color:#fff;line-height:1;}
  .msg.win{color:#aef5c8;} .msg.lose{color:#ffb3a8;} .msg b{font-family:'Baloo 2';font-size:1.2rem;}

  .clavier{display:flex;flex-direction:column;gap:7px;margin-top:2px;}
  .rangee{display:flex;justify-content:center;gap:6px;}
  .touche{font-family:'Baloo 2';font-weight:800;font-size:1.15rem;color:var(--ink);background:var(--tile);border:none;
    border-radius:10px;flex:1 1 0;max-width:40px;height:48px;box-shadow:0 4px 0 #c8c0a8;cursor:pointer;
    transition:transform .05s,box-shadow .05s,background .15s;}
  .touche:active{transform:translateY(4px);box-shadow:0 0 0 #c8c0a8;}
  .touche:disabled{cursor:default;}
  .touche.bon{background:var(--good);color:#fff;box-shadow:0 4px 0 #1f8650;}
  .touche.faux{background:#d9d3c4;color:#8f8a7d;box-shadow:0 4px 0 #b8b09c;opacity:.85;}
  .touche:focus-visible{outline:3px solid #ffd45e;outline-offset:2px;}

  .rejouer{margin-top:8px;font-family:'Baloo 2';font-weight:800;font-size:1.15rem;color:#3a2f5e;background:#ffd45e;border:none;
    border-radius:14px;padding:14px;box-shadow:0 5px 0 #d6a92e;cursor:pointer;transition:transform .05s,box-shadow .05s;}
  .rejouer:active{transform:translateY(5px);box-shadow:0 0 0 #d6a92e;}

  @media (prefers-reduced-motion:reduce){
    .part.pop{animation:none;opacity:1;transform:none;} #figure.swing,#clouds,#birds,#stars circle,#sun.burst{animation:none;}
    .slot .ch,.drop{animation:none;} .touche:active,.rejouer:active{transform:none;}
  }
  @media (max-height:700px){ h1{font-size:2.3rem;} .touche{height:44px;} .slot{height:38px;} }
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>Le Pendu</h1>
    <div class="coeurs" id="coeurs" aria-label="vies restantes"></div>
  </header>
  <div class="topline">
    <span class="cat">Catégorie : <b id="cat">…</b></span>
    <span class="serie" id="serie">série 0</span>
  </div>

  <div class="scene">
    <svg viewBox="0 0 300 250" role="img" aria-label="Scène du pendu">
      <defs>
        <linearGradient id="skyGrad" x1="0" y1="0" x2="0" y2="1">
          <stop class="sky-stop-top" offset="0"/><stop class="sky-stop-bot" offset="1"/>
        </linearGradient>
        <radialGradient id="sunGrad" cx="50%" cy="50%" r="50%">
          <stop offset="0" stop-color="#fff3b0"/><stop offset="1" stop-color="#ffd23e"/>
        </radialGradient>
      </defs>

      <rect x="0" y="0" width="300" height="250" fill="url(#skyGrad)"/>

      <g id="stars">
        <circle cx="40" cy="40" r="1.6" fill="#fff"/><circle cx="90" cy="25" r="1.2" fill="#fff" style="animation-delay:.4s"/>
        <circle cx="150" cy="45" r="1.8" fill="#fff" style="animation-delay:.8s"/><circle cx="210" cy="30" r="1.3" fill="#fff" style="animation-delay:1.2s"/>
        <circle cx="260" cy="60" r="1.6" fill="#fff" style="animation-delay:.2s"/><circle cx="120" cy="70" r="1.1" fill="#fff" style="animation-delay:1.6s"/>
        <circle cx="275" cy="25" r="1.4" fill="#fff" style="animation-delay:1s"/><circle cx="20" cy="80" r="1.2" fill="#fff" style="animation-delay:.6s"/>
      </g>

      <g id="birds" stroke="#46506b" stroke-width="2" fill="none" stroke-linecap="round">
        <path d="M30,46 Q36,40 42,46 Q48,40 54,46"/>
        <path d="M75,62 Q80,57 85,62 Q90,57 95,62" transform="scale(.8)"/>
      </g>

      <g id="sun">
        <g stroke="#ffe07a" stroke-width="3" stroke-linecap="round">
          <line x1="252" y1="14" x2="252" y2="2"/><line x1="252" y1="78" x2="252" y2="90"/>
          <line x1="216" y1="46" x2="204" y2="46"/><line x1="288" y1="46" x2="300" y2="46"/>
          <line x1="227" y1="21" x2="219" y2="13"/><line x1="277" y1="71" x2="285" y2="79"/>
          <line x1="277" y1="21" x2="285" y2="13"/><line x1="227" y1="71" x2="219" y2="79"/>
        </g>
        <circle cx="252" cy="46" r="20" fill="url(#sunGrad)"/>
      </g>

      <g id="clouds" fill="#ffffff">
        <g opacity=".95"><ellipse cx="70" cy="55" rx="26" ry="15"/><ellipse cx="95" cy="50" rx="20" ry="16"/><ellipse cx="48" cy="60" rx="18" ry="12"/></g>
        <g opacity=".8" transform="translate(150,18) scale(.8)"><ellipse cx="70" cy="55" rx="26" ry="15"/><ellipse cx="95" cy="50" rx="20" ry="16"/><ellipse cx="48" cy="60" rx="18" ry="12"/></g>
      </g>

      <g id="rain"></g>

      <g id="bolt" stroke="#ffe23a" stroke-width="4" fill="#ffe23a" stroke-linejoin="round">
        <polygon points="150,40 132,96 148,96 130,150 168,86 150,86 164,40"/>
      </g>

      <!-- collines -->
      <path d="M0,212 Q90,190 300,210 L300,250 L0,250 Z" fill="#8fd07f"/>
      <path d="M0,205 Q150,178 300,205 L300,250 L0,250 Z" fill="#6cc15f"/>
      <path d="M0,205 Q150,178 300,205" fill="none" stroke="#54a847" stroke-width="4"/>
      <g stroke="#4f9e43" stroke-width="2" stroke-linecap="round">
        <path d="M30,206 L30,198 M34,206 L36,199 M26,206 L24,200"/>
        <path d="M250,206 L250,198 M254,206 L256,199 M246,206 L244,200"/>
        <path d="M150,200 L150,192 M154,200 L156,193 M146,200 L144,194"/>
      </g>
      <g>
        <circle cx="44" cy="214" r="3.2" fill="#ff6f91"/><circle cx="44" cy="214" r="1.4" fill="#ffd45e"/>
        <circle cx="262" cy="216" r="3.2" fill="#fff" stroke="#ddd"/><circle cx="262" cy="216" r="1.4" fill="#ffd45e"/>
        <circle cx="120" cy="220" r="3" fill="#b98cff"/><circle cx="120" cy="220" r="1.3" fill="#ffd45e"/>
      </g>

      <!-- potence -->
      <g>
        <rect class="wood" x="40" y="198" width="84" height="14" rx="5"/>
        <rect class="wood" x="74" y="44" width="14" height="158" rx="4"/>
        <rect class="wood" x="74" y="40" width="134" height="14" rx="4"/>
        <polygon class="wood" points="88,54 88,80 114,54"/>
        <line class="grain" x1="80" y1="60" x2="80" y2="190"/><line class="grain" x1="84" y1="70" x2="84" y2="185"/>
        <line class="grain" x1="100" y1="44" x2="180" y2="44"/><line class="grain" x1="110" y1="50" x2="190" y2="50"/>
        <rect class="wood-l" x="77" y="46" width="4" height="150" rx="2"/>
      </g>

      <!-- personnage -->
      <g id="figure">
        <path class="part rope" data-step="1" d="M201,50 L201,70" stroke="var(--rope)" stroke-width="5" stroke-linecap="round" fill="none"/>
        <g class="part" data-step="2">
          <circle cx="201" cy="86" r="15" fill="var(--skin)" stroke="var(--skin-d)" stroke-width="2"/>
          <g class="live-only">
            <circle cx="195" cy="84" r="2.3" fill="#3a2f2f"/><circle cx="207" cy="84" r="2.3" fill="#3a2f2f"/>
            <path d="M195,94 Q201,90 207,94" fill="none" stroke="#3a2f2f" stroke-width="2" stroke-linecap="round"/>
          </g>
          <g class="lost-only" stroke="#3a2f2f" stroke-width="2" stroke-linecap="round">
            <path d="M192,81 L198,87 M198,81 L192,87"/><path d="M204,81 L210,87 M210,81 L204,87"/>
            <path d="M195,95 Q201,99 207,95" fill="none"/>
          </g>
          <ellipse class="sweat" cx="214" cy="88" rx="2.6" ry="4" fill="#7ec8ff"/>
        </g>
        <rect class="part" data-step="3" x="193" y="99" width="16" height="46" rx="8" fill="var(--shirt)" stroke="#c44a39" stroke-width="2"/>
        <path class="part" data-step="4" d="M194,106 L176,126" stroke="var(--skin)" stroke-width="8" stroke-linecap="round" fill="none"/>
        <path class="part" data-step="5" d="M208,106 L226,126" stroke="var(--skin)" stroke-width="8" stroke-linecap="round" fill="none"/>
        <path class="part" data-step="6" d="M197,145 L185,176" stroke="var(--pants)" stroke-width="9" stroke-linecap="round" fill="none"/>
        <path class="part" data-step="7" d="M205,145 L217,176" stroke="var(--pants)" stroke-width="9" stroke-linecap="round" fill="none"/>
      </g>
    </svg>
    <div class="flash" id="flash"></div>
    <canvas class="confetti" id="confetti"></canvas>
  </div>

  <div class="mot" id="mot" aria-live="polite"></div>
  <div class="msg" id="msg"></div>

  <div class="clavier" id="clavier"></div>
  <button class="rejouer" id="rejouer">Nouvelle partie</button>
</div>

<script>
const BANQUE = {
  "Animaux":["ÉLÉPHANT","GIRAFE","CROCODILE","HÉRISSON","ÉCUREUIL","PAPILLON","TORTUE","RENARD","BALEINE","CHOUETTE","PINGOUIN","KANGOUROU","DAUPHIN","GRENOUILLE","HIPPOPOTAME","SERPENT","FLAMANT","PERROQUET","LIBELLULE","MARMOTTE","LOUTRE","PANTHÈRE","SCARABÉE","MOUSTIQUE","HIBOU","BLAIREAU","CHAMEAU","ZÈBRE","GORILLE","AUTRUCHE","COCCINELLE","SANGLIER","PÉLICAN","RHINOCÉROS","CHIMPANZÉ","ESCARGOT","MÉSANGE","HÉRON","LÉZARD","CASTOR","BELETTE","FOURMI","ABEILLE","CHAUVE-SOURIS","ÉPERVIER","MORSE","PHOQUE","RAGONDIN"],
  "Fruits & légumes":["FRAISE","ABRICOT","POTIRON","AUBERGINE","FRAMBOISE","ARTICHAUT","PASTÈQUE","CITROUILLE","CERISE","BANANE","ANANAS","MYRTILLE","GROSEILLE","COURGETTE","POIVRON","BROCOLI","ÉPINARD","RHUBARBE","CLÉMENTINE","BETTERAVE","ASPERGE","RADIS","CONCOMBRE","PRUNE","MANDARINE","PAMPLEMOUSSE","GRENADE","POIREAU","CHAMPIGNON","HARICOT","LENTILLE","AVOCAT","FIGUE","NAVET","CITRON","PÊCHE","MELON","COURGE","ENDIVE","FENOUIL","OIGNON","CHÂTAIGNE","NOISETTE"],
  "Pays":["PORTUGAL","BELGIQUE","ÉTHIOPIE","AUSTRALIE","ARGENTINE","THAÏLANDE","NORVÈGE","CANADA","BRÉSIL","ÉGYPTE","ISLANDE","MAROC","JAPON","MEXIQUE","FINLANDE","SÉNÉGAL","ÉQUATEUR","AUTRICHE","CROATIE","VIETNAM","INDONÉSIE","IRLANDE","ESPAGNE","ALLEMAGNE","POLOGNE","ROUMANIE","COLOMBIE","NIGÉRIA","KENYA","PÉROU","GRÈCE","TURQUIE","SUÈDE","HONGRIE","CHILI","BOLIVIE","ALGÉRIE","TUNISIE","UKRAINE","ÉCOSSE","DANEMARK","SUISSE","ITALIE","CAMBODGE"],
  "Villes":["MARSEILLE","BORDEAUX","STRASBOURG","TOULOUSE","MONTRÉAL","BRUXELLES","GENÈVE","BARCELONE","VENISE","LISBONNE","ISTANBUL","AMSTERDAM","HELSINKI","NANTES","GRENOBLE","MONTPELLIER","LYON","NICE","LILLE","RENNES","AVIGNON","QUÉBEC","COPENHAGUE","VARSOVIE","SÉVILLE","DUBLIN","NAPLES","BERLIN","PRAGUE","VIENNE","ATHÈNES","CASABLANCA","SINGAPOUR"],
  "Objets":["PARAPLUIE","ORDINATEUR","TÉLÉPHONE","FAUTEUIL","BOUSSOLE","LAMPADAIRE","TROMBONE","CISEAUX","BOUTEILLE","VALISE","LUNETTES","MARTEAU","CLAVIER","BROUETTE","ÉCHELLE","ARROSOIR","AGRAFEUSE","PARASOL","HORLOGE","BOUGIE","PINCEAU","CADENAS","ÉPONGE","BALANCE","JUMELLES","TABOURET","BROSSE","ENTONNOIR","BOCAL","CARAFE","ÉVENTAIL","THERMOMÈTRE","CHANDELIER","PASSOIRE","TIRELIRE","BOUÉE"],
  "Nature":["MONTAGNE","CASCADE","COQUILLAGE","ARC-EN-CIEL","VOLCAN","NÉNUPHAR","BANQUISE","FORÊT","RIVIÈRE","NUAGE","ÉCLAIR","TORNADE","GLACIER","DÉSERT","MARÉE","FALAISE","BRUME","AURORE","RUISSEAU","CRATÈRE","PRAIRIE","CANYON","OASIS","MARÉCAGE","ROSÉE","AVALANCHE","TONNERRE","CAVERNE","LAGON","COLLINE","TORRENT","GEYSER","RÉCIF","DUNE","ESTUAIRE"],
  "Métiers":["BOULANGER","ASTRONAUTE","JARDINIER","MÉDECIN","ARCHITECTE","PLOMBIER","POMPIER","VÉTÉRINAIRE","MENUISIER","PÂTISSIER","ÉLECTRICIEN","INFIRMIÈRE","PROFESSEUR","CUISINIER","FACTEUR","BIBLIOTHÉCAIRE","HORLOGER","COMÉDIEN","JOURNALISTE","PHARMACIEN","MÉCANICIEN","AGRICULTEUR","PEINTRE","SERVEUR","COIFFEUR","MARIN","DENTISTE","FLEURISTE","BOUCHER","CHARPENTIER","PILOTE","NOTAIRE","BIJOUTIER","COUTURIER","TRADUCTEUR","ARCHÉOLOGUE"],
  "Maison":["CUISINE","GRENIER","CHEMINÉE","ESCALIER","COUSSIN","BAIGNOIRE","FENÊTRE","ARMOIRE","MATELAS","ÉTAGÈRE","CANAPÉ","RIDEAU","PLAFOND","TIROIR","BALCON","OREILLER","ROBINET","COULOIR","VÉRANDA","MOQUETTE","COMMODE","LAVABO","PLACARD","PAILLASSON","CHAUDIÈRE","TERRASSE","CLOISON","PARQUET","LUSTRE","TAPISSERIE","SERRURE"],
  "Sport":["NATATION","ESCALADE","CYCLISME","ESCRIME","AVIRON","GYMNASTIQUE","PLONGÉE","RANDONNÉE","BASKET","HANDBALL","ÉQUITATION","PATINAGE","VOILE","ATHLÉTISME","BADMINTON","SURF","TENNIS","KARATÉ","MARATHON","TRAMPOLINE","BOXE","RUGBY","PÉTANQUE","CANOË","LUTTE","PLANCHE","TIRE-À-L'ARC","HALTÈRES","SKATEBOARD","TAEKWONDO"],
  "Musique":["GUITARE","VIOLON","TROMPETTE","BATTERIE","ACCORDÉON","SAXOPHONE","PIANO","FLÛTE","HARPE","CLARINETTE","ORCHESTRE","MÉLODIE","TAMBOURIN","VIOLONCELLE","CHORALE","PARTITION","CORNEMUSE","UKULÉLÉ","BANJO","XYLOPHONE","TUBA","MARACAS","MÉTRONOME","HARMONICA","CONCERTO","SYMPHONIE","REFRAIN","CYMBALE"],
  "Corps humain":["ÉPAULE","GENOU","CHEVILLE","POIGNET","COUDE","MENTON","SOURCIL","TALON","NUQUE","MÂCHOIRE","OMOPLATE","POUMON","CRÂNE","CLAVICULE","POITRINE","TENDON","ARTÈRE","ESTOMAC","CERVEAU","NARINE","GENCIVE","ABDOMEN","CHEVEUX","ORTEIL","ROTULE","VERTÈBRE","PAUPIÈRE","MOLLET"],
  "Nourriture":["BAGUETTE","FROMAGE","CROISSANT","OMELETTE","RACLETTE","GAUFRE","CHOCOLAT","CONFITURE","RATATOUILLE","QUICHE","BRIOCHE","MACARON","POTAGE","CRÊPE","TARTIFLETTE","SANDWICH","SPAGHETTI","LASAGNE","HAMBURGER","PIZZA","SALADE","BISCUIT","BONBON","YAOURT","SAUCISSE","GRATIN","TIRAMISU","RAVIOLI","NOUGAT","CARAMEL","BEIGNET","SORBET","FONDUE","CASSOULET"],
  "Transports":["VOITURE","BICYCLETTE","AVION","HÉLICOPTÈRE","TRAMWAY","MÉTRO","BATEAU","CAMION","SCOOTER","MONTGOLFIÈRE","FUSÉE","SOUS-MARIN","TROTTINETTE","PAQUEBOT","LOCOMOTIVE","AMBULANCE","TÉLÉPHÉRIQUE","CARAVANE","DÉCAPOTABLE","VOILIER","TRACTEUR","FUNICULAIRE","DIRIGEABLE"],
  "École":["CARTABLE","TROUSSE","GOMME","RÈGLE","CAHIER","STYLO","TABLEAU","CRAIE","ÉQUERRE","COMPAS","DICTIONNAIRE","RÉCRÉATION","PUPITRE","CLASSEUR","SURLIGNEUR","CALCULATRICE","FEUTRE","AGENDA","PORTE-MINE","BULLETIN"],
  "Vêtements":["PANTALON","CHEMISE","ÉCHARPE","MANTEAU","CHAUSSURE","CHAPEAU","GANTS","ROBE","CRAVATE","CEINTURE","PYJAMA","SANDALE","BLOUSON","BONNET","CHAUSSETTE","VESTE","JUPE","SWEAT","COSTUME","FOULARD","SALOPETTE","IMPERMÉABLE","MOUFLE"],
  "Espace":["PLANÈTE","GALAXIE","COMÈTE","MÉTÉORE","SATELLITE","TÉLESCOPE","CONSTELLATION","NÉBULEUSE","ASTÉROÏDE","SOLEIL","NAVETTE","ÉCLIPSE","ORBITE","SUPERNOVA","COSMONAUTE","ATTERRISSEUR","ANNEAU","CRATÈRE","GRAVITÉ"],
  "Fleurs & plantes":["TOURNESOL","MARGUERITE","COQUELICOT","TULIPE","JONQUILLE","ORCHIDÉE","LAVANDE","MUGUET","PISSENLIT","FOUGÈRE","CACTUS","BAMBOU","LIERRE","GLYCINE","PÂQUERETTE","CHRYSANTHÈME","VIOLETTE","GÉRANIUM","MIMOSA","ROSIER","JACINTHE","NÉNUPHAR","CAMÉLIA","PRIMEVÈRE"],
  "Boissons":["LIMONADE","TISANE","SMOOTHIE","MILKSHAKE","GRENADINE","CITRONNADE","CAPPUCCINO","ESPRESSO","INFUSION","NECTAR","SIROP","SODA","CIDRE","CHOCOLAT","MOJITO","ORANGEADE","FRAPPÉ","TILLEUL"],
  "Mythologie":["DRAGON","LICORNE","SORCIÈRE","CHEVALIER","PRINCESSE","OGRE","LUTIN","VAMPIRE","FANTÔME","SIRÈNE","GÉANT","TROLL","PHÉNIX","GRIFFON","MINOTAURE","CYCLOPE","GOBELIN","MÉDUSE","CENTAURE","PÉGASE","FARFADET","SORCIER"],
  "Outils":["TOURNEVIS","PERCEUSE","SCIE","PINCE","RABOT","NIVEAU","VISSEUSE","TENAILLE","BOULON","ÉCROU","PONCEUSE","ÉTABLI","BURIN","MÈCHE","ENCLUME","CISAILLE","SÉCATEUR","TRUELLE","PIOCHE","RÂTEAU"],
  "Couleurs":["ROUGE","ORANGE","JAUNE","VERT","VIOLET","MARRON","BLANC","TURQUOISE","INDIGO","POURPRE","BEIGE","FUCHSIA","ÉMERAUDE","ÉCARLATE","OCRE","MAUVE","LAVANDE","CORAIL","SAUMON","AZUR"],
  "Informatique":["SOURIS","ÉCRAN","INTERNET","LOGICIEL","FICHIER","DOSSIER","RÉSEAU","NAVIGATEUR","IMPRIMANTE","ROBOT","PIXEL","CASQUE","MANETTE","COURRIEL","CLAVIER","ALGORITHME","SERVEUR","TÉLÉCHARGEMENT","MOTDEPASSE"],
  "Météo":["ORAGE","TEMPÊTE","BROUILLARD","VERGLAS","CANICULE","GRÊLE","BRUINE","ÉCLAIRCIE","GIVRE","MOUSSON","OURAGAN","BOURRASQUE","ROSÉE","BLIZZARD","SÉCHERESSE","CRACHIN"]
};

const RANGEES=["AZERTYUIOP","QSDFGHJKLM","WXCVBN"];
const MAX=7;
const STAGES=[
  {top:'#9ad7ff',bottom:'#eaf7ff',sun:1,cloud:0},
  {top:'#8fcdf6',bottom:'#e6f4fe',sun:1,cloud:.15},
  {top:'#a9c6df',bottom:'#eef0ee',sun:.75,cloud:.45},
  {top:'#f0bd8a',bottom:'#ffe9cf',sun:.55,cloud:.65},
  {top:'#e79a6b',bottom:'#ffce9a',sun:.35,cloud:.8},
  {top:'#8f6a93',bottom:'#e7a079',sun:.15,cloud:.95},
  {top:'#4f447a',bottom:'#8f6a93',sun:0,cloud:1},
  {top:'#241f44',bottom:'#4a3f72',sun:0,cloud:1}
];
const reduce=window.matchMedia('(prefers-reduced-motion:reduce)').matches;
const norm=s=>s.normalize('NFD').replace(/[\u0300-\u036f]/g,'').toUpperCase();
const estLettre=c=>/[A-ZÀ-ÖØ-Þ]/i.test(c);

let mot="",cat="",devinees=new Set(),erreurs=0,fini=false,serie=0;
const $=id=>document.getElementById(id);
const svg=document.querySelector('.scene svg');
const $parts=[...document.querySelectorAll('.part')];
const figure=$('figure');

// pluie générée une fois
(function buildRain(){
  const g=$('rain');
  for(let i=0;i<18;i++){
    const x=8+Math.random()*284;
    const l=document.createElementNS('http://www.w3.org/2000/svg','line');
    l.setAttribute('class','drop');
    l.setAttribute('x1',x);l.setAttribute('y1',40);
    l.setAttribute('x2',x-3);l.setAttribute('y2',52);
    if(!reduce) l.style.animation=`fall ${(.6+Math.random()*.5).toFixed(2)}s linear ${(Math.random()*1).toFixed(2)}s infinite`;
    g.appendChild(l);
  }
})();

function setScene(e){
  const s=STAGES[Math.min(e,MAX)];
  svg.style.setProperty('--skyTop',s.top);
  svg.style.setProperty('--skyBottom',s.bottom);
  svg.style.setProperty('--sunOp',s.sun);
  svg.style.setProperty('--cloudOp',s.cloud);
  svg.style.setProperty('--starOp', e>=5?(e-4)/3:0);
  svg.style.setProperty('--rainOp', e>=4?Math.min((e-3)/3,1):0);
  svg.style.setProperty('--birdOp', e<=1?1:0);
}
function coeurs(){
  let h="";for(let i=0;i<MAX;i++) h+=`<span class="${i<MAX-erreurs?'c':'v'}">❤</span>`;
  $('coeurs').innerHTML=h;
}
function clavier(){
  $('clavier').innerHTML="";
  for(const r of RANGEES){
    const row=document.createElement('div');row.className="rangee";
    for(const L of r){
      const b=document.createElement('button');
      b.className="touche";b.textContent=L;b.dataset.l=L;
      b.addEventListener('click',()=>jouer(L,b));
      row.appendChild(b);
    }
    $('clavier').appendChild(row);
  }
}
function dessinerMot(reveal){
  const box=$('mot');box.innerHTML="";
  for(const c of mot){
    if(c===" "){const g=document.createElement('span');g.className="gap";box.appendChild(g);continue;}
    if(c==="-"||c==="'"){const s=document.createElement('span');s.className="sep";s.textContent=c;box.appendChild(s);continue;}
    const slot=document.createElement('span');slot.className="slot";
    if(devinees.has(norm(c))){slot.classList.add('filled');slot.innerHTML=`<span class="ch">${c}</span>`;}
    else if(reveal){slot.classList.add('miss');slot.textContent=c;}
    box.appendChild(slot);
  }
}
function nouvellePartie(){
  const cats=Object.keys(BANQUE);
  cat=cats[Math.floor(Math.random()*cats.length)];
  const liste=BANQUE[cat];
  mot=liste[Math.floor(Math.random()*liste.length)];
  devinees=new Set();erreurs=0;fini=false;
  $('cat').textContent=cat;
  $('msg').textContent="";$('msg').className="msg";
  figure.classList.remove('swing','lost','worried');
  $('bolt').classList.remove('zap');$('sun').classList.remove('burst');
  $parts.forEach(p=>p.classList.remove('pop'));
  setScene(0);coeurs();clavier();dessinerMot(false);
}
function motTrouve(){return [...mot].every(c=>!estLettre(c)||devinees.has(norm(c)));}

function jouer(L,btn){
  if(fini||devinees.has(L))return;
  devinees.add(L);btn.disabled=true;
  const present=[...mot].some(c=>estLettre(c)&&norm(c)===L);
  if(present){
    btn.classList.add('bon');dessinerMot(false);
    if(motTrouve())gagner();
  }else{
    btn.classList.add('faux');erreurs++;coeurs();setScene(erreurs);
    if(erreurs>=4)figure.classList.add('worried');
    const p=$parts.find(p=>+p.dataset.step===erreurs);
    if(p)p.classList.add('pop');
    if(erreurs>=MAX)perdre();
  }
}
function gagner(){
  fini=true;serie++;$('serie').textContent="série "+serie;
  $('msg').innerHTML="Bravo, sauvé ! 🎉";$('msg').className="msg win";
  setScene(0);
  const sun=$('sun');sun.classList.remove('burst');void sun.offsetWidth;if(!reduce)sun.classList.add('burst');
  confetti();
}
function perdre(){
  fini=true;serie=0;$('serie').textContent="série 0";
  setScene(MAX);figure.classList.add('lost');
  const f=$('flash');f.classList.remove('go');void f.offsetWidth;
  const b=$('bolt');b.classList.remove('zap');void b.offsetWidth;
  if(!reduce){f.classList.add('go');b.classList.add('zap');figure.classList.add('swing');}
  dessinerMot(true);
  $('msg').innerHTML="Perdu… le mot était <b>"+mot+"</b>";$('msg').className="msg lose";
}

function confetti(){
  if(reduce)return;
  const cv=$('confetti'),W=cv.clientWidth,H=cv.clientHeight;cv.width=W;cv.height=H;
  const ctx=cv.getContext('2d');
  const cols=['#ffd45e','#e8604c','#2fae6b','#3b6ea5','#ff8fb3','#ffffff'];
  const N=140,P=[];
  for(let i=0;i<N;i++)P.push({x:Math.random()*W,y:-10-Math.random()*H*.4,vx:(Math.random()-.5)*3,
    vy:2+Math.random()*3,s:5+Math.random()*6,rot:Math.random()*6,vr:(Math.random()-.5)*.4,c:cols[i%cols.length]});
  let t=0;(function loop(){t++;ctx.clearRect(0,0,W,H);
    for(const p of P){p.vy+=.06;p.x+=p.vx;p.y+=p.vy;p.rot+=p.vr;
      ctx.save();ctx.translate(p.x,p.y);ctx.rotate(p.rot);ctx.globalAlpha=Math.max(0,1-t/175);
      ctx.fillStyle=p.c;ctx.fillRect(-p.s/2,-p.s/2,p.s,p.s*.6);ctx.restore();}
    if(t<175)requestAnimationFrame(loop);else ctx.clearRect(0,0,W,H);})();
}

document.addEventListener('keydown',e=>{
  const L=norm(e.key);
  if(L.length===1&&L>="A"&&L<="Z"){
    const b=$('clavier').querySelector('[data-l="'+L+'"]');
    if(b&&!b.disabled)jouer(L,b);
  }
});
$('rejouer').addEventListener('click',nouvellePartie);
nouvellePartie();
</script>
</body>
</html>
