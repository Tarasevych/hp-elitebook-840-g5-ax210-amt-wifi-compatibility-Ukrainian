# HP EliteBook 840 G5 + Intel AX210 + AMT Wi-Fi: Compatibiliteitsonderzoek

**Documentatietalen:** [Українська (Oekraïens)](./README.md) | **Nederlands**

**Gerelateerde documenten:** [Publicatiemanifest](./МАНІФЕСТ-ПУБЛІКАЦІЇ.md) | [Publicatiestatus](./СТАТУС-ПУБЛІКАЦІЇ.md)

---

## Samenvatting van het onderzoek

Deze opslagplaats documenteert een praktisch compatibiliteitsonderzoek: in een HP EliteBook 840 G5 werd de oorspronkelijke draadloze netwerkmodule uit de Intel vPro-generatie (Intel 8265) vervangen door een moderne Intel Wi-Fi 6E AX210. Het besturingssysteem Windows accepteerde de nieuwe module probleemloos, maar de beheerfirmware Intel ME/AMT op het platform (HP Q78 / Intel CSME 11.8) herkende de AX210 niet als een draadloos apparaat voor beheer buiten het besturingssysteem om (vóór het opstarten van het besturingssysteem of in uitgeschakelde toestand).

### Hoofdconclusie

Het vervangen van een draadloze netwerkmodule kent twee afzonderlijke compatibiliteitslagen:
1. **Besturingssysteemlaag (Windows):** De Intel AX210 werkt uitstekend als reguliere Wi-Fi-adapter onder Windows (`VEN_8086&DEV_2725`).
2. **Beheerfirmwarelaag (Intel ME/AMT):** De AX210 wordt niet automatisch een ondersteunde draadloze AMT-interface binnen de firmware HP Q78 / Intel CSME 11.8, omdat de firmware zoekt naar een draadloos doelapparaat uit de 8265-klasse (`0x24FD`).

---

## Overzicht per compatibiliteitslaag

| Compatibiliteitslaag | Onderzoeksvraag | Resultaat in deze casus |
| --- | --- | --- |
| Besturingssysteem en WLAN | Herkent en gebruikt Windows de Intel AX210? | **Ja**, werkt volledig als reguliere Wi-Fi |
| Beheerfirmware (ME/AMT) | Kan Intel ME/AMT de AX210 gebruiken vóór of buiten het besturingssysteem? | **Niet bevestigd** door lokale bewijsstukken |
| Controleproef met Intel 8265 | Herkent ME/AMT de oorspronkelijke Intel 8265 op dezelfde laptop? | **Ja**, zichtbaar als draadloos apparaat `0x24FD` |
| Draadloos thuisprofiel (WPA2-Personal/PSK) | Werkt een standaard PSK-thuisprofiel in AMT 11.8 met de 8265? | **Nee**, profielaanmaak werd geweigerd in AMT 11.8 |
| Praktisch beheer op afstand onder Windows | Kan Windows veilig via Wi-Fi op afstand worden beheerd? | **Ja**, via een particulier VPN/Tailscale-netwerk en RDP |
| Beheer van BIOS en uitgeschakelde toestand | Hoe krijgt de eigenaar toch volledige controle zonder werkend besturingssysteem? | **Praktische route:** externe IP-KVM + stroombeheer (slimme stekker of schakelaar) |

---

## Praktische architectuur voor volledig beheer op afstand

In plaats van te proberen niet-ondersteunde firmware aan te passen voor de AX210, biedt een gelaagde architectuur directe en betrouwbare controle:

- **Windows-niveau:** Intel AX210 + particulier VPN (Tailscale) + extern bureaublad (RDP).
- **BIOS / vóór het besturingssysteem:** Externe IP-KVM (HDMI-beeldopname + USB-toetsenbord en -muis).
- **Uitgeschakelde toestand:** Slimme stekker met inschakeling bij stroomtoevoer of een fysieke knopbediening.

---

## Volledig overzicht van onderzoeksdocumenten (Oekraïens)

