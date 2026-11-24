---
layout: center
---

# DevOps

---

## Mitä kuuluu ohjelmistokehitysprosessiin?

<LiveTimeline storage-key="ohjelmistokehitysprosessi" initial="Koodin kirjoittaminen" class="mt-4" />

<!--
Kysy yleisöltä ideoita. Tuplaklikkaa tyhjää kohtaa (tai "+" oikeassa alakulmassa) lisätäksesi vaiheen,
raahaa laatikoita mihin tahansa, tuplaklikkaa laatikkoa muokataksesi, × poistaa, ↺ palauttaa alkutilaan.
-->

---

## Prosessin sidosryhmät

<LiveTimeline storage-key="prosessin-sidosryhmat" initial="Devitiimi" class="mt-4" />

<!--
Kysy yleisöltä ideoita. Tuplaklikkaa tyhjää kohtaa (tai "+" oikeassa alakulmassa) lisätäksesi sidosryhmän,
raahaa laatikoita mihin tahansa, tuplaklikkaa laatikkoa muokataksesi, × poistaa, ↺ palauttaa alkutilaan.
-->

---

## Muistutus: Agilen palautesilmukka

<img src="../images/agile-feedback-loop.png" class="max-h-100 mx-auto" alt="Agile feedback loop: Plan, Build, Test and demo, Feedback" />

---

## Handoverit

<LiveTimeline storage-key="prosessin-sidosryhmat" initial="Devitiimi" class="mt-4" />

<!--
Sama tilanne kuin "Prosessin sidosryhmät" -dian: samat laatikot ja samat muutokset, koska molemmat käyttävät samaa tallennusavainta.
-->

---

## Mitä on DevOps?

- Andrew Schafer ja Patrick Debois käyttivät termiä ensimmäisen kerran 2008–2009
- DevOps on alunperin kulttuuri, filosofia ja joukko käytäntöjä, jotka yhdistävät ohjelmistokehityksen (Dev) ja
  IT-toimintojen (Ops) tiimit
- Mielestäni käsitettä voidaan laajentaa myös muuhun IT-organisaation osiin.

---

## Periaatteet

<v-clicks>

- Siilojen purkaminen
    - Yhteistyö ja yhteinen vastuu
- Työnkulun tehostaminen
    - Automaatio, CI/CD, Self-service
- Nopea palaute
    - Inkrementaalinen kehitys, jatkuva parantaminen, yhteiset mittarit
- Kokeilukulttuuri
    - Epäonnistumisen salliminen, nopea palautuminen, korkea luottamus
</v-clicks>

<h1 v-click class="text-center underline mt-12"> Gene Kim: The Three Ways </h1>

<!--
Nopea palautuminen toistoilla. 20% ajasta non-functional tekemiseen.
-->
---

## SRE

- Site Reliablility Engineering
- Google kehitti SRE:n 2003, kun heillä oli tarve hallita suuria järjestelmiä.
- Konsepteja:
    - SLA, SLO, SLI ja virhebudjetit
    - Toil reduction
    - Monitorinti ja mittaaminen
    - Blameless postmortem ja psykologinen turvallisuus

---

## Klassinen konsepti: DevOps Wall of Confusion

<div class="w-full mt-2">
   <img src="../images/wall-of-confusion.png" class="block w-full h-auto max-h-[19rem] object-contain" alt="Wall of confusion" />
  <div class="grid grid-cols-2 gap-8 mt-3">
    <div v-click class="min-h-24 w-full rounded-lg border-2 border-current bg-white p-3 text-lg text-black">Devaaja haluaa julkaista mahdollisimman paljon ominaisuuksia.</div>
    <div v-click class="min-h-24 w-full rounded-lg border-2 border-current bg-white p-3 text-lg text-black">Operaatioäijä haluaa pitää järjestelmän vakaana.</div>
  </div>
</div>

---

## You build it, you run it

<div class="w-full mt-2">
   <img src="../images/youbuildityourunit.png" class="block w-full h-auto max-h-[19rem] object-contain" alt="Wall of confusion" />
  <div class="grid grid-cols-2 gap-8 mt-3">
    <div v-click class="min-h-24 w-full rounded-lg border-2 border-current bg-white p-3 text-lg text-black">- Alunperin Amazonilta 2026. <br>
- Vaatii monitaitoisen tiimin (Cross-functional).
</div>
    <div v-click class="min-h-24 w-full rounded-lg border-2 border-current bg-white p-3 text-lg text-red">Vaara: <br> - Skaalautuminen? <br> - Devitiimin aika kuluu muuhun, kuin devaamiseen.</div>
  </div>
</div>

---

## Virhebudjetit

<div class="w-full mt-2">
   <img src="../images/errorbudgets.png" class="block mx-auto h-auto max-h-[20rem] max-w-full object-contain" alt="Error budgets" />
  <div class="grid grid-cols-2 gap-6 mt-2">
    <div v-click class="w-full rounded-lg border-2 border-current bg-white p-2 text-base text-black">- Devillä ja operaatioilla on yhteinen sopimus <br>
