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
<div v-click class="!text-2xl">Esimerkkejä:

* HTML Status code 2xx / Xxx
* Tavoitevasteaika / Kaikki vasteajat
* Onnistuneet ostotapahtumat / Kaikki ostotapahtumat

</div>
---

<img src="../images/availabilitymathwithlauri.svg" class="block mx-auto h-auto max-h-[30rem] max-w-full object-contain" alt="Availability math" />

---

## Mites ne muut seinät?

<div v-click class="!text-2xl"> - QA ja tietoturva
</div>
<div v-click class="!text-2xl"> - Shift-left 
<img src="../images/shiftleft.png">
</div>

<div v-click class="!text-2xl"> - Myös: DevSecOps </div>
---

## Miten sitten?

<div class="relative h-0">
  <div v-click.hide class="absolute inset-x-0 -top-10 text-center text-[12rem] font-bold leading-none opacity-80">?</div>
</div>

<v-after>

- Kulttuurinen muutos
  - Laatu sisällytetty jo suunnitteluvaiheeseen
- Test-driven Development (TDD)
- CI/CD putket
- Moniosaajatiimit (Cross-functional teams)
  - Avainhenkilöt mukana koko prosessin ajan

</v-after>

---

## Measuring DevOps