### Hoofddocumenten
- [`README.md`](./README.md) — Volledig hoofdartikel in het Oekraïens.
- [`МАНІФЕСТ-ПУБЛІКАЦІЇ.md`](./МАНІФЕСТ-ПУБЛІКАЦІЇ.md) — Publicatiemanifest en overzicht van de opslagplaats.
- [`СТАТУС-ПУБЛІКАЦІЇ.md`](./СТАТУС-ПУБЛІКАЦІЇ.md) — Publicatiestatus en verificatiegegevens.

### Technische rapporten en bewijsstukken (`docs/`)
- [`docs/звіт-сумісності.md`](./docs/звіт-сумісності.md) — Volledig technisch compatibiliteitsrapport.
- [`docs/підсумок-доказів.md`](./docs/підсумок-доказів.md) — Gezuiverde samenvatting van de bewijsstukken.
- [`docs/очищений-пакет-доказів.md`](./docs/очищений-пакет-доказів.md) — Gezuiverd bewijspakket (zonder privégegevens).
- [`docs/матриця-сумісності.md`](./docs/матриця-сумісності.md) — Compatibiliteitsmatrix van modules en routes.
- [`docs/журнал-дослідів.md`](./docs/журнал-дослідів.md) — Logboek van uitgevoerde experimenten (E01–E09).
- [`docs/закриття-випадку.md`](./docs/закриття-випадку.md) — Eindconclusie en afsluiting van het onderzoek.
- [`docs/практичний-маршрут.md`](./docs/практичний-маршрут.md) — Praktische ondersteunde toegangsroute (VPN/RDP + IP-KVM).
- [`docs/підтримувана-рамка-дослідження.md`](./docs/підтримувана-рамка-дослідження.md) — Ondersteund onderzoekskader en veiligheidsgrenzen.
- [`docs/межі-ші-допомоги-в-ремонті.md`](./docs/межі-ші-допомоги-в-ремонті.md) — Grenzen van AI-ondersteuning bij hardware- en firmwarevraagstukken.

### Experimentverslagen (`docs/`)
- [`docs/ax210-базова-перевірка-2026-07-09.md`](./docs/ax210-базова-перевірка-2026-07-09.md) — Basiscontrole van Intel AX210 onder Windows en ME/AMT.
- [`docs/8265-контроль-2026-07-09.md`](./docs/8265-контроль-2026-07-09.md) — Controleproef met het oorspronkelijke Intel 8265-model.
- [`docs/8265-інтерпретація-контролю-2026-07-09.md`](./docs/8265-інтерпретація-контролю-2026-07-09.md) — Interpretatie van de controleproef met de Intel 8265.
- [`docs/8265-спроба-amt-wifi-досяжності-2026-07-09.md`](./docs/8265-спроба-amt-wifi-досяжності-2026-07-09.md) — Test van draadloze AMT-bereikbaarheid met de Intel 8265.
- [`docs/8265-обмеження-wpa2-personal-psk-2026-07-09.md`](./docs/8265-обмеження-wpa2-personal-psk-2026-07-09.md) — Beperking van WPA2-Personal/PSK in AMT 11.8.

### Samenwerking en reproductie (`docs/`)
- [`docs/запит-до-виробника.md`](./docs/запит-до-виробника.md) — Formeel verzoek aan HP en Intel voor duidelijke compatibiliteitsdocumentatie.
- [`docs/критерії-виробничого-рішення.md`](./docs/критерії-виробничого-рішення.md) — Criteria voor een aanvaardbare oplossing van de fabrikant.
- [`docs/план-доказів-для-захисту.md`](./docs/план-доказів-для-захисту.md) — Plan van de bewijsbasis voor onderbouwing van het onderzoek.
- [`docs/план-збору-доказів-власником.md`](./docs/план-збору-доказів-власником.md) — Stappenplan voor het verzamelen van bewijs door eigenaren.
- [`docs/запрошення-до-повторення.md`](./docs/запрошення-до-повторення.md) — Uitnodiging voor onafhankelijke reproductie door andere eigenaren.
- [`docs/шаблон-відтворення.md`](./docs/шаблон-відтворення.md) — Sjabloon voor het indienen van reproductieresultaten.
- [`.github/ISSUE_TEMPLATE/запит-сумісності.md`](./.github/ISSUE_TEMPLATE/запит-сумісності.md) — Meldingssjabloon voor compatibiliteitsrapporten.
