---

**Git-Repository mit 10.000 Commits, `git rebase -i HEAD~15000`**
Der Befehl versucht, die letzten 15.000 Commits zu rebasen. Da nur 10.000 Commits existieren, scheitert der Befehl mit einem Fehler. Klassischer Fehler: Der Entwickler setzt falsche Annahmen über die Repository-Geschichte voraus und ignoriert die tatsächliche Commits-Anzahl.

---

**Python: `[] == []` vs. `[] is []`**
`[] == []` ist True, weil die Listen denselben Inhalt haben (Vergleich nach Wert). `[] is []` ist False, weil jedes `[]` ein neues Objekt ist (Vergleich nach Referenz). In CPython werden Listen als PyObject-Referenzen verwaltet. Bei `==` wird `__eq__` aufgerufen, bei `is` wird die Speicheradresse verglichen. Listenobjekte werden bei Erstellung immer neu allokiert, daher unterschiedliche Adressen.

---

**Starke Konsistenz vs. Eventual Consistency (CAP-Theorem)**
- **Starke Konsistenz:** Jeder Lesezugriff gibt den letzten geschriebenen Wert zurück (z. B. nach jedem Write muss ein Read den neuen Wert sehen).
- **Eventual Consistency:** Nach einer gewissen Zeit garantiert das System Konsistenz, aber währenddessen können Lesezugriffe veraltete Werte zurückgeben.
**DynamoDB:** Kompromiss zwischen Verfügbarkeit (A) und Partitionstoleranz (P), setzt auf Eventual Consistency für hohe Skalierbarkeit und geringere Latenz.

---

**Python-Implementierung von `@lru_cache` für Fibonacci**
```python
from functools import wraps

def lru_cache(maxsize=128):
    def decorator(func):
        cache = {}
        @wraps(func)
        def wrapper(*args):
            if args in cache:
                return cache[args]
            result = func(*args)
            cache[args] = result
            return result
        return wrapper
    return decorator

@lru_cache()
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

# Vergleich: Ohne Cache: O(2^n), mit Cache: O(n)
```

---

**Vektoruhr (Vector Clocks) zur Erkennung kausaler Inkonsistenzen**
```plaintext
Initialisierung: VC = [0, 0, ..., 0] (für jeden Prozess i)

Bei Ereignis e in Prozess P:
  VC[P] += 1

Bei Nachricht m von P nach Q:
  Q.VC = max(Q.VC, m.VC)
  Q.VC[Q] += 1

Inkonsistenz: Wenn VC_i[j] > VC_j[j] für Prozess j, dann ist Ereignis i in j kausal nach Ereignis j.
```

---

**Quantenverschränkung vs. Klassische Korrelation**
- **Verschränkung:** Quantenobjekte haben nicht-lokale Korrelationen – Messung an einem Objekt bestimmt sofort den Zustand des anderen, unabhängig von der Distanz (ohne klassische Kommunikation).
- **Klassische Korrelation:** Ergebnisse sind vorbestimmt oder durch lokale verborgene Variablen erklärbar.
**Bell-Ungleichung:** Zeigt, dass keine lokale verborgene Variable Quantenverschränkung erklären kann. Die Verletzung der Bell-Ungleichung bestätigt die Nicht-Lokalität der Quantenmechanik.

---

**SQL-Fensterfunktion für Median pro Abteilung (ohne `PERCENTILE_CONT`)**
```sql
SELECT
    department,
    AVG(salary) AS median_salary
FROM (
    SELECT
        department,
        salary,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary) AS row_num,
        COUNT(*) OVER (PARTITION BY department) AS total
    FROM employees
) AS ranked
WHERE row_num IN (FLOOR((total + 1) / 2), CEIL((total + 1) / 2))
GROUP BY department;
```

---

**Nash-Gleichgewicht vs. Pareto-Optimalität (Spieltheorie)**
- **Nash-Gleichgewicht:** Kein Spieler kann sich durch einseitiges Abweichen verbessern.
- **Pareto-Optimalität:** Kein Spieler kann besser gestellt werden, ohne einen anderen zu verschlechtern.
**Gefangenendilemma:** Nash-Gleichgewicht ist Kooperieren (beide leugnen), aber Pareto-optimal wäre Kooperation (beide gestehen). Kooperation ist instabil, weil individuelle Rationalität zu Nash führt.

---

**Piagets kognitive Entwicklungsstadien & Vygotskys ZPD**
- **Piaget:** 4 Stadien (sensomotorisch, präoperational, konkret-operational, formal-operational) – Entwicklung durch innere Reifung und Assimilation/ Akkommodation.
- **Vygotsky:** ZPD – Lernen durch soziale Interaktion und "Scaffolding" (Unterstützung durch kompetentere Personen).
**Widerspruch:** Piagets Stadientheorie betont universelle, altersabhängige Entwicklung; Vygotskys ZPD betont kulturelle und soziale Einflüsse. Piaget sieht Denken als individuell, Vygotsky als sozial konstruiert.

---

**Thread-sicherer Singleton in Rust mit `std::sync::Once`**
```rust
use std::sync::Once;

static INIT: Once = Once::new();
static mut SINGLETON: Option<MyStruct> = None;

pub fn get_instance() -> &'static mut MyStruct {
    unsafe {
        INIT.call_once(|| {
            SINGLETON = Some(MyStruct::new());
        });
        SINGLETON.as_mut().unwrap()
    }
}
```
**Warum `Once`/`OnceLock` besser als `Mutex`?** `Once` initialisiert das Singleton genau einmal, ohne Locks für jeden Zugriff. `Mutex` würde Overhead für wiederholte Zugriffe verursachen.

---

**Byzantinischer vs. Crash-Fehler (Verteilte Systeme)**
- **Byzantinischer Fehler:** Knoten verhält sich willkürlich (z. B. lügt, sendet widersprüchliche Nachrichten).
- **Crash-Fehler:** Knoten fällt aus und antwortet nicht mehr.
**PBFT:** Erfordert `n >= 3f + 1` Knoten, um bis zu `f` byzantinische Fehler tolerieren zu können. Crash-Fehler erfordern nur `n > f` Knoten.

---

**Saga Pattern vs. Two-Phase Commit (Microservices)**
- **Saga (Orchestrierung/Choreographie):** Mehrere verteilte Transaktionen werden als Kette von lokalen Transaktionen und Kompensationsaktionen verwaltet. Keine globale Sperre, aber Komplexität durch Kompensation.
- **Two-Phase Commit (2PC):** Globale Sperre bis alle Teilnehmer bereit sind; blockiert bei Teilnehmern, die nicht antworten.
**Wahl:** Saga für hohe Verfügbarkeit und Skalierbarkeit; 2PC nur für Systeme, die Konsistenz über alles stellen.

---

**Doppelte Freigabe (Double Free) in Rust**
Rust verhindert "double free" zur Compile-Zeit durch das **Eigentumsmodell**:
- Jeder Wert hat genau einen Besitzer (`ownership`).
- Beim Verlassen des Gültigkeitsbereichs wird der Wert automatisch freigegeben (`drop`).
- **Borrowing:** Referenzen (`&`) erlauben temporären Zugriff, ohne Eigentum zu übertragen.
- **Lebenszeiten (`lifetime`):** Garantieren, dass Referenzen nur gültig sind, solange der Besitzer existiert.
Beispiel:
```rust
let x = String::from("hello"); // x besitzt den String
let y = &x; // y borgt x
drop(x); // Fehler: x kann nicht freigegeben werden, während y noch existiert
```

