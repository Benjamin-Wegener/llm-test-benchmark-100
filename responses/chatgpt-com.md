**1 (Deutsch)**
`HEAD~15000` existiert nicht (nur 10 000 Commits). `git rebase -i` schlägt fehl: *invalid upstream*. Klassischer Fehler: falsche Annahme über Historienlänge bzw. Off-by-many; Rebase-Basis liegt außerhalb der erreichbaren Historie.

---

**2 (English)**
`[] == []` → value comparison (`PyObject_RichCompare`): both lists empty → equal.
`[] is []` → identity comparison (pointer equality): two distinct `PyObject*` allocations → False.
CPython: each `[]` creates a new `PyListObject` with its own reference count; `==` checks contents, `is` checks address.

---

**3 (Français)**
Cohérence forte: lecture voit la dernière écriture (linéarisabilité).
Cohérence éventuelle: convergence à terme sans garantie immédiate.
CAP: DynamoDB choisit AP par défaut (disponibilité + tolérance partition), avec option de lectures fortement cohérentes (coût/latence ↑).

---

**4 (日本語)**

```python
import time

def lru_cache(fn):
    cache = {}
    def wrapper(n):
        if n in cache:
            return cache[n]
        res = fn(n)
        cache[n] = res
        return res
    return wrapper

def fib_plain(n):
    if n < 2: return n
    return fib_plain(n-1)+fib_plain(n-2)

@lru_cache
def fib_cached(n):
    if n < 2: return n
    return fib_cached(n-1)+fib_cached(n-2)

for f in (fib_plain, fib_cached):
    t=time.time(); print(f(35), time.time()-t)
```

計算量: 通常 O(φⁿ) → メモ化で O(n)。実行時間は指数→線形に改善。

---

**5 (Español)**
Vector clocks: cada nodo mantiene vector V.

```
on_event(i): V[i]++
on_send(msg): V[i]++; msg.V = V
on_recv(msg):
  for k: V[k]=max(V[k], msg.V[k])
  V[i]++
```

Comparación: V ≤ W ⇒ causalidad; incomparable ⇒ concurrencia.

---

**6 (中文)**
量子纠缠：态不可分解，存在非经典相关。
经典关联：联合概率可分解。
贝尔不等式违反 ⇒ 不存在局域隐变量 ⇒ 非局域性（或放弃实在性/局域性）。

---

**7 (Русский)**

```sql
SELECT dept,
       AVG(salary) AS median
FROM (
  SELECT dept, salary,
         ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary) rn,
         COUNT(*) OVER (PARTITION BY dept) cnt
  FROM t
) s
WHERE rn IN ((cnt+1)/2, (cnt+2)/2)
GROUP BY dept;
```

---

**8 (العربية)**
توازن ناش: لا لاعب يحسّن فرديًا.
الأمثل باريتو: لا يمكن تحسين أحد دون الإضرار بآخر.
معضلة السجين: ناش ≠ باريتو بسبب الحوافز الفردية. في المناخ: مشكلة تنسيق/ركوب مجاني.

---

**9 (हिन्दी)**
पियाजे: संवेदी-गति, पूर्व-संचालन, ठोस, औपचारिक।
वायगोत्स्की: ZPD = सहायता से सीखना।
विरोध: पियाजे = आंतरिक चरण; वायगोत्स्की = सामाजिक-निर्माणवादी।

---

**10 (English)**

```rust
use std::sync::Once;

static mut INSTANCE: Option<MyType> = None;
static ONCE: Once = Once::new();

fn get() -> &'static MyType {
    unsafe {
        ONCE.call_once(|| {
            INSTANCE = Some(MyType::new());
        });
        INSTANCE.as_ref().unwrap()
    }
}
```

`OnceLock`/`lazy_static` safer (no `unsafe`, no locking overhead after init).

---

**11 (Deutsch)**
Crash Fault: Knoten stoppt.
Byzantine: beliebig/feindlich.
PBFT braucht ≥3f+1, um bei f bösartigen Knoten Mehrheiten (2f+1) zu sichern.

---

**12 (Français)**
2PC: fort, mais bloquant.
Saga: compensations, eventual consistency.
Orchestration: centralisé; choreography: événements.
Choix: haute dispo → Saga; forte atomicité → 2PC.

---

**13 (日本語)**
所有権は一意→moveで移転。borrowは一時参照。lifetimeで有効範囲保証。
同一資源に複数free不可→コンパイル時に禁止。

---

**14 (Español)**
Ecuación de Drake estima N.
Paradoja de Fermi: N alto vs silencio.
Gran Filtro: etapas improbables (pasado o futuro).

---

**15 (中文)**
RNN梯度：连乘雅可比→<1消失，>1爆炸。
LSTM/GRU：门控+加性路径，保持梯度。

---

**16 (Русский)**
Raft: выборы с random timeout → уменьшает split vote. majority elects leader.