- Näin heillä on myös yhteinen tavoite, jonka onnistumista voidaan objektiivisesti seurata.
</div>
    <div v-click class="w-full rounded-lg border-2 border-current bg-white p-2 text-base text-red">- Vaatii hyvin toteutetun metriikan<br>
- En ole kovin usein nähnyt oikeassa elämässä.
</div>
  </div>
</div>

---
layout: image-left
image: ../images/slamath.png
backgroundSize: contain
---

## Virhebudjetit jatkuu 

<br>
<br>
<div v-click class="!text-2xl"> Esimerkkejä:

* HTML Status code 2xx / Xxx
* Tavoitevasteaika / Kaikki vasteajat
* Onnistuneet ostotapahtumat / Kaikki ostotapahtumat

<a href="https://vkolupaev.com/sre/img/availability-cheat-sheet.webp" target="_blank" class="text-blue-600 underline">Availability Cheat Sheet </a>
</div>

<!--
- slide 16, vois nostaa ehkä esimerkki luvuksi että AWS lupaa EC2 single-zone 99,95% ja multi-az arkkitehtuurille 99,99%, pätee kaikkiin muihinkin AWS compute palveluihin (harvalla multi-region oikeasti)
-->

---

<img src="../images/availabilitymathwithlauri.svg" class="block mx-auto h-auto max-h-[30rem] max-w-full object-contain" alt="Availability math" />

---

## Mites ne muut seinät?

<div v-click class="!text-2xl"> - QA ja tietoturva
</div>
<div v-click class="!text-2xl"> - Shift-left 
<img src="../images/shiftleft.png" class="block mx-auto max-h-[20rem] h-auto object-contain">
<div class="abs-bl m-6 text-sm opacity-60">Lähde: https://devopedia.org/shift-left/ </div>
</div>
<div v-click class="!text-2xl"> - Myös: DevSecOps </div>

---

## Miten sitten?

<div class="relative h-0">
  <div v-click.hide class="absolute inset-x-0 -top-10 text-center text-[12rem] font-bold leading-none opacity-80">?</div>
</div>

<div v-after class="!text-2xl">

- Kulttuurinen muutos
  - Laatu sisällytetty jo suunnitteluvaiheeseen
- Esim. Test-driven Development (TDD)
- CI/CD putket
- Moniosaajatiimit (Cross-functional teams)
  - Avainhenkilöt mukana koko prosessin ajan
  - Two-pizza team?

</div>

---

## Shift-right?  
<br>
<div v-click class="!text-2xl"> - Khyl! </div>
<br>

<div v-click> 
<img src="../images/Canaryandab.drawio.svg">
</div>

---

## Blue/Green deployment
<br>
<img src="../images/bluegreen.svg">

---
layout: image-right
image: ../images/Acceleratebook.jpg
backgroundSize: contain
---
## Prosessien mittaaminen

### DORA-mittarit (Accelerate, 2018)

- Lead time for changes
- Deployment frequency
- Mean time to restore (MTTR)
- Change failure rate

<v-clicks>

 - Parhaat tiimit suoriutuvat mittareilla hyvin ja eivätkä uhraa laatua tehokkuuden nimissä!
 - Huom! Korrelaatio vs kausaliteetti.
    - DORA-mittarit eivät mittaa esimerkiksi asiakastyytyväisyyttä, kehittäjäkokemusta yms.

</v-clicks>

---
layout: two-cols
---

## SPACE

::left::

- **S**atisfaction and well-being
- **P**erformance
- **A**ctivity
- **C**ommunication and collaboration
- **E**fficiency and flow

::right::

<div class="text-sm">

### DORA vs SPACE
| Aspect | DORA | SPACE |
|---|---|---|
| **Focus** | Software delivery performance | Developer productivity holistically |
| **Scope** | 4–5 specific metrics | 5 dimensions with many possible metrics |
| **Metrics** | Deployment frequency, lead time, change failure rate, time to restore | Varies by organization: satisfaction surveys, efficiency metrics, performance indicators, etc. |
| **Purpose** | Signal: "How are we doing?" | Diagnosis: "What should we improve?" |
| **Best for** | Benchmarking delivery capabilities | Understanding and improving developer experience |

</div>

<div class="abs-bl m-6 text-sm opacity-60">Lähde: space-framework.com</div>

<!--
S: Devaajien hyvinvointi. Työn imu, eNPS, psykologinen turvallisuus
P: Työn tulokset. Quality metrics, customer satisfaction
A: Työn tekeminen. Commitit, PR:t, buildit, deployt
C: Kommunikaatio, Dokumentaation laatu, onboarding, PR:n aukiolo.
E: Katkeamaton aika, Pipelinejen aika, WIP asioita kerrallaan.

-->

---
layout: image
image: ../images/devopsvenn.png
backgroundSize: contain
---