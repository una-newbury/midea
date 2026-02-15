# Midea M-Thermal Arctic 8 kW – tööloogika optimeerimise projekt

## 1. Projekti eesmärk

**KAKS TÖÖREZHIIMI** (manuaalse ümberlülitamisega)

### Kokkuhoiu rezhiim
  - nö. soojemate ilmadega,  **−12 °C välistemperatuurini**
  - "**<span style="color:red">Curve 3 + Offset 12</span>**" ([GRAAFIK](https://una-newbury.github.io/midea/))
  - −12 °C juures on see **target 53.3 °C**
  - mis senise kogemuse põhjal on veel "odavama elektrikulu" piirkonnas (plahvatus toimub kusagil 50 ja 60 vahel)

### Krõbeda külma rezhiim
  - külmem kui **−12 °C välistemperatuur**
  - **<span style="color:red">manuaalselt target 60 °C</span>** peale
  - "OK, päev on pakaseline, kuluta rohkem energiat/raha"
  - kui külmad möödas, siis manuaalselt tagasi "Curve 3 + Offset 12" peale

* nö. soojemate ilmadega,  **−12 °C välistemperatuurini**, töötaks süsteem nii, et ta ei üritaks ajada küttevee temperatuuri üle  **50–53 °C** (eeldatavasti nende eesmärk-kraadide juures elektrikulu veel ei lähe kõrgustesse).
* alates  **−12 °C välistemperatuurist** aga lülituks sisse max  **60 °C** ehk "kallis töörežiim" ehk "OK, päev on pakaseline, kuluta rohkem energiat/raha".
* ühesõnaga vältida olukorda, kus lisaküte ja/või kõrge küttevee temperatuur ja/või kompressori ebaoptimaalne töö domineerib juba mõõdukate miinuskraadide juures.

Seadistada õhk-vesi soojuspump nii, et:

- kompressor töötab maksimaalselt suure osa talvest iseseisvalt,
- elektriline lisaküte (IBH) osaleb alles siis, kui see on reaalselt vajalik,
- kõrge küttevee temperatuur (~60 °C) kasutatakse ainult pakaselistes tingimustes,
- süsteem töötab tootja ette nähtud tööpiirkonnas.

Oluline: eesmärk ei ole muuta 60 °C töörežiimi „odavaks“, vaid tagada, et see aktiveerub ainult siis, kui see on vältimatu sisekliima hoidmiseks.

---

## 2. Hoone ja kasutusloogika

- Hoone: ~150 m² kahekorruseline puitmaja
- Soojusjaotus: radiaatorid
- Empiiriline piir:
    - kuni ~-12 °C välisõhutemperatuurini piisab ~53 °C kütteveest
    - alla -12 °C vajab hoone ~58–60 °C

Sellest tulenevalt kasutatakse kahte teadlikult valitud töörežiimi:

### Režiim A – Tavaline talv (= -12 °C)

- Heating Curve 3 + Offset 12
- -12 °C juures ~53 °C
- 0 °C juures ~47 °C
- eesmärk: kõrge hooajaline kasutegur

### Režiim B – Pakane (< -12 °C)

- Manuaalne ümberlülitus konstantsele sihttemperatuurile 58–60 °C
- eesmärk: tagada sisekliima ka madalatel välistemperatuuridel

Küttekurvide interaktiivne visualiseerimine:
https://una-newbury.github.io/midea/

---

## 3. Süsteemi juhtloogika põhimõte

Eesmärk on saavutada järgmine tööjaotus:

| Välistemperatuur | Kompressor | Elektritenni roll | Sihttemperatuur |
|------------------|------------|------------------|----------------|
| = -12 °C         | Peamine    | Vältida          | Küttekõver     |
| < -12 °C         | Peamine + vajadusel abi | Lubatud | 58–60 °C |

Oluline on vältida olukorda, kus elektritenni osalus algab juba -5…-8 °C juures.

---

## 4. Parameetrite ülevaade ja ettepanekud

Allpool on koondatud paigalduskonfiguratsiooni parameetrid koos maaletooja tehniliste selgitustega.

| Parameeter | Kirjeldus (maaletooja sõnastus) | Tehase seadistus | Soovitus projektis |
|------------|----------------------------------|------------------|-------------------|
| `dT1_IBH_ON` | Elektritenni sisselülitus hüsterees soovitud väljuva vee temperatuuri ja hetke väljuva vee temperatuur delta järgi | 5 °C | 6 °C |
| `t_IBH_Delay` | Ajaline viide kompressori sisselülitus hetkest, millal võib vajadusel elektritenni sisse lülitada | 30 min | 25–30 min |
| `T4_IBH_ON` | Välisõhutemperatuur, millest alates on lubatud elektritenni võimsuse puudujäägi korral lülitada | -5 °C | -13 °C |
| `T4HMAX` | Välisõhutemperatuur, millest alates töötab madalal võimsusel või elektri/välise allikaga | (vahemik 20–35 °C) | 30 °C (kontrollitav) |
| `T4HMIN` | Välisõhutemperatuur, millest alates kütet enam ei tooda ja elektritenn võtab üle | -15 °C | -20…-25 °C |

---

## 5. Parameetrite valiku loogika

### `dT1_IBH_ON = 6 °C`
- Väldib elektritenni liiga varajast sekkumist.
- Annab kompressorile võimaluse sagedust tõsta.
- Sobib radiaatorkütte inertsiga.

### `t_IBH_Delay = 25–30 min`
- Väldib lühiajalistest kõikumistest tulenevat lisakütte aktiveerimist.
- Säilitab kompressori prioriteedi.

### `T4_IBH_ON = -13 °C`
- Elektritenni osalus ei ole lubatud temperatuuridel = -12 °C.
- Vastab hoone empiirilisele piirile.

### `T4HMIN = -20…-25 °C`
- Tagab, et kompressor töötab võimalikult madala välistemperatuurini.
- Väldib olukorda, kus süsteem lülitub liiga vara puhtalt elektrirežiimi.

### `T4HMAX`
- Mõjutab ülemist välistemperatuuri piiri, kus süsteem võib lülituda teisele loogikale.
- Ei ole kriitiline talvises tööpiirkonnas, kuid vajab kinnitust sobiva väärtuse osas.

---

## 6. Eeldatav töökarakteristik

### 6.1 Välistemperatuur vs küttevee temperatuur

Režiim A (Curve 3 + Offset 12):

| Välistemperatuur | Küttevee temp |
|------------------|---------------|
| 0 °C             | ~47 °C       |
| -5 °C            | ~50 °C       |
| -12 °C           | ~53 °C       |

Režiim B (manuaalne):

| Välistemperatuur | Küttevee temp |
|------------------|---------------|
| < -12 °C         | 58–60 °C      |

---

### 6.2 Välistemperatuur vs elektritarbimine (kontseptuaalne)

```

Elektritarbimine
¦
¦                 _________
¦                /
¦               /
¦              /
¦             /
¦____________/
-12°C        Välistemperatuur

```

- = -12 °C: kompressoripõhine, kõrgem COP
- < -12 °C: kõrgem sihttemperatuur ja vajadusel elektriline abi

---

## 7. Kinnitusküsimused

1. Kas kirjeldatud kahe-režiimi loogika on juhtloogika seisukohalt korrektne?
2. Kas Curve ? Fixed target ümberlülitus säilitab Offset väärtuse?
3. Kas soovitatud parameetrid on tootja loogika ja Eesti kliima kontekstis põhjendatud?
4. Kas on tootjapoolseid piiranguid, mis takistaksid sellise strateegia rakendamist?

---

## 8. Kokkuvõte

Käesolev projekt ei muuda süsteemi füüsikalisi piiranguid ega tööpiirkonda.  
Eesmärk on viia juhtloogika kooskõlla hoone tegeliku soojusvajadusega ning vähendada olukordi, kus elektriline lisaküte aktiveerub enneaegselt.

Projekt eeldab paigalduskonfiguratsiooni parameetrite teadlikku seadistamist ning kasutajapoolset hooajalist režiimivahetust.