---

**17 (العربية)**
انتشار الخماسي:

* قيود نفسية-صوتية (بساطة النِّسَب)
* تقارب ثقافي
* تحيز تعريفي
  المرجح: مزيج.

---

**18 (हिन्दी)**
हाँ—मनो-ध्वनिक सरलता (कम असंगति), लेकिन सांस्कृतिक कारक भी।

---

**19 (Deutsch)**
Plotin ersetzt Substanz durch emanative Einheit. Widerlegung nicht vollständig: Aristoteles bleibt kategorial anders.

---

**20 (Français)**
Gaz parfait: Ω ∝ V^N E^{3N/2} ⇒ S=k lnΩ donne dS=δQ/T (via relations thermodynamiques).

---

**21 (日本語)**
物自体は認識不能だが要請的概念。ヤコービは循環批判、フィヒテは自我中心で再構成。

---

**22 (Español)**
Diagonalización: construye programa que contradice halting oracle.
Equivalencia indecidible porque implica halting.

---

**23 (中文)**
前景理论：损失厌恶、非线性概率。
爱荷华任务：人偏向即时收益，违背期望效用。

---

**24 (Русский)**
Коллапс неунитарен vs уравнение Шрёдингера унитарно.
Many-worlds: нет коллапса, только разветвление.

---

**25 (العربية)**
CFG أقل قدرة؛ mildly context-sensitive تغطي تبعيات متداخلة. تشومسكي غير كافٍ للغة الطبيعية.

---

**26 (हिन्दी)**
प्राकृतिक भाषा में cross-serial dependencies → CFG अपर्याप्त।

---

**27 (English)**
Non-delegation limits agency power; Chevron deferred to agencies.
Post-*Loper Bright*: courts interpret law de novo → power shifts to judiciary.

---

**28 (Deutsch)**
Projektiv: Ellipse schneidet Ferngerade nicht; Parabel berührt sie einmal (Doppelpunkt).

---

**29 (Français)**
Reconnaissance: identité via autrui. Maître-esclave → lutte → base pour Honneth.

---

**30 (日本語)**
スタグフレーションで期待が変化→長期フィリップス曲線は垂直。

---

**31 (Español)**
rB>C: altruismo evoluciona si beneficio ponderado por parentesco supera costo.

---

**32 (中文)**
第一定理：真だが証明不能命題。
第二：無矛盾性は自己証明不可。ヒルベルト計画崩壊。

---

**33 (Русский)**
Юм: индукция無根拠。ポッパー: 反証可能性で科学。

---

**34 (العربية)**
النموذج الأولي: فئات ذات مراكز. الطائر الأحمر أقرب للتمثيل من البطريق.

---

**35 (हिन्दी)**
प्रोटोटाइप सिद्धांत: श्रेणियाँ धुंधली, केंद्रीय उदाहरण।

---

**36 (Deutsch)**
Base Editing: keine DSB, nur Basenumwandlung → präziser/sicherer.

---

**37 (Français)**
DAG: causalité explicite. collider: variable avec deux flèches entrantes → biais si conditionnée.

---

**38 (日本語)**
ミッレト: 寛容＋不平等の両面。

---

**39 (Español)**
Trampa de Tucídides: potencia emergente vs dominante. Limitado: simplista.

---

**40 (中文)**
R2P vs 主权：利比亚显示干预与滥用争议。

---

**41 (Русский)**
Пеано содержит арифметику → неполнота. Полные упорядоченные поля неарифметичны в том смысле.

---

**42 (العربية)**
نفس جواب 9.

---

**43 (हिन्दी)**
मिल्लत + तंजीमात = तनाव بين المساواة والإصلاح.

---

**44 (English)**
Type I: false positive; Type II: false negative.
Bayesian: posterior probabilities, not error rates.
Replication crisis: many findings fail to replicate.

---

**45 (Deutsch)**
Oresme: Gruppierung zeigt Divergenz (≥1/2+1/2+…).

---

**46 (Français)**
Résolution AG: non contraignante. Traité: obligatoire. Jus cogens: normes impératives.

---

**47 (日本語)**
二重過程：直感（情動）vs 熟慮 → トロッコ差。

---

**48 (Español)**
Müller-Lyer: inferencias perceptivas profundas → persiste.

---

**49 (中文)**
紧化：额外维度卷曲。卡拉比-丘决定低维物理。

---

**50 (Русский)**
RT inhibitors block reverse transcriptase; monotherapy → rapid resistance.

---

**51 (العربية)**
هايك: المعرفة موزعة؛ الأسعار تنسقها. التخطيط يفشل.

---

**52 (हिन्दी)**
AI भी पूर्णतः वितरित ज्ञान नहीं पकड़ सकता (आलोचना जारी).

---

**53 (English)**
25M☉: H→He (pp/CNO), He→C,O, далее α-captures → Fe. Iron: no exothermic fusion.

