# FACTS.md — write-upin numeroledger (B, 2026-08-05)
*Jokainen numero jonka draft mainitsee TÄYTYY löytyä tästä taulukosta täsmälleen tässä muodossa,
tai se on lisättävä tänne provenanssin kanssa ENNEN käyttöä. provenance_audit ajetaan draftia
vasten tätä ledgeriä vastaan — numero ilman riviä täällä = automaattinen FAIL.*

| # | Väite | Arvo | Provenanssi |
|---|---|---|---|
| F1 | Session-CV log loss (paras malli, GroupKFold) | 0.5462 | OOF-ajo, memory: measured chain |
| F2 | Sama malli, REAL test log loss | 0.6162 | submissio "complex" 2026-08-02 |
| F3 | Base-rate-vakion (0.7025) real-test score | 0.6238 | base-rate-probe-submissio 08-02 |
| F4 | Siitä johdettu test-positiiviosuus | ~0.685 (0.68496) | F3:sta käänteisesti; re-derivointi −0.08256 vs −0.08224 |
| F5 | Train-positiiviosuus | 0.702469 (4dp 0.7025 = F3:n vakio) | train_labels_44ujmj2.csv, n=35,072, laskettu 2026-08-05 (A); F3/F5-ristiriita purkautui — sama luku eri tarkkuuksilla |
| F6 | Test n | 10,508 | submission_format-rivit |
| F7 | Train n | 35,072 | train_features-rivit (EI test-n — vanha virhe) |
| F8 | PROXY→test-gapin dekompositio: set difficulty | +0.01431 (63.8 %) | h3_findings.md §2; HUOM dekomponoitu suure on proxy 0.59376 → test 0.6162 = +0.02244, EI koko CV(0.5462)→test-gap |
| F9 | — session composition (aitoa skilliä) | +0.00951 (42.4 %) | h3_findings.md §2; osuudet ylittävät 100 % koska taulussa on negatiivisia termejä |
| F10 | — kontaminaatio = warm-row +0.00432 + fold-ID-leak +0.00230 | +0.00662 (29.5 %) | h3_findings.md §2, identiteetti asserted 1e-9 h3_gap.py |
| F11 | Leader (LB #1) | 0.5964 | leaderboard 08-02 |
| F12 | Oma rank | 137 | leaderboard 08-04 |
| F13 | Top-15 cutoff | 0.6044 | leaderboard 08-04 |
| F14 | Alijäämä leaderiin | +0.0198 | F2−F11; HUOM: ALIJÄÄMÄ, log loss pienempi voittaa |
| F15 | Alijäämä = k × kokonaismittausvirhe | 3× | h3: deficit 0.0198 vs total measurement error |
| F16 | Champion-ensemble vs `complex` clean rulerilla | +0.00339, CI [−0.00194,+0.00812] → EI eroa | h3; "9.3σ" on kielletty luku |
| F17 | Scrambled-label-selektionulli ylittää kandidaatin | 32.5 % permutaatioista | hive-5 08-03 |
| F18 | Objective-difficulty-oraakkelin arvo | +0.05367 skill | hive-4 ceiling 08-03 |
| F19 | Leaderin implikoitu text→difficulty-mappi | r≈0.53; oma paras r=0.255 | hive-4 |
| F20 | Kaikki palkinnot top-15-rajattuja (ml. 9×$2K pub bonus) | sääntösitaatti | rules page, verifioitu 08-05 |
| F21 | 2*SE OOF-vertailuille | ≈0.0043 | OOF-ketju; HUOM pätee train-OOF:iin, EI real-testiin |
| F22 | ECE (kalibrointi) | ≤0.009 | eval.py |
| F23 | Transcript-liftin dominoiva feature | mean_student_len (perm. importance 0.0068) | transcript_features |

| F24 | Submissiokiintiö | 3 scored / viikko (smoke-testit eivät kuluta) | platform page/6, verbatim muistissa |
| F25 | Scored-slotteja jäljellä koko kisassa | ~9–12 | johdettu 08-04 (ikkunat 08-09/08-16/08-23 × 3) |
| F26 | RATKAISTU: train-raten tarkka arvo | 0.702469 (24 637/35 072) — vakio 0.7025 = tämä pyöristettynä 4 desim.; draftin rinnastus "0.7025 = train base rate" PITÄÄ. F5:n "0.702" = 3 desim. pyöristys samasta luvusta | laskettu train_labels-datasta 08-05 (B) |

| F27 | Test set EI kokonaan Third Space Learningistä (multi-source shift) | organizer-vahvistus | kwetstone, kilpailufoorumi (SSO-portattu, luettu 08-04) — sanamuoto draftissa: "confirmed by the organizers on the competition forum", EI "documented"/"publicly" |
| F28 | Paras mahdollinen vakiofloor testissä (test-raten entropia H(q)) | 0.62304 (kanoninen, h3_findings/h3_gap.py) | B:n riippumaton lasku pyöristetystä q:sta antoi 0.62307 — ero 3e-5 on ankkurin (0.6238, 4dp) pyöristysepävarmuuden ±5e-5 sisällä. DRAFTISSA korkeintaan "about 0.623" (3 desim.) — 4. desimaali ei ole ankkurin tarkkuudella tuettu. HUOM ≠ F3:n 0.6238 = TRAIN-raten vakion testiscore |
| F29 | Train-sisäinen entropiafloor | 0.60876 | laskettu F5:stä 08-05 (B); täsmää OOF-ketjun "base rate 0.6088" -riviin |

| F30 | Clean-ruler-proxy (ensemble) | 0.59376 | h3_findings.md §2; naiivi session-CV 0.5462 → proxy-korjaus = leak+warm-korjaus |
| F31 | Proxy→test-gap yhteensä | +0.02244 (identiteetti, loput rivit: honest cold-row skill −0.00114, minus skill-we-had −0.00687) | h3_findings.md §2 taulukko |
| F32 | Real transferred skill testissä: oma / leader | +0.00687 / +0.02667 (3.9x) | h3_findings.md §2; yksi observaatio, EI error baria — kirjoita varauksella |
| F33 | Like-for-like-varaus | complexin oma proxy 0.59861 → gap +0.01759; johtopäätökset säilyvät | h3_findings.md §2 caveat |

| F34 | Warm/cold-anatomia: warm-rivien base rate 0.7626 vs cold 0.6365; yhden vastauksen sessio ei voi olla warm (0/14,457); observable rps>1-split toistaa 101.1% composition-termistä, pelkkä rps-sarake 75.5% | session composition = havaittava kovariaatti, EI artefakti | h3_findings.md §2 keskirivi + adversarial re-test 08-03 |

| F35 | JÄÄDYTETTY slot2-ennuste (81-feature, hidden test) | **envelope [0.6168, 0.6216]**, keskus ~0.619 | Kaksi riippumatonta reittiä 08-05 ~06:08: (A) ankkuriruler-proxy 0.599204 + havaittu Δ 0.020015 → 0.619219 [0.616794, 0.621644]; (B) clean-cold 0.657552 + n=2 cold-Δ −0.0401±2e-4 → [0.617361, 0.617551]; reitit leikkaavat. Route-B:n kapeus = Δ-yhtäpitävyys, EI ennustevarmuus; n=2 samaa perhettä → range, ei CI. SHA:t: h81_oof.py 0f06dad5…, .h81_oof.npy 55580c9a…, .h81_oof_faithfulobj.npy 3497b5f7…, h81_oof_anchor.py b368b46d…, .h81_oof_anchor.npy 9c811e6e…, .h81_oof_anchor_constobj.npy d7869a3c… (täydet h81_oof_result.md:ssä). Esirekisteröity ENNEN 08-09-ikkunaa; tulos kirjataan samaan tauluun kummin päin tahansa |
| F36 | RECHECK-TRIGGERI: ch3:n "seventeen thousandths" | pätee 3dp-ankkureilla (0.702−0.685=0.017) | JOS kumpikin ankkuri joskus restataan 4dp:nä, erotus 0.702469−0.68496=0.01751 pyöristyy 0.018:aan → sana päivitettävä. Tarkkuussäännön muutosvahti (B, 08-05) |

| F37 | h12: 5-variantin LLM-päättelysyvyys-AUROC-vaihteluväli (paikallinen Qwen2.5-0.5B, objektitaso n=398) | 0.469–0.531 | h12_reasoning_depth.py + inline-analyysi, data/h12_reasoning_depth.csv, 08-08 |
| F38 | h12: varianttien välinen ristikorrelaatio | 0.74–0.91 | sama ajo |
| F39 | h12: korrelaatio empiiriseen vaikeuteen (5 varianttia) | r=−0.031…+0.074 | sama ajo |
| F40 | h12: 5-variantin ensemble AUROC / korrelaatio | 0.5097 / 0.077 | sama ajo |
| F41 | h13: Kimi-CoT-otos, käyttökelpoinen/yritetty | 13/20 (7 menetetty token-budjettiin, ei sisällölliseen syyhyn) | h13_kimi_reasoning_depth.py, 08-08 |
| F42 | h13: Spearman(Kimi complexity, empiirinen vaikeus) | −0.102 (p=0.740) | sama ajo, n=13 |
| F43 | h13: Spearman(Kimi steps, empiirinen vaikeus) | −0.047 (p=0.880) | sama ajo, n=13 |
| F44 | h13: "has_near_miss_trap"=True-osuus | 100 % (13/13, nollavarianssi) | sama ajo |
| F45 | h14: ennustehyöty, baseline vs +LLM-kompleksisuus (5-fold GroupKFold, OOF-objektivaikeus) | baseline LL=0.5550±0.0059 AUC=0.7056±0.0052; +complexity LL=0.5549±0.0062 AUC=0.7055±0.0054; Δlog-loss=+0.00011 (SE~0.00013) | h14_predictive_utility.py, 08-08 |
| F46 | h15: yksi-vastaus-session-populaatio joilla piirteet saatiin | 7576/14457 (63.3 % sessioista / 41.2 % riveistä on yksi-vastaus-sessioita; piirteet onnistuivat 52.4 %:lle näistä) | h15_response_latency.py, 08-08 |
| F47 | h15: raakakorrelaatiot is_correctiin | latency r=+0.0033 p=0.7711; answer_len r=−0.0248 p=0.0311; chars/s r=+0.0067 p=0.8475 (n=832) | sama ajo, n=7576 ellei toisin |
| F48 | h15: ennustehyöty (sama h14-kuri) | baseline LL=0.6109±0.0135 AUC=0.6940; +latency LL=0.6112±0.0131 AUC=0.6930; Δlog-loss=−0.00029 (SE~0.00024, väärä suunta) | sama ajo |
| F49 | h16: kirjoitusvirhe-osuuden raakakorrelaatiot | kaikki r=−0.0191 p=0.0965 (n=7573); ≥3 sanaa r=−0.0146 p=0.4574 (n=2581); ≥5 sanaa r=−0.0167 p=0.5631 (n=1208) | h16_typos.py, 08-08 |
| F50 | h16: ennustehyöty (n=2581, ≥3 alfasanaa) | baseline LL=0.6384±0.0293 AUC=0.6790; +typo LL=0.6388±0.0296 AUC=0.6791; Δlog-loss=−0.00044 (SE~0.00057, väärä suunta) | sama ajo |
| F51 | hive-4: oraakkeli oppilaan kyvykkyydestä session MUIDEN vastausten perusteella | ~0.000 (ei tallennettua tarkkaa desimaalia — approksimaatio muistissa) | hive-4 ceiling analysis 08-03, ks. F18:n rinnakkaisluku +0.05367 (objektivaikeus-oraakkeli) |
| F52 | Uniikkien learning_objective_id:iden lukumäärä | 398 | train_features-datan `learning_objective_id.nunique()`, sama luku toistuu koko projektin muistissa (mm. h12/h13:n otantapohja) |
| F53 | Yksi-vastaus-sessioiden lukumäärä (ch2:n "zero of 14,457" -väitteen tausta) | 14,457 / 22,821 sessiota (63.3 %), 14,457 / 35,072 riviä (41.2 %) | TAKAUTUVASTI LISÄTTY 08-08 -auditissa: ch2 (08-05) käytti lukua ilman omaa ledger-riviä; itsenäisesti uudelleenlaskettu 08-08 h15_response_latency.py:n ajossa ("testing on 14457 single-response-session rows (41.2% of all data)") — sama luku, riippumaton toistolasku vahvistaa alkuperäisen väitteen |
| F54 | **[SCORE] slot1: ensemble (δ=0) hidden-test log loss** | **0.6141** | Normal submission 2026-08-09, job **id-4351**, platform submissions page (auto_submit.py SUBMITTED_SCORE 0.6141 id-4351; uuden-id:n tarkistus läpäisty — id ei ollut sivun before-joukossa). Bundle sha256 288db042… verifioitu ennen lähetystä = F35-taulun slot1-identiteetti. Prereg-ennuste 0.613 [0.607, 0.619] (PREREGISTERED.md, jäädytetty 08-04/08-05) → OSUMA. Konttiloki (View log id-4351) EI vielä luettu — kirjattu tähän alustan omasta score-taulusta |
| F55 | **[SCORE] slot2: 81-feature hidden-test log loss** | **0.6146** | Normal submission 2026-08-09, job **id-4352**, platform submissions page (auto_submit.py SUBMITTED_SCORE 0.6146 id-4352; uuden-id:n tarkistus läpäisty). Bundle submission/submission.zip sha256 5f479f9c… verifioitu ennen lähetystä = F35:n jäädytetty artefakti. F35-envelope [0.6168, 0.6216] → **OHITUS, alapuolelta** (parempi kuin ennuste; log lossissa pienempi = parempi) |
| F56 | Johdetut etäisyydet 08-09-tuloksista (peruslaskenta F54/F55/F35-riveistä) | slot2−slot1 = 0.6146−0.6141 = **+0.0005**; etäisyys envelopen alareunaan 0.6168−0.6146 = **0.0022**; reitti-A-keskus 0.619219−0.6146 = **0.0046**; reitti-B-keskus ((0.617361+0.617551)/2=0.617456)−0.6146 = **0.0029** → molemmat reitit yliarvioivat degradaation, reitti B (clean-cold) lähempänä | aritmetiikka näkyvissä; lähdeluvut F35 (reittien arvot), F54, F55 |
| F57 | CORRECTIONS.md:n numeroitujen korjausten lukumäärä | **25** (numerot 1–25) | laskettu 08-09 `grep "^\*\*[0-9]*\." CORRECTIONS.md` — raaka osumamäärä 27 sisältää 2 väärää positiivista (rivinalkuiset "9.3σ" ja "52.3%" -literaalit), numerosarja on 1–25 aukoton |

## Kiellettyjen väitteiden lista (audit FAILaa jos draft sisältää)
- "9.3σ" missään muodossa (F16 kumoaa)
- "lead/johto" suhteessa leaderiin (F14: alijäämä)
- rate-väite "pari tuottaa enemmän korjauksia kuin yksin" (ei solo-baselinea — VISIONS.md V2)
- gain +0.0625 ilman rinnalla F2−F3-real-test-vastinetta (+0.0076)
- mikään train-OOF-SE sovellettuna real-test-eroihin (F21 huomautus)

## Auditin ajotapa (t2, B)
1. Poimi draftin kaikki numeeriset literaalit.
2. Jokainen täsmää ledgerin riviin TAI on peruslaskutoimitus ledgerin riveistä TAI FAIL.
3. Kiellettyjen väitteiden grep.
4. measurement_audit.py niille väitteille joissa on predictions+labels-pari.