---

**Fermi-Paradoxon & Drake-Gleichung**
- **Fermi-Paradoxon:** Wenn das Universum voller intelligenter Zivilisationen ist, warum haben wir keine gefunden?
- **Drake-Gleichung:** Schätzt die Anzahl intelligenter Zivilisationen in der Milchstraße.
**Großer Filter:** Eine Barriere, die die Entstehung intelligenten Lebens oder Zivilisationen extrem unwahrscheinlich macht (z. B. frühe Auslöschung, Seltenheit technologischer Entwicklung).
**Großes Schweigen:** Entweder gibt es keine Zivilisationen, die den Filter überwinden, oder der Filter liegt noch vor uns (z. B. Selbstzerstörung).

---

**Gradient Vanishing/Exploding in tiefen RNNs**
- **Vanishing Gradients:** Bei langen Sequenzen werden Gradienten durch wiederholte Multiplikation mit Gewichten (z. B. in `h_t = tanh(W * h_{t-1} + U * x_t)`) extrem klein → Gewichte in frühen Schichten werden kaum angepasst.
- **Exploding Gradients:** Gradienten werden extrem groß → numerische Instabilität und "NaN"-Werte.
**LSTM/GRU:** Nutzen Gating-Mechanismen (vergessen/Input/Output-Gates), um Gradienten über lange Sequenzen zu propagieren und die Probleme zu mildern.

---

**Raft-Algorithmus: Leader-Wahl bei Split-Brain**
- **Split-Brain:** Netzwerkpartition trennt die Mehrheit der Knoten von einer Minderheit.
- **Raft:** Jeder Knoten startet einen Wahl-Timer. Der Knoten mit dem kürzesten Timer wird zum Kandidaten und fragt andere Knoten nach Stimmen.
- **Sicherheit:** Nur ein Knoten kann Leader werden, wenn er Stimmen von einer Mehrheit (inkl. sich selbst) erhält.
- **Split-Brain:** Die Minderheit kann keinen Leader wählen, weil sie keine Mehrheit hat. Die Mehrheit wählt einen Leader und blockiert weitere Wahlen, bis die Partition behoben ist.

---

**Pentatonik in unverbundenen Musikkulturen**
- **Mögliche Erklärungen:**
  1. **Psychologisch-akustisch:** Das Ohr bevorzugt harmonische Intervalle mit einfachen Frequenzverhältnissen (z. B. 3:2, 4:3), die in Pentatonik vorkommen.
  2. **Kulturelle Konvergenz:** Ähnliche Umweltbedingungen oder instrumentelle Constraints führen zu ähnlichen Skalen.
  3. **Definition der Forscher:** "Pentatonik" wird oft als Skala mit 5 Tönen definiert, unabhängig von kulturellen Kontexten.
**Beleg:** Pentatonik ist in Westafrika, Ostasien, keltischer und Anden-Musik verbreitet, ohne kulturellen Austausch.

---

**Plotins Henologie vs. Aristoteles’ Substanzbegriff**
- **Plotin:** Das Eine (Hen) ist jenseits von Sein und Denken, Ursprung aller Dinge. Die Welt entsteht durch Emanation: Nous (Geist) → Psyche (Seele) → Kosmos.
- **Aristoteles:** Substanz ist das, was unabhängig existiert und Eigenschaften trägt. Substanzen sind individuell (z. B. Sokrates) oder universal (z. B. Menschheit).
**Widerlegung:** Plotin negiert die Individualität der Substanz – alles ist aus dem Einen emaniert und daher nur dessen Erscheinung. Aristoteles’ Substanzbegriff setzt aber eine grundlegende ontologische Kategorie voraus, die Plotin als illusorisch ansieht.

---

**Boltzmann-Entropie & thermodynamische Definition**
Für ein ideales Gas:
- **Boltzmann-Entropie:** \( S = k \ln(\Omega) \), wobei \(\Omega\) die Anzahl der Mikrozustände ist.
- **Thermodynamische Definition:** \( dS = \delta Q / T \) (Clausius).
**Zusammenhang:** Für ein ideales Gas ist \(\Omega\) proportional zum Volumen \(V\) und zur Temperatur \(T\). Die statistische Mechanik zeigt, dass \( S = k \ln(\Omega) \) mit \( dS = \delta Q / T \) konsistent ist, wenn \(\delta Q = T dS\) und die Energieänderung durch Arbeit und Wärme beschrieben wird.

---
**Kants Ding an sich vs. Erscheinung**
- **Ding an sich:** Unerkennbarer Kern der Realität, jenseits möglicher Erfahrung.
- **Erscheinung:** Das, was wir durch Sinneswahrnehmung und Verstand erkennen.
**Jacobi:** Kants Unterscheidung führt zu Agnostizismus – wir können nicht wissen, ob die Erscheinung dem Ding an sich entspricht.
**Fichte:** Reagiert mit der These, dass das Ich das Ding an sich ist ("Ich bin Ich"). Kants Unterscheidung wird so aufgehoben.

---
**Halting-Problem & Programmäquivalenz**
- **Halting-Problem:** Unentscheidbar, weil ein Algorithmus, der für alle Programme entscheidet, ob sie halten, zu einem Widerspruch führt (Turing’s Diagonalisierung).
- **Programmäquivalenz:** Unentscheidbar, weil es auf das Halting-Problem reduziert werden kann (z. B.: Zwei Programme sind äquivalent, wenn sie für alle Eingaben dasselbe Ergebnis liefern oder beide nicht halten).

---
**Prospect Theory (Kahneman & Tversky) vs. Expected Utility**
- **Expected Utility:** Nutzen wird linear gewichtet (Risikoaversion durch abnehmenden Grenznutzen).
- **Prospect Theory:** Nutzen wird relativ zu einem Referenzpunkt bewertet. Verluste wiegen schwerer als Gewinne (Verlustaversion). Entscheidungen werden durch Framing-Effekte beeinflusst.
**Iowa Gambling Task:** Probanden bevorzugen zunächst risikoreiche Decks, lernen aber mit Erfahrung, sichere Decks zu wählen – ein Verhalten, das Expected Utility nicht erklären kann.

---
**Quantenmessparadoxon & Many-Worlds-Interpretation**
- **Messparadoxon:** Kollaps der Wellenfunktion scheint nicht-unitär (nicht reversibel).
- **Many-Worlds:** Die Wellenfunktion kollabiert nicht – alle möglichen Messergebnisse existieren in parallelen Universen. Der "Kollaps" ist eine Illusion aus der Perspektive eines einzelnen Universums.

---
**Freie kontextuelle vs. leicht kontextsensitive Grammatiken**
- **Freie kontextuelle Grammatiken (Type-2):** Regeln der Form \(A \to \alpha\), wobei \(A\) ein Nichtterminal und \(\alpha\) eine Zeichenkette ist. Keine Abhängigkeit vom Kontext.
- **Leicht kontextsensitive Grammatiken (Type-3, z. B. TAG, HPSG):** Erlauben begrenzte Kontextabhängigkeiten (z. B. durch Merkmale oder eingeschränkte Substitution).
**Chomskys Hierarchie:** Unzureichend für natürliche Sprachen, weil sie keine Dislokation, Inklusion oder andere Phänomene (z. B. in Schwyzerdütsch) modellieren kann.