---

**54 (Deutsch)**
Familienähnlichkeit: keine festen Essenzen, nur überlappende Ähnlichkeiten.

---

**55 (Français)**
Histones vs méthylation ADN: régulation; héritabilité épigénétique via maintenance.

---

**56 (日本語)**
同51。

---

**57 (Español)**
Deuda técnica: intereses acumulativos → lock-in.

---

**58 (中文)**
全景监狱：内化监控；局限：过度泛化。

---

**59 (Русский)**
Transformers: self-attention + positional encoding → long-range dependencies.

---

**60 (العربية)**
فرنسا: رقابة مسبقة؛ أمريكا: لاحقة. آثار ديمقراطية مختلفة.

---

**61 (हिन्दी)**
पूर्व-समीक्षा = विधायी नियंत्रण; न्यायिक समीक्षा = संतुलन।

---

**62 (Deutsch)**
HF ignoriert Elektronenkorrelation → Energie zu hoch. Post-HF (CI, CC, MP2) korrigieren.

---

**63 (Français)**
Querelle → séparation pouvoirs; Concordat de Worms compromise.

---

**64 (日本語)**
悪魔は情報消去でエントロピー増加（ランダウアー）。

---

**65 (Español)**
Actos: locucionario (decir), ilocucionario (intención), perlocucionario (efecto).

---

**66 (中文)**
流动性陷阱：利率近零→货币政策失效。日本验证。

---

**67 (Русский)**
P: polynomial. NP: verifiable. NP-complete: hardest (TSP).

---

**68 (العربية)**
هيغل vs ماركس: مثالية vs مادية؛ كوجيف أثّر سياسياً.

---

**69 (हिन्दी)**
विरासत + विच्छेद: विचार से भौतिकता की ओर।

---

**70 (English)**
Tritone unstable due to dissonance; resolves via dominant → tonic.

---

**71 (Deutsch)**
Deflation ↑ real debt → sales → price fall → spiral.

---

**72 (Français)**
Violence symbolique: domination intériorisée via habitus.

---

**73 (日本語)**
パース=探究、ジェームズ=有用性、デューイ=実践。

---

**74 (Español)**
Hard problem: qualia. Zombies → fisicalismo cuestionado.

---

**75 (中文)**
图灵测试：行为主义。中文房间：语义缺失。

---

**76 (Русский)**
gRNA направляет Cas9; PAM нужен для разреза; RuvC/HNH режут цепи.

---

**77 (العربية)**
rB>C يفسر الإيثار.

---

**78 (हिन्दी)**
पियाजे vs वायगोत्स्की: व्यक्तिगत vs सामाजिक।

---

**79 (English)**
Race: `Get` outside lock → lost updates. 1000 goroutines overwrite each other.

---

**80 (Deutsch)**
GPCR: Ligand→G-Protein→Second messengers. Viele Targets wegen zentraler Rolle.

---

**81 (Français)**
DMN: pensée interne, auto-référentielle.

---

**82 (日本語)**
アロー: 非独裁・パレート・独立性 → 同時満足不可。

---

**83 (Español)**
Teseo: identidad cambiante. Locke=memoria; Hume=haz; Parfit=continuidad.

---

**84 (中文)**
多重实现性反对还原论；功能主义以功能定义心智。

---

**85 (Русский)**
Репликация=копии; шардинг=разбиение; партиционирование=общее деление.

---

**86 (العربية)**
النموذج الأولي كما سابقًا.

---

**87 (हिन्दी)**
पेंटाटोनिक: सरलता + संज्ञानात्मक प्राथमिकता।

---

**88 (English)**
Heckscher-Ohlin: factor endowments. Leontief paradox contradicts.
Krugman: economies of scale, intra-industry trade.

---

**89 (Deutsch)**
Wie 9.

---

**90 (Français)**
Nash vs Pareto: individuel vs collectif; dilemme divergence.

---

**91 (日本語)**
無知のヴェール→公平。ノージック→権利侵害批判。

---

**92 (Español)**
ESBL: β-lactamasas hidrolizan antibióticos → resistencia global.

---

**93 (中文)**
继承+断裂：辩证法→唯物转向。

---

**94 (Русский)**
Парадокс Симпсона: агрегирование меняет вывод.

---

**95 (العربية)**
دليل المادة/الطاقة المظلمة؛ WIMP غير مكتشف.

---

**96 (हिन्दी)**
ΛCDM: डार्क मैटर + डार्क एनर्जी आधार।

---

**97 (Deutsch)**
Viele Beweise; geometrischer oft elegant.

---

**98 (Français)**
Prototype: exemplaire central (rouge-gorge).

---

**99 (日本語)**
強い相対性=思考決定；弱い=影響。

---

**100 (Español)**
Huntington vs Fukuyama: conflicto cultural vs convergencia liberal.

