# Midea M-Thermal Arctic 8 kW – tööloogika optimeerimise projekt

## 1. Projekti eesmärk

**KAKS TÖÖREZHIIMI** (manuaalse ümberlülitusega)

### <img src="files/euro.png" alt="settings" width="16" height="16"> <img src="files/euro.png" alt="settings" width="16" height="16"> <img src="files/euro.png" alt="settings" width="16" height="16"> Kokkuhoiu rezhiim <img src="files/euro.png" alt="settings" width="16" height="16"> <img src="files/euro.png" alt="settings" width="16" height="16"> <img src="files/euro.png" alt="settings" width="16" height="16">
  - nö. soojemate ilmadega, **−12 °C välistemperatuurini**
  - "**<span style="color:red">Curve 3 + Offset 12</span>**"
  - −12 °C juures on see **target 53.3 °C** (vt. graafik allpool)
  - mis peaks olema veel "odavama elektrikulu" piirkonnas
  - toetuda võimalikult kompressori tööle
  - elektriline lisaküte asub osalema alles siis, kui hädavajalik
  - aga siiski mitte ka kompressorit hulluks ajada, leida minimaalse elektritarbimise kombinatsioon (parameetrite abil, vt. allpool)

### <img src="files/cold.png" alt="settings" width="16" height="16"> <img src="files/cold.png" alt="settings" width="16" height="16"> <img src="files/cold.png" alt="settings" width="16" height="16"> Krõbeda külma rezhiim <img src="files/cold.png" alt="settings" width="16" height="16"> <img src="files/cold.png" alt="settings" width="16" height="16"> <img src="files/cold.png" alt="settings" width="16" height="16">
  - külmem kui **−12 °C välistemperatuur**
  - **<span style="color:red">manuaalselt target 60 °C</span>** peale
  - "OK, päev on pakaseline, kuluta rohkem energiat/raha"
  - kui külmad möödas, siis manuaalselt tagasi "Curve 3 + Offset 12" peale

![img.png](img.png)

## 2. Olulised parameetrid

(maaletooja sõnastus)

Nende settingutega saab määrata mis hetkeni töötab kompressoriga ja elektritenni kasutamise loogika:

| Parameeter    | Kirjeldus                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------|
| `T4HMAX`      | Välisõhutemperatuur, millest alates töötab madalal võimsusel või elektri/välise allikaga.                           |
| `T4HMIN`      | Välisõhutemperatuur, millest alates kütet enam ei tooda ja elektritenn võtab üle.                                   |
| `dT1_IBH_ON`  | Elektritenni sisselülitus hüsterees soovitud väljuva vee temperatuuri ja hetke väljuva vee temperatuur delta järgi. |
| `t_IBH_Delay` | Ajaline viide kompressori sisselülitus hetkest, millal võib vajadusel elektritenni sisse lülitada.                  |
| `T4_IBH_ON`   | Välisõhutemperatuur, millest alates on lubatud elektritenni võimsuse puudujäägi korral lülitada.                    |

Leitavad ka manuaalist (kuigi ilma kirjelduseta):
- [https://www.emhbt.com/Manual/controller/controller-2024/kjrh-120l2/operation-manualsmarthome](https://www.emhbt.com/Manual/controller/controller-2024/kjrh-120l2/operation-manualsmarthome)
- (kontroller `KJRH-120L2`)

## 3. Hetke ideed parameetritega

ja (arvatavad) tehase seadistused (TS)

| Parameeter    | TS     | Idee                                                                                 |
|---------------|--------|--------------------------------------------------------------------------------------|
| `T4HMAX`      | ?      | Ei muuda eriti asja (võimalik valida vahemikus 20-35 kraadi)                         |
| `T4HMIN`      | -15 °C | <span style="color:red">Langetada -25 °C peale</span>                                |
| `dT1_IBH_ON`  | 5 °C   | Jätta 5 °C (võimalik valida 2-10 kraadise hüstereesi vahel)                          |
| `t_IBH_Delay` | 30 min | Jätta 30 min (võimalik valida 15-120 min)                                            |
| `T4_IBH_ON`   | -5 °C  | <span style="color:red">Langetada -13 °C peale</span> (võimalik valida +10 kuni -15) |


## 4. Parameetrite valiku loogika

### `dT1_IBH_ON = 5 °C`
- Väldib elektritenni liiga varajast sekkumist.
- Annab kompressorile võimaluse sagedust tõsta.
- Sobib radiaatorkütte inertsiga.

### `t_IBH_Delay = 30 min`
- Väldib lühiajalistest kõikumistest tulenevat lisakütte aktiveerimist.
- Säilitab kompressori prioriteedi.

### `T4_IBH_ON = -13 °C`
- Elektritenni osalus ei ole lubatud enne kui on -13 °C.

### `T4HMIN = -25 °C`
- Tagab, et kompressor töötab võimalikult madala välistemperatuurini.
- Väldib olukorda, kus süsteem lülitub liiga vara puhtalt elektrirežiimi.

### `T4HMAX`
- Mõjutab ülemist välistemperatuuri piiri, kus süsteem võib lülituda teisele loogikale.
- Ei ole kriitiline talvises tööpiirkonnas, kuid vajab kinnitust sobiva väärtuse osas.


## 5. Hoone

- Hoone: ~150 m² kahekorruseline puitmaja
- Soojusjaotus: radiaatorid
- Tunnetuslik piir:
  - kuni ~-12 °C välisõhutemperatuurini piisab ~53 °C kütteveest
  - alla -12 °C vajab hoone ~58–60 °C