---
**Nicht-Überweisungsdoktrin vs. Chevron-Deference (US-Recht)**
- **Nicht-Überweisungsdoktrin:** Congress darf keine Macht delegieren, die es selbst nicht hat (Art. I, Section 1 der Verfassung).
- **Chevron-Deference (1984):** Gerichte müssen Behördeninterpretationen von Gesetzen respektieren, wenn das Gesetz unklar ist.
**Loper Bright (2024):** Chevron wurde aufgehoben. Gerichte müssen Gesetze unabhängig interpretieren – Behörden haben kein Deference-Mehr. **Auswirkung:** Congress muss Gesetze präziser formulieren, Gerichte haben mehr Macht.

---
**Ellipse vs. Parabel (projektive Geometrie)**
- **Ellipse:** Schneidet die Ferngerade nicht (alle Punkte im Endlichen).
- **Parabel:** Tangential zur Ferngeraden (ein Punkt im Unendlichen).
- **Projektiv:** Ellipsen und Parabeln sind Kegelschnitte, die sich durch ihre Schnittmenge mit der Ferngeraden unterscheiden. Hyperbeln schneiden die Ferngerade in zwei Punkten.

---
**Hegels "Anerkennung" & Honneth**
- **Hegel (Herr-Knecht-Dialektik):** Anerkennung ist der Schlüssel zur Freiheit. Der Herr wird nur durch den Knecht anerkannt und umgekehrt – ein Prozess der gegenseitigen Abhängigkeit und Selbstfindung.
- **Honneth:** Erweiterung zu moderner Gesellschaftstheorie: Anerkennung als Grundlage für Identität und soziale Integration (Liebe, Rechte, Solidarität).

---
**Zusammenbruch der Phillips-Kurve (Stagflation 1970er)**
- **Phillips-Kurve:** Trade-off zwischen Inflation und Arbeitslosigkeit.
- **Stagflation:** Hohe Inflation + hohe Arbeitslosigkeit + stagnierendes Wachstum.
**Gründe:**
1. Ölpreisschocks (Angebotsseite) erhöhten Kosten, ohne Nachfrage zu steigern.
2. Erwartungen: Arbeitnehmer und Unternehmen antizipierten hohe Inflation → Lohn-Preis-Spirale.
3. Geldpolitik: Zentralbanken konnten nicht gleichzeitig Inflation bekämpfen und Wachstum fördern.
**Folge:** Phillips-Kurve wurde als instabil angesehen – es gab keine stabile Trade-off-Relation mehr.

---
**Inclusive Fitness (Hamilton) & Eusozialität**
- **Inclusive Fitness:** Gesamtfitness = direkte Fitness (eigene Nachkommen) + indirekte Fitness (Nachkommen Verwandter, gewichtet durch Verwandtschaftsgrad \(r\)).
- **Regel \(rB > C\):** Altruistisches Verhalten evolviert, wenn der Nutzen \(B\) für den Empfänger, multipliziert mit dem Verwandtschaftsgrad \(r\), größer ist als die Kosten \(C\) für den Altruisten.
**Beispiel Eusoziale Hymenopteren:** Arbeiterinnen sind sterile und helfen der Königin (ihre Mutter), weil \(r\) zu Schwestern 0.75 ist (haplo-diploider Mechanismus).

---
**Gödels Unvollständigkeitssätze & Hilbert-Programm**
- **1. Satz:** Jedes konsistente, rekursiv axiomatisierbare formale System, das die Arithmetik enthält, ist unvollständig – es gibt wahre, aber unbeweisbare Aussagen.
- **2. Satz:** Ein solches System kann seine eigene Konsistenz nicht beweisen.
**Auswirkung auf Hilbert:** Das Ziel, die Mathematik durch formale Systeme zu fundieren (Hilberts Programm), scheitert. Unvollständigkeit ist unvermeidbar.

---
**Humes Induktionsproblem & Poppers Falsifikationismus**
- **Hume:** Induktion (Schluss von beobachteten auf zukünftige Fälle) ist logisch nicht gerechtfertigt. Es gibt keinen "logischen Sprung" von "Alle bisher beobachteten Schwäne sind weiß" zu "Alle Schwäne sind weiß".
- **Popper:** Wissenschaftliche Theorien sind nicht verifizierbar, aber falsifizierbar. Induktion ist irrelevant – Theorien werden durch strenge Tests und Widerlegung gestärkt.

---
**Rosch’s Prototyp vs. klassische Kategorien**
- **Klassische Kategorien:** Definiert durch notwendige und hinreichende Bedingungen (z. B. "Vogel = hat Federn + kann fliegen + legt Eier").
- **Prototyp:** Kategorien haben einen zentralen Prototyp (z. B. "Rotkehlchen" für Vögel), andere Exemplare werden durch Ähnlichkeit zum Prototyp klassifiziert.
**Beispiel:** Pinguin ist ein schlechter Prototyp für "Vogel", weil er nicht fliegen kann – trotzdem wird er als Vogel erkannt.

---
**CRISPR-Cas9 vs. Base Editing**
- **CRISPR-Cas9:** Schneidet DNA an spezifischer Stelle (Doppelstrangbruch). Fehleranfällig durch Off-Target-Effekte und Reparaturmechanismen (NHEJ).
- **Base Editing:** Führt gezielte Punktmutationen ein, ohne die DNA zu schneiden (z. B. C→T oder A→G). Präziser und sicherer, da keine Doppelstrangbrüche entstehen.

---
**Korrelation vs. Kausalität & DAGs (Judea Pearl)**
- **Korrelation:** Statistischer Zusammenhang.
- **Kausalität:** Ursache-Wirkungs-Beziehung.
**Problem:** Ohne kausales Modell (DAG) kann Korrelation irreführend sein (z. B. Scheinkorrelation durch Confounder).
**Collider:** Eine Variable, die von zwei anderen abhängt (z. B. "Bildung" und "Einkommen" hängen von "sozioökonomischem Status" ab). Kontrolle eines Colliders kann zu verzerrten Schätzungen führen.

---
**Osmanisches Millet-System: Erfolg oder strukturelle Ungleichheit?**
- **Milit-System:** Religiöse Minderheiten (Millets) erhielten Autonomie in rechtlichen und religiösen Angelegenheiten.
- **Erfolg:** Ermöglichte langfristige Stabilität in einem multiethnischen Reich.
- **Ungleichheit:** Muslimische Mehrheit hatte Vorrechte; Nicht-Muslime waren rechtlich und politisch benachteiligt. Tansimat-Reformen (19. Jh.) versuchten, dies zu ändern, scheiterten aber an Widerständen.

---
**Thukydides-Falle & USA-China**
- **Thukydides-Falle:** Krieg zwischen aufstrebender und etablierter Macht ist wahrscheinlich (z. B. Athen vs. Sparta).
- **Beschränkungen:** Globalisierung, wirtschaftliche Interdependenz und nukleare Abschreckung machen direkte Konflikte unwahrscheinlich. Wirtschaftliche Rivalität und technologische Konkurrenz dominieren.

---
**R2P vs. Souveränität (Internationales Recht)**
- **R2P (Responsibility to Protect):** Staat hat primäre Verantwortung, seine Bevölkerung zu schützen. Bei Versagen hat die internationale Gemeinschaft das Recht, zu intervenieren.
- **Souveränität:** Staatliche Souveränität und Nicht-Einmischung sind zentrale Prinzipien (Art. 2(7) UN-Charta).
**Spannung:** R2P rechtfertigt Interventionen (z. B. Libyen 2011), die als Verletzung der Souveränität kritisiert werden.

