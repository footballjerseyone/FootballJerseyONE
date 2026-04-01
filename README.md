<!DOCTYPE html>
"Europa":["Deutschland","Frankreich","Spanien","England","Italien","Portugal","Niederlande","Belgien","Schweiz","Österreich"],
"Südamerika":["Brasilien","Argentinien","Uruguay","Kolumbien","Chile","Peru"],
"Afrika":["Nigeria","Marokko","Senegal","Ghana","Ägypten","Algerien","Tunesien","Kamerun"],
"Asien":["Japan","Südkorea","Saudi Arabien","Iran","Australien","Qatar","China"],
"Nordamerika":["USA","Mexiko","Kanada","Costa Rica","Jamaika"],
"Ozeanien":["Australien","Neuseeland"]
};

// ---------------- 10 LIGEN ----------------
const leagues={
"Premier League":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal","Tottenham","Newcastle","Aston Villa","West Ham","Brighton"],
"La Liga":["Real Madrid","Barcelona","Atletico Madrid","Sevilla","Valencia","Real Sociedad","Villarreal","Athletic Bilbao","Betis","Girona"],
"Bundesliga":["Bayern München","Dortmund","Leipzig","Leverkusen","Frankfurt","Stuttgart","Freiburg","Wolfsburg","Gladbach","Hoffenheim"],
"Serie A":["Juventus","Inter","AC Milan","Napoli","Roma","Lazio","Atalanta","Fiorentina","Bologna","Torino"],
"Ligue 1":["PSG","Marseille","Lyon","Monaco","Lille","Nice","Rennes","Lens","Nantes","Toulouse"],
"Eredivisie":["Ajax","PSV","Feyenoord","AZ Alkmaar","Utrecht","Twente","Sparta Rotterdam","Heerenveen","NEC","Willem II"],
"Liga Portugal":["Benfica","Porto","Sporting","Braga","Guimaraes","Boavista","Rio Ave","Famalicao","Casa Pia","Arouca"],
"Brasileirao":["Flamengo","Palmeiras","Sao Paulo","Corinthians","Fluminense","Botafogo","Gremio","Internacional","Atletico Mineiro","Santos"],
"Argentina Liga":["Boca Juniors","River Plate","Racing","Independiente","San Lorenzo","Velez","Estudiantes","Newells","Rosario Central","Huracan"],
"MLS":["Inter Miami","LA Galaxy","LAFC","New York City","Seattle Sounders","Atlanta United","Chicago Fire","Toronto FC","Columbus Crew","Orlando City"]
};

const retro=["Deutschland 1990","Brasilien 2002","Frankreich 1998","Barcelona 2009","Inter 2010"];

// ---------------- NAV ----------------
function go(p){location.hash=p;render();}
window.onhashchange=render;

function openCart(){document.getElementById('cartModal').style.display='flex';renderCart();}
function closeCart(){document.getElementById('cartModal').style.display='none';}

function add(name){cart.push({name});save();}
function renderCart(){
let b=document.getElementById('cartBody');
if(cart.length===0){b.innerHTML='Leer';return;}
b.innerHTML=cart.map((c,i)=>`<div class='cartItem'>${c.name}<button onclick='remove(${i})'>X</button></div>`).join('');
}
function remove(i){cart.splice(i,1);save();renderCart();}

// ---------------- RENDER ----------------
function render(){
updateCart();
const app=document.getElementById('app');
let h=location.hash.replace('#','')||'clubs';

if(h==='national'){
app.innerHTML='<div class="title">Nationalteams</div>'+Object.keys(national).map(c=>
`<div class='sub'>${c}</div><div class='grid'>${national[c].map(n=>`<div class='card'>${n}<br><button class='btn' onclick="add('${n} Heim')">Heim</button><button class='btn' onclick="add('${n} Auswärts')">Auswärts</button></div>`).join('')}</div>`).join('');
}

else if(h==='clubs'){
app.innerHTML='<div class="title">Top 10 Ligen</div><div class="grid">'+Object.keys(leagues).map(l=>`<div class='card' onclick="location.hash='league-'+l">${l}</div>`).join('')+'</div>';
}

else if(h.startsWith('league-')){
let l=h.replace('league-','');
app.innerHTML='<div class="title">'+l+'</div><div class="grid">'+leagues[l].map(t=>`<div class='card'>${t}<br><button class='btn' onclick="add('${t} Heim')">Heim</button><button class='btn' onclick="add('${t} Auswärts')">Auswärts</button></div>`).join('')+'</div>';
}

else if(h==='retro'){
app.innerHTML='<div class="title">Retro</div><div class="grid">'+retro.map(r=>`<div class='card'>${r}<br><button class='btn' onclick="add('${r}')">Kaufen</button></div>`).join('')+'</div>';
}
}

render();
</script>

</body>
</html>