---
**Gödels Theorem & formale Systeme**
- **Peano-Arithmetik:** Enthält genug Ausdrucksstärke für Gödels Unvollständigkeitssätze.
- **Vollständige geordnete Körper:** Können Aussagen über ihre eigene Konsistenz treffen (z. B. "0 ≠ 1"), daher sind Gödels Sätze nicht direkt anwendbar.

---
**Piagets vs. Vygotskys Entwicklungspsychologie**
- **Piaget:** Entwicklung ist universell, stadienabhängig und individuell. Kognitive Strukturen entstehen durch Assimilation/Akkommodation.
- **Vygotsky:** Entwicklung ist sozial und kulturell geprägt. Lernen durch ZPD und "Scaffolding".
**Widerspruch:** Piaget betont biologische Reifung, Vygotsky soziale Interaktion. Piagets Stadien sind starr, Vygotskys ZPD dynamisch.

---
**Osmanisches Millet-System: Religiöser Pluralismus oder institutionelle Ungleichheit?**
- **Pluralismus:** Ermöglichte Koexistenz verschiedener Religionen und Kulturen unter osmanischer Herrschaft.
- **Ungleichheit:** Muslimische Mehrheit genoss Privilegien; Nicht-Muslime waren rechtlich und sozial benachteiligt. Tansimat-Reformen (19. Jh.) versuchten, dies zu ändern, scheiterten aber an Widerstand und strukturellen Problemen.

---
**Typ I vs. Typ II Fehler (Frequentist) & Bayesianische Inferenz**
- **Typ I (α):** Falsche Ablehnung der Nullhypothese (falsch positiv).
- **Typ II (β):** Falsche Beibehaltung der Nullhypothese (falsch negativ).
**Bayesianisch:** Keine festen Fehlerraten. Stattdessen wird die Wahrscheinlichkeit einer Hypothese durch die posteriori Verteilung aktualisiert. Typ I/II-Errors sind nicht direkt übertragbar, da sie von der Stichprobenverteilung abhängen.

---
**Harmonische Reihe & Unendlichkeitsbeweis**
- **Harmonische Reihe:** \( \sum_{n=1}^{\infty} \frac{1}{n} \)
**Oresmes Beweis:**
Gruppiere Terme:
\( 1 + \frac{1}{2} + (\frac{1}{3} + \frac{1}{4}) + (\frac{1}{5} + ... + \frac{1}{8}) + ... \)
Jede Gruppe ist \( \geq \frac{1}{2} \). Da es unendlich viele Gruppen gibt, divergiert die Reihe gegen Unendlich.

---
**UNO-Resolution vs. ratifizierter Vertrag (Völkerrecht)**
- **Resolution der UNO-Generalversammlung:** Politische Empfehlung, rechtlich nicht bindend (außer bei Haushaltsfragen).
- **Ratifizierter Vertrag:** Bindendes internationales Recht, das von Staaten umgesetzt werden muss.
**Jus cogens:** Normen, die von der internationalen Gemeinschaft als zwingend anerkannt werden (z. B. Verbot von Völkermord). UNO-Resolutionen können Jus cogens widerspiegeln, aber nicht selbst schaffen.

---
**Trolley- vs. Footbridge-Problem & Moralpsychologie**
- **Trolley-Problem:** Utilitaristisch – Gleise umstellen, um 5 zu retten (1 Tod).
- **Footbridge-Problem:** Aktive Tötung eines Einzelnen, um 5 zu retten – emotional aversiver.
**Grünes dual-process Modell:** Moralische Urteile entstehen durch schnelle, intuitive (System 1) und langsame, rationale (System 2) Prozesse. Das Footbridge-Problem aktiviert emotionale Reaktionen, die utilitaristische Abwägungen überlagern.

---
**Müller-Lyer-Illusion & Wahrnehmungspsychologie**
- **Illusion:** Zwei gleich lange Linien erscheinen unterschiedlich lang, wenn Pfeilspitzen nach innen/außen zeigen.
**Erklärung:** Wahrnehmung nutzt räumliche Tiefe als Hinweis. Pfeile nach außen deuten auf eine "nähere" Ecke hin, Pfeile nach innen auf eine "tiefere" – das Gehirn interpretiert dies als Größenunterschied.

---
**Stringtheorie & Kompaktifizierung**
- **Kompaktifizierung:** Extra-Dimensionen (über die 4 bekannten hinaus) sind "aufgerollt" (kompakt) und daher nicht direkt beobachtbar.
**Warum nötig?** Stringtheorie erfordert 10 oder 11 Dimensionen für mathematische Konsistenz. Kaluza-Klein-Theorie und Calabi-Yau-Mannigfaltigkeiten erklären, wie diese Dimensionen versteckt bleiben.

---
**HIV & Reverse-Transkriptase-Hemmer**
- **Mechanismus:** Reverse-Transkriptase-Hemmer blockieren die Umwandlung von viraler RNA in DNA, unterbricht den Replikationszyklus.
- **Monotherapie:** Führt schnell zu Resistenzen, weil HIV eine hohe Mutationsrate hat. Kombinationstherapien (HAART) sind nötig, um multiple Angriffsziele zu adressieren.

---
**Hayeks Wissensproblem & zentrale Planung**
- **Wissensproblem:** Wissen ist dezentral und subjektiv. Eine zentrale Behörde kann nie alle relevanten Informationen sammeln und verarbeiten.
- **Preissignale:** Märkte aggregieren verteiltes Wissen durch Preise. Zentrale Planung scheitert, weil sie Preise nicht ersetzen kann – sie sind das Ergebnis spontaner Ordnung.

---
**Hayeks Wissensproblem in der KI-Planung**
Hayeks Argument gilt auch gegen KI-gestützte zentrale Planung: KI kann zwar große Datenmengen verarbeiten, aber nicht das implizite, verteilte Wissen (z. B. lokale Präferenzen, unvorhergesehene Umstände) erfassen. Märkte bleiben effizienter für Koordination.

---
**25-Sonnenmassen-Stern: Nukleosynthese bis zum Eisenkern-Kollaps**
1. **Wasserstoffbrennen:** pp-Kette → CNO-Zyklus → Helium.
2. **Heliumbrennen:** Triple-Alpha-Prozess → Kohlenstoff, Sauerstoff.
3. **Kohlenstoffbrennen:** Neon, Magnesium.
4. **Neonbrennen:** Sauerstoff, Magnesium.
5. **Sauerstoffbrennen:** Silizium, Schwefel.
6. **Siliziumbrennen:** Eisen (Endpunkt, da Eisen die höchste Bindungsenergie pro Nukleon hat).
**Eisenkern-Kollaps:** Ohne weitere Fusion kollabiert der Kern, Supernova-Explosion, Bildung eines Neutronensterns oder Schwarzen Lochs.

---
**Wittgensteins "Familienähnlichkeit"**
- **Kritik an notwendigen/ hinreichenden Bedingungen:** Begriffe (z. B. "Spiel") haben keine gemeinsame Definition, sondern zeigen "Familienähnlichkeiten" – überlappende Ähnlichkeiten zwischen Mitgliedern einer Kategorie.
- **Beispiel:** Spiele haben keine gemeinsame Eigenschaft (z. B. Wettbewerb, Regeln, Spaß), sondern ähneln sich in verschiedenen Aspekten.

---
**Epigenetik: Histon-Modifikationen vs. DNA-Methylierung**
- **Histon-Modifikationen:** Chemische Veränderungen an Histon-Proteinen (z. B. Acetylierung, Methylierung), die die DNA-Verpackung und Genexpression beeinflussen.
- **DNA-Methylierung:** Methylgruppen an Cytosin-Basen, meist in Promotorregionen, die Gene abschalten.
**Vererbbarkeit:** Beide können durch Zellteilung weitergegeben werden, ohne die DNA-Sequenz zu ändern. Umweltfaktoren (z. B. Ernährung) können Epigenome beeinflussen.

---
**Hayeks Wissensproblem in der Makroökonomie**
Hayek argumentiert, dass verteilte Informationen (z. B. lokale Präferenzen, technologische Möglichkeiten) durch Preissignale koordiniert werden. Zentrale Planung (z. B. sozialistische Ökonomien) scheitert, weil sie diese Signale nicht ersetzen kann. Japanische "verlorene Jahrzehnte" zeigen, wie staatliche Eingriffe zu Ineffizienz und Stagnation führen.

---
**Technische Schulden in der Softwareentwicklung (makroökonomische Perspektive)**
- **Technische Schulden:** Kosten zukünftiger Anpassungen durch kurzfristige Lösungen (z. B. schlecht dokumentierter Code, veraltete Technologien).
- **Makroökonomisch:** Legacy-Systeme sind wie "tote Kapitalgüter" – sie binden Ressourcen, die für Innovation fehlen. Die "Schuldenfalle" entsteht, wenn Reparaturen teurer werden als Neuentwicklungen, aber Neuentwicklungen nicht möglich sind (Lock-in-Effekt).

---
**Foucaults "Überwachung und Strafe" & digitale Überwachung**
- **Panoptikum:** Architektur, in der Gefangene ständig überwacht werden können, ohne zu wissen, ob sie beobachtet werden → Selbstdisziplin.
- **Digitale Überwachung:** Ähnliche Mechanismen durch Datenanalyse (z. B. Social Scoring, Predictive Policing). **Kritik:** Digitale Systeme sind noch invasiver und schwerer zu durchschauen als das architektonische Panoptikum.

---
**Transformatore & Aufmerksamkeit vs. RNNs**
- **Mechanismus:** Selbstaufmerksamkeit (Self-Attention) gewichtet Eingabetoken nach ihrer Relevanz für den aktuellen Kontext. Positionale Kodierung (z. B. Sinus/Cosinus-Funktionen) fügt räumliche Informationen hinzu.
- **Vorteile:** Parallelisierung (keine sequentielle Abhängigkeit wie bei RNNs), bessere Handhabung langer Abhängigkeiten, Skalierbarkeit.

---
**Französische Präventivkontrolle vs. amerikanische Judicial Review**
- **Frankreich:** Verfassungsrat prüft Gesetze vor ihrer Verkündung (präventiv). Politisch dominiert, weniger gerichtlich.
- **USA:** Gerichte prüfen Gesetze nach ihrer Verkündung (repressiv). Richterliche Unabhängigkeit, aber politische Spannungen (z. B. "Court-Packing").
**Demokratische Implikationen:**
- Frankreich: Parlament hat letzte Kontrolle, aber weniger Transparenz.
- USA: Gerichte können Gesetze blockieren, aber demokratische Legitimation ist indirekt.

---
**Quantenchemie: Hartree-Fock & Korrelationsenergie**
- **Hartree-Fock:** Vernachlässigt Elektronenkorrelation (Wechselwirkung zwischen Elektronen). Die Energie ist daher systematisch zu hoch.
- **Post-HF-Methoden:** z. B. Configuration Interaction (CI), Coupled Cluster (CC), Møller-Plesset-Störungstheorie. Korrigieren die Korrelationsenergie durch Berücksichtigung angeregter Zustände.

---
**Investiturstreit & Macht von Kirche vs. Staat**
- **Investiturstreit (1075–1122):** Konflikt zwischen Papst Gregor VII. und Kaiser Heinrich IV. um die Einsetzung von Bischöfen.
- **Auswirkungen:** Schwächung der kaiserlichen Autorität, Stärkung der päpstlichen Macht. **Wormser Konkordat (1122):** Kompromiss – Kaiser behielt weltliche, Papst geistliche Investitur.

---
**Zweiter Hauptsatz der Thermodynamik & Maxwellscher Dämon**
- **Zweiter Hauptsatz:** Entropie eines abgeschlossenen Systems nimmt zu.
- **Maxwellscher Dämon:** Ein "Dämon", der Moleküle sortiert, scheint den Zweiten Hauptsatz zu verletzen.
**Landauers Prinzip:** Die Löschung von Information (z. B. Dämons Gedächtnis) kostet Energie (\(kT \ln 2\) pro Bit). Der Dämon muss Information löschen, um zu "resetten" – dies erhöht die Entropie anderswo und erhält den Zweiten Hauptsatz.

---
**Sprechakte (Austin & Searle)**
- **Locutionary Act:** Äußerung eines Satzes mit Bedeutung (z. B. "Es ist kalt hier").
- **Illocutionary Act:** Intention hinter der Äußerung (z. B. Bitte, den Fenster zu schließen).
- **Perlocutionary Act:** Wirkung auf den Hörer (z. B. Hörer schließt das Fenster).

---
**Liquiditätsfalle & Geldpolitik (Makroökonomie)**
- **Ursachen:** Nominalzinsen können nicht unter null fallen (Nullzinsgrenze). Bei Deflation steigen reale Zinsen, was Investitionen hemmt.
- **Auswirkungen:** Geldpolitik verliert Wirksamkeit – zusätzliche Geldmenge wird gehortet statt investiert.
**Japan:** "Verlorene Jahrzehnte" – trotz ultralockerer Geldpolitik (Quantitative Easing) blieb die Inflation niedrig und das Wachstum schwach. Zeigt, dass Geldpolitik bei Deflation und sinkenden Erwartungen an Grenzen stößt.

---
**P vs. NP & Komplexitätsklassen**
- **P:** Probleme, die in Polynomialzeit lösbar sind (z. B. Sortieren).
- **NP:** Probleme, deren Lösung in Polynomialzeit überprüfbar ist (z. B. Erfüllbarkeit von Formeln).
- **NP-vollständig:** Probleme in NP, die mindestens so schwer sind wie alle anderen NP-Probleme (z. B. Traveling Salesman).
**Beispiel TSP:** Kein bekannter Algorithmus löst TSP in Polynomialzeit – es ist NP-vollständig.

---
**Hegels "Ende der Geschichte" vs. Marx’ historischer Materialismus**
- **Hegel:** Geschichte endet mit der vollkommenen Verwirklichung der Freiheit im modernen Verfassungsstaat (preußisches Modell).
- **Marx:** Geschichte endet mit dem Klassenkampf und der klassenlosen Gesellschaft (Kommunismus).
**Kojèves Interpretation:** Hegels "Ende der Geschichte" ist ein dialektischer Prozess, der in Marx’ Revolution mündet. Hegels Freiheit wird bei Marx zur materiellen Gleichheit.

---
**Hegel vs. Marx: Erbe und Bruch**
- **Hegel:** Dialektik als Prozess der Selbstverwirklichung des Geistes (Idealismus).
- **Marx:** Dialektik als Prozess der materiellen Produktionsverhältnisse (Materialismus).
**Bruch:** Marx kehrt Hegels Dialektik um – nicht der Geist, sondern die Ökonomie treibt die Geschichte voran. Hegels "absoluter Geist" wird bei Marx zur "klassenlosen Gesellschaft".

---
**Gleichstufiger Triton & Dominantseptakkord**
- **Gleichstufiger Triton:** Intervall von 6 Halbtonen (z. B. C–F#). Klingt dissonant und erzeugt Spannung, weil er nicht in der Tonleiter aufgelöst wird.
- **Dominantseptakkord:** Enthält einen Triton (z. B. G7: G-B-D-F). Der Triton wird durch die Auflösung in die Tonika (C) aufgelöst – das schafft harmonische Spannung und Entspannung.

---
**Deflation & Schulden-Deflationsspirale (Fisher 1933)**
- **Mechanismus:**
  1. Deflation erhöht die reale Schuldenlast (Schulden sind nominal fix, aber Geldwert steigt).
  2. Schuldner müssen mehr verkaufen, um Schulden zu bedienen → Preise fallen weiter.
  3. Unternehmen und Haushalte sparen mehr, konsumieren weniger → Nachfrage bricht ein.
  4. Wirtschaft schrumpft, Deflation verstärkt sich.
**Beispiel:** Große Depression – Deflation führte zu Bankenpleiten, Arbeitslosigkeit und weiterem Preisverfall.

---
**Bourdieus "Symbolische Gewalt" & Habitus**
- **Symbolische Gewalt:** Unterdrückung durch scheinbar neutrale Institutionen (z. B. Schule, Sprache), die soziale Hierarchien reproduzieren.
- **Habitus:** Verinnerlichte Dispositionen (Denk-, Wahrnehmungs-, Handlungsmuster), die durch soziale Position geprägt sind. Reproduziert soziale Ungleichheit, indem sie sie als "natürlich" erscheinen lässt.

---
**Pragmatismus: Peirce, James, Dewey**
- **Peirce:** Wahrheit ist das, worauf sich eine Gemeinschaft von Forschern im Laufe der Zeit einigen wird ("Belief Fixation").
- **James:** Wahrheit ist das, was sich "im Leben bewährt" ("Pragmatic Maxim").
- **Dewey:** Wahrheit entsteht durch experimentelles Handeln und Anpassung an die Umwelt ("Instrumentalismus").
**Unterschiede:** Peirce betont Wissenschaft, James Individuum, Dewey Gesellschaft.

---
**Chalmers’ "Hard Problem of Consciousness"**
- **Einfaches Problem:** Erklärt kognitive Funktionen (z. B. Aufmerksamkeit, Wahrnehmung).
- **Hartes Problem:** Warum und wie entsteht subjektives Erleben (Qualia) aus physikalischen Prozessen?
**Philosophische Zombies:** Hypothetische Wesen, die sich wie Menschen verhalten, aber kein Bewusstsein haben. Ihre Möglichkeit stellt den Physikalismus infrage – wenn Zombies denkbar sind, ist Bewusstsein nicht auf Physik reduzierbar.

---
**Turing-Test: Philosophie & Grenzen**
- **Test:** Ein Computer gilt als intelligent, wenn ein Mensch ihn nicht von einem Menschen unterscheiden kann.
- **Voraussetzungen:** Menschliche Intelligenz wird als Maßstab genommen; Test ist operational, nicht theoretisch.
**Searls "Chinesisches Zimmer":**
- Ein Mensch folgt Regeln, um chinesische Fragen mit chinesischen Antworten zu beantworten – versteht aber kein Chinesisch.
- **Argument:** Syntax ≠ Semantik. Der Turing-Test misst nur Verhalten, nicht Bewusstsein oder Verständnis.
**Kritik & Grenzen:** Der Test sagt nichts über die Natur von Intelligenz oder Bewusstsein aus. Starke KI (Bewusstsein) ist damit nicht bewiesen.

---
**CRISPR-Cas9: Molekularer Mechanismus**
1. **gRNA (guide RNA):** Bindet an Ziel-DNA (komplementäre Sequenz + PAM-Sequenz).
2. **Cas9:** Endonuklease, die DNA an der Zielsequenz schneidet (Doppelstrangbruch).
3. **PAM-Sequenz (z. B. NGG):** Notwendig für Cas9-Bindung.
4. **Nukleasen (RuvC & HNH):** Schneiden die DNA-Stränge (3’- und 5’-Ende).

---
**Inclusive Fitness bei Eusozialen (Hamilton)**
- **Formel:** \( rB > C \), wobei \( r \) = Verwandtschaftsgrad, \( B \) = Nutzen für Empfänger, \( C \) = Kosten für Altruist.
- **Hymenopteren (Bienen, Ameisen):** Haplo-diploid – Arbeiterinnen sind zu 75% mit Schwestern verwandt (\( r = 0.75 \)). Altruistisches Verhalten (z. B. Verteidigung des Stocks) lohnt sich, weil die indirekte Fitness hoch ist.

---
**Piaget vs. Vygotsky: Entwicklungspsychologie**
- **Piaget:** Entwicklung ist universell, stadienabhängig (sensomotorisch → formal-operational). Lernen ist individuell.
- **Vygotsky:** Entwicklung ist sozial und kulturell (ZPD, Scaffolding). Lernen durch Interaktion mit kompetenteren Personen.
**Widerspruch:** Piagets Stadien sind starr, Vygotskys ZPD dynamisch. Piaget betont biologische Reifung, Vygotsky kulturelle Einflüsse.

---
**Go-Code: Race Condition**
```go
func (c *Cache) Increment(key string) {
    val := c.Get(key)
    c.mu.Lock()
    c.data[key] = val + 1
    c.mu.Unlock()
}
```
**Problem:** Zwischen `Get(key)` und `Lock()` kann sich `val` ändern, wenn andere Goroutines `c.data` modifizieren. **Lösung:**
```go
func (c *Cache) Increment(key string) {
    c.mu.Lock()
    val := c.data[key]
    c.data[key] = val + 1
    c.mu.Unlock()
}
```

---
**GPCR-Signaltransduktion**
1. **Ligand bindet** an GPCR (7-Transmembran-Rezeptor).
2. **Konformationsänderung** aktiviert das G-Protein (αβγ-Untereinheit).
3. **Gα dissoziiert**, bindet GTP, aktiviert Effektoren (z. B. Adenylatcyclase → cAMP → PKA).
4. **Signalweiterleitung:** cAMP aktiviert Proteinkinase A, die Zielproteine phosphoryliert.
5. **Terminierung:** GTPase-Aktivität von Gα hydrolysiert GTP zu GDP → Gα reassoziiert mit βγ, Signal stoppt.
**Pharmaka-Ziel:** ~30% aller Medikamente wirken über GPCRs (z. B. Betablocker, Opioide).

---
**Default Mode Network (DMN) in Neurowissenschaften**
- **Rolle:** Aktiv bei Ruhezustand, Selbstreflexion, Erinnerungen, Zukunftsplanung.
- **Aktivität:** Deaktiviert bei externer Aufmerksamkeit (z. B. Aufgabenlösen).
- **Bewusstsein:** DMN ist mit subjektiver Erfahrung und "Mind-Wandering" assoziiert. Störungen (z. B. Alzheimer) beeinflussen autobiografisches Gedächtnis.

---
**Arrows Unmöglichkeitstheorem**
- **Axiome:**
  1. **Unbeschränkter Definitionsbereich:** Alle Präferenzordnungen sind zugelassen.
  2. **Pareto-Effizienz:** Wenn alle A über B bevorzugen, muss das Ergebnis A>B sein.
  3. **Unabhängigkeit von irrelevanten Alternativen:** Das Ergebnis zwischen A und B hängt nur von A und B ab, nicht von C.
  4. **Diktaturfreiheit:** Kein einzelner Wähler kann das Ergebnis allein bestimmen.
**Folge:** Es gibt keine soziale Wohlfahrtsfunktion, die alle Axiome gleichzeitig erfüllt.

---
**Theseus’ Schiff & persönliche Identität**
- **Paradoxon:** Wenn alle Planken eines Schiffes ersetzt werden, ist es dann noch dasselbe Schiff?
- **Locke:** Identität basiert auf Bewusstsein/Kontinuität, nicht auf materieller Kontinuität.
- **Hume:** Es gibt kein "Ich" – nur Bündel von Wahrnehmungen.
- **Parfit:** Identität ist eine Frage der psychologischen Kontinuität, nicht der Substanz.

---
**Mehrfachrealisierbarkeit & Physikalismus**
- **Argument:** Mentale Zustände (z. B. Schmerz) können durch unterschiedliche physische Zustände realisiert werden (z. B. Gehirn eines Menschen vs. Alien).
- **Folge:** Mentale Zustände sind nicht auf eine bestimmte Physik reduzierbar.
**Putnams Funktionalismus:** Mentale Zustände sind funktionale Zustände – sie werden durch ihre Rolle im kognitiven System definiert, nicht durch ihre physische Realisierung.

---
**Verteilte Datenbanken: Replikation vs. Sharding vs. Partitionierung**
- **Replikation:** Daten werden auf mehreren Knoten kopiert (hohe Verfügbarkeit, aber Konsistenzprobleme).
- **Sharding:** Daten werden horizontal aufgeteilt (z. B. nach Benutzer-ID). Skaliert gut, aber komplexe Joins über Shards.
- **Partitionierung:** Ähnlich wie Sharding, aber oft logischer (z. B. nach Region). Kann mit Sharding kombiniert werden.

---
**Roschs Prototyp vs. klassische Kategorien (Kognitive Psychologie)**
- **Klassisch:** Kategorien haben klare Grenzen (z. B. "Vogel = hat Federn + kann fliegen").
- **Prototyp:** Kategorien haben einen zentralen Prototyp (z. B. Rotkehlchen), andere Exemplare werden durch Ähnlichkeit klassifiziert.
**Beispiel:** Pinguin ist ein schlechter Prototyp für "Vogel", weil er nicht fliegen kann – trotzdem wird er als Vogel erkannt.

---
**Pentatonik in unverbundenen Musikkulturen (Kognitive Musikpsychologie)**
- **Erklärung:**
  1. **Akustisch:** Einfache Frequenzverhältnisse (z. B. 3:2, 4:3) erzeugen stabile Intervalle, die in Pentatonik vorkommen.
  2. **Kulturelle Konvergenz:** Ähnliche Umweltbedingungen oder instrumentelle Constraints führen zu ähnlichen Skalen.
  3. **Definition:** "Pentatonik" wird oft als Skala mit 5 Tönen definiert, unabhängig von kulturellen Kontexten.
**Beleg:** Pentatonik ist in Westafrika, Ostasien, keltischer und Anden-Musik verbreitet, ohne kulturellen Austausch.

---
**Heckscher-Ohlin-Theorem & Leontief-Paradoxon**
- **Heckscher-Ohlin:** Länder exportieren Güter, die ihre reichlich vorhandenen Produktionsfaktoren intensiv nutzen.
- **Leontief-Paradoxon:** USA (kapitalreich) exportierten arbeitsintensive Güter und importierten kapitalintensive – widersprach der Theorie.
**Krugmans neue Handelstheorie:** Erklärt Handel durch steigende Skalenerträge und Produktdifferenzierung – nicht nur durch Faktorausstattung.

---
**Stagflation & Phillips-Kurve**
- **Phillips-Kurve:** Trade-off zwischen Inflation und Arbeitslosigkeit.
- **Stagflation (1970er):** Hohe Inflation + hohe Arbeitslosigkeit + stagnierendes Wachstum.
**Gründe:**
1. Ölpreisschocks (Angebotsseite) erhöhten Kosten, ohne Nachfrage zu steigern.
2. Erwartungen: Arbeitnehmer und Unternehmen antizipierten hohe Inflation → Lohn-Preis-Spirale.
3. Geldpolitik: Zentralbanken konnten nicht gleichzeitig Inflation bekämpfen und Wachstum fördern.
**Folge:** Phillips-Kurve wurde als instabil angesehen – es gab keine stabile Trade-off-Relation mehr.

---
**Piaget vs. Vygotsky: Entwicklungspsychologie**
- **Piaget:** Entwicklung ist universell, stadienabhängig (sensomotorisch → formal-operational). Lernen ist individuell.
- **Vygotsky:** Entwicklung ist sozial und kulturell (ZPD, Scaffolding). Lernen durch Interaktion mit kompetenteren Personen.
**Widerspruch:** Piagets Stadien sind starr, Vygotskys ZPD dynamisch. Piaget betont biologische Reifung, Vygotsky kulturelle Einflüsse.

---
**Nash-Gleichgewicht vs. Pareto-Optimalität (Spieltheorie)**
- **Nash-Gleichgewicht:** Kein Spieler kann sich durch einseitiges Abweichen verbessern (z. B. Gefangenendilemma: Beide leugnen).
- **Pareto-Optimalität:** Kein Spieler kann besser gestellt werden, ohne einen anderen zu verschlechtern (z. B. Gefangenendilemma: Beide gestehen).
**Dilemma:** Nash-Gleichgewicht ist nicht Pareto-optimal. Kooperation wäre besser, ist aber instabil.

---
**Rawls’ "Schleier des Nichtwissens" & Differenzprinzip**
- **Schleier des Nichtwissens:** Entscheidungen über Gerechtigkeit werden getroffen, ohne zu wissen, welche Position man im Leben einnehmen wird.
- **Differenzprinzip:** Ungleichheiten sind nur dann gerechtfertigt, wenn sie den am wenigsten Begünstigten zugutekommen.
**Nozick:** Kritik am Differenzprinzip – es rechtfertigt staatliche Umverteilung, die individuelle Rechte verletzt. Gerechtigkeit entsteht durch freiwillige Transaktionen, nicht durch staatliche Eingriffe.

---
**ESBL-vermittelte Resistenz gegen β-Laktam-Antibiotika**
- **Mechanismus:** Extended-Spectrum Beta-Laktamasen (ESBLs) hydrolysieren β-Laktam-Ringe (z. B. Penicilline, Cephalosporine).
- **Genetik:** ESBL-Gene (z. B. *blaCTX-M*) sind auf Plasmiden lokalisiert und können horizontal übertragen werden.
- **Bedrohung:** ESBL-bildende Bakterien (z. B. *E. coli*, *K. pneumoniae*) sind multiresistent und schwer behandelbar.

---
**Hegels "Ende der Geschichte" & Marx’ historischer Materialismus**
- **Hegel:** Geschichte endet mit der vollkommenen Verwirklichung der Freiheit im modernen Verfassungsstaat.
- **Marx:** Geschichte endet mit dem Klassenkampf und der klassenlosen Gesellschaft (Kommunismus).
**Erbe & Bruch:** Marx kehrt Hegels Dialektik um – nicht der Geist, sondern die Ökonomie treibt die Geschichte voran. Hegels "absoluter Geist" wird bei Marx zur "klassenlosen Gesellschaft".

---
**Simpsons Paradoxon**
- **Definition:** Ein Trend erscheint in verschiedenen Gruppen, verschwindet oder kehrt sich um, wenn die Gruppen kombiniert werden.
- **Beispiel:** Ein Medikament wirkt in allen Altersgruppen besser als ein Placebo, aber in der Gesamtpopulation scheint es schlechter zu wirken, weil ältere Patienten (die generell schlechtere Ergebnisse haben) häufiger das Medikament erhalten.

---
**Dunkle Materie & Dunkle Energie (Astronomie)**
- **Dunkle Materie:** Nicht-leuchtende Masse, die durch Gravitationswirkung auf sichtbare Materie nachgewiesen wird (z. B. Galaxienrotation).
- **Dunkle Energie:** Unbekannte Form von Energie, die die beschleunigte Expansion des Universums antreibt (~68% der Energiedichte).
**WIMPs (Weakly Interacting Massive Particles):** Hypothetische Teilchen, die Dunkle Materie erklären könnten. Bisher keine direkte Detektion.

---
**ΛCDM-Modell & Kosmologie**
- **ΛCDM:** Standardmodell der Kosmologie mit Dunkler Energie (Λ), kalter Dunkler Materie (CDM) und baryonischer Materie.
- **Einfluss:** Erklärt großräumige Struktur des Universums, kosmische Hintergrundstrahlung, beschleunigte Expansion.
- **Herausforderungen:** Natur von Dunkler Materie und Dunkler Energie ist unklar. Alternative Modelle (z. B. modifizierte Newtonsche Dynamik) erklären einige Beobachtungen ohne Dunkle Materie.

---
**Drei Beweise des Satzes von Pythagoras**
1. **Algebraisch:** \( a^2 + b^2 = c^2 \) für rechtwinkliges Dreieck mit Seiten \( a, b, c \).
2. **Geometrisch (Euklid):** Zerlegung in zwei kleinere, ähnliche Dreiecke.
3. **Ähnlichkeitsbeweis:** Flächen von Quadraten über den Seiten.
**Elegantester Beweis:** Liu Huis Beweis (13. Jh.) – verwendet algebraische Identitäten und geometrische Zerlegung, kombiniert beide Ansätze.

---
**Roschs Prototyp & Kognitive Linguistik**
- **Prototyp:** Kategorien haben einen zentralen Prototyp (z. B. Rotkehlchen für "Vogel"), andere Exemplare werden durch Ähnlichkeit klassifiziert.
- **Klassische Theorie:** Kategorien haben klare Grenzen (z. B. "Vogel = hat Federn + kann fliegen").
**Beispiel:** Pinguin ist ein schlechter Prototyp für "Vogel", weil er nicht fliegen kann – trotzdem wird er als Vogel erkannt.

---
**Sprachliche Relativität (Sapir-Whorf) & Pirahã-Zahlen**
- **Starke Hypothese:** Sprache bestimmt das Denken (z. B. Hopi haben kein Zeitkonzept).
- **Schwache Hypothese:** Sprache beeinflusst das Denken (z. B. Pirahã haben nur "eins", "zwei", "viele").
**Pirahã-Studien:** Die Sprache beeinflusst numerische Kognition – Pirahã können keine genauen Mengen über 2 unterscheiden. Zeigt, dass Sprache Denkprozesse prägt, aber nicht vollständig determiniert.

---
**Huntingtons "Clash of Civilizations" vs. Fukuyamas "Ende der Geschichte"**
- **Huntington:** Nach dem Kalten Krieg werden Konflikte entlang kultureller (nicht ideologischer) Linien verlaufen (z. B. Islam vs. Westen).
- **Fukuyama:** Liberal-demokratische Systeme sind das "Ende der Geschichte" – der Endpunkt ideologischer Evolution.
**Kritik:** Huntington unterschätzt globale Wirtschaftskooperation; Fukuyama ignoriert lokale Konflikte und kulturelle Hybridisierung.

---
**R2P vs. Souveränität (Internationales Recht)**
- **R2P (Responsibility to Protect):** Staatliche Souveränität ist an die Verantwortung gebunden, die Bevölkerung zu schützen. Bei Versagen hat die internationale Gemeinschaft das Recht zu intervenieren.
- **Souveränität:** Staatliche Souveränität und Nicht-Einmischung sind zentrale Prinzipien (Art. 2(7) UN-Charta).
**Libyen 2011:** R2P wurde als Rechtfertigung für Interventionen genutzt, die als Verletzung der Souveränität kritisiert wurden.

---
**Gödels Sätze & formale Systeme mit Peano-Arithmetik**
- **1. Satz:** Jedes konsistente, rekursiv axiomatisierbare System, das die Peano-Arithmetik enthält, ist unvollständig.
- **2. Satz:** Ein solches System kann seine eigene Konsistenz nicht beweisen.
**Warum gilt das nicht für vollständige geordnete Körper?** Diese Systeme können Aussagen über ihre eigene Konsistenz treffen (z. B. "0 ≠ 1"), daher sind Gödels Sätze nicht direkt anwendbar.

---
**Piagets vs. Vygotskys Entwicklungspsychologie**
- **Piaget:** Entwicklung ist universell, stadienabhängig (sensomotorisch → formal-operational). Lernen ist individuell.
- **Vygotsky:** Entwicklung ist sozial und kulturell (ZPD, Scaffolding). Lernen durch Interaktion mit kompetenteren Personen.
**Widerspruch:** Piagets Stadien sind starr, Vygotskys ZPD dynamisch. Piaget betont biologische Reifung, Vygotsky kulturelle Einflüsse.

---
**Osmanisches Millet-System: Religiöser Pluralismus oder strukturelle Ungleichheit?**
- **Pluralismus:** Ermöglichte langfristige Stabilität in einem multiethnischen Reich durch religiöse Autonomie.
- **Ungleichheit:** Muslimische Mehrheit hatte Vorrechte; Nicht-Muslime waren rechtlich und sozial benachteiligt. Tansimat-Reformen (19. Jh.) versuchten, dies zu ändern, scheiterten aber an Widerstand und strukturellen Problemen.

---
**Thukydides-Falle & USA-China**
- **These:** Krieg zwischen aufstrebender und etablierter Macht ist wahrscheinlich (z. B. Athen vs. Sparta).
- **Beschränkungen:** Globalisierung, wirtschaftliche Interdependenz und nukleare Abschreckung machen direkte Konflikte unwahrscheinlich. Wirtschaftliche Rivalität und technologische Konkurrenz dominieren.

---
**R2P vs. Souveränität (Internationales Recht)**
- **R2P (Responsibility to Protect):** Staatliche Souveränität ist an die Verantwortung gebunden, die Bevölkerung zu schützen. Bei Versagen hat die internationale Gemeinschaft das Recht zu intervenieren.
- **Souveränität:** Staatliche Souveränität und Nicht-Einmischung sind zentrale Prinzipien (Art. 2(7) UN-Charta).
**Libyen 2011:** R2P wurde als Rechtfertigung für Interventionen genutzt, die als Verletzung der Souveränität kritisiert wurden.
