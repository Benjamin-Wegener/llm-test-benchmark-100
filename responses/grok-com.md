Der Befehl `git rebase -i HEAD~15000` schlägt fehl mit „fatal: bad revision 'HEAD~15000'“, weil das Repo nur 10.000 Commits hat und HEAD~15000 nicht existiert. Klassischer Fehler: Entwickler überschätzen die Historie-Länge.

[] == [] ist True, weil == den Inhalt vergleicht (beide Listen leer). [] is [] ist False, weil is die Objekt-Identität prüft. In CPython erzeugt jedes [] ein neues PyListObject (PyObject mit ob_refcnt=1) an unterschiedlicher Heap-Adresse.

La cohérence forte garantit que toute lecture voit immédiatement la dernière écriture (modèle ACID). La cohérence éventuelle permet des lectures temporaires obsolètes qui convergent ensuite. DynamoDB sacrifie la cohérence forte pour la disponibilité et la tolérance aux partitions (AP du théorème CAP).

以下は手動実装です：

```python
def lru_cache(maxsize=128):
    cache = {}
    def decorator(func):
        def wrapper(*args):
            if args not in cache:
                if len(cache) >= maxsize: cache.pop(next(iter(cache)))
                cache[args] = func(*args)
            return cache[args]
        return wrapper
    return decorator

@lru_cache()
def fib(n): return n if n <= 1 else fib(n-1) + fib(n-2)
```

再帰フィボナッチ：メモ化なし O(2^n)、メモ化あり O(n)。実行時間は n=40 で数万倍高速。

En un sistema distribuido, cada nodo mantiene un vector V de contadores de reloj (uno por nodo). Al enviar un mensaje, se incrementa V[i] y se adjunta V. Al recibir, se actualiza V[j] = max(V[j], recibido[j]) y se detecta causalidad: si para todo k, V[k] ≤ recibido[k] y existe k donde V[k] < recibido[k], entonces causal; si incomparable, inconsistencia.

```pseudocode
on send(m):
    V[self] += 1
    m.vector = copy(V)
    broadcast(m)

on receive(m):
    for i in nodes:
        V[i] = max(V[i], m.vector[i])
```

量子纠缠是量子态的非局域关联，测量一个粒子瞬间决定另一个的状态，无法用经典概率描述。经典关联是局域的、可分离的。贝尔不等式违反证明量子关联不能用局域隐变量解释，证实量子力学的非局域性。

```sql
SELECT dept, salary
FROM (
    SELECT dept, salary,
           ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary) AS rn,
           COUNT(*) OVER (PARTITION BY dept) AS cnt
    FROM employees
) t
WHERE rn = (cnt + 1) / 2 OR rn = (cnt / 2) + 1;
```

في نظرية الألعاب، توازن ناش هو حالة لا يستفيد أي لاعب من تغيير استراتيجيته وحده. الأمثل باريتو هو حالة لا يمكن تحسين أحد دون إيذاء آخر. في معضلة السجين يختلفان لأن الخيانة هي ناش لكن التعاون أفضل باريتو. هذا يفسر صعوبة التعاون الدولي في تغير المناخ: كل دولة تفضل "الخيانة" (عدم خفض الانبعاثات) رغم أن التعاون أفضل للجميع.

पियाजे के चार चरण: संवेदी-गतिक (0-2), पूर्व-क्रियात्मक (2-7), ठोस क्रियात्मक (7-11), औपचारिक क्रियात्मक (11+). वायगोत्स्की का ZPD: वह क्षेत्र जहां बच्चा सहायता से सीख सकता है लेकिन अकेले नहीं. विरोध: पियाजे व्यक्तिगत खोज पर जोर, वायगोत्स्की सामाजिक-सांस्कृतिक मध्यस्थता पर.

```rust
use std::sync::{Once, OnceLock};

static INIT: Once = Once::new();
static mut SINGLETON: Option<MySingleton> = None;

pub struct MySingleton;

impl MySingleton {
    pub fn get() -> &'static MySingleton {
        unsafe {
            INIT.call_once(|| SINGLETON = Some(MySingleton));
            SINGLETON.as_ref().unwrap()
        }
    }
}
```

lazy_static/OnceLock bevorzugt, weil sie lazy + thread-safe ohne expliziten Mutex-Lock bei jedem Zugriff sind; Mutex wäre teurer und blockierend.

Byzantine Fault: Knoten können beliebig (auch böswillig) falsch antworten. Crash Fault: Knoten fallen nur aus. PBFT braucht n ≥ 3f + 1, weil es bis zu f byzantinische Knoten toleriert und Quorums (2f+1) für Konsens benötigt, um Mehrheit trotz Lügen zu sichern.

Saga (orchestration: zentraler Orchestrator koordiniert; choreography: Events steuern dezentral) ist asynchron, kompensierend und hochverfügbar, aber komplexer. 2PC ist synchron, streng konsistent, aber blockierend und weniger skalierbar. Wähle Saga bei Microservices (hohe Verfügbarkeit), 2PC bei strenger ACID in wenigen Services.

Rust Ownership: Eigentümer (owner) hat unique mutable Reference; Borrowing erlaubt nur eine mutable oder mehrere immutable References gleichzeitig. Lifetime bindet Referenzen an Scope des Owners. Compiler prüft statisch, dass kein zweiter Owner existiert → kein double free möglich (Move semantics + Drop nur beim Owner).

La paradoja de Fermi: Drake-Gleichung schätzt viele Zivilisationen, aber wir hören nichts („Great Silence“). Great Filter: ein extrem unwahrscheinliches Ereignis (z. B. Entstehung von Leben oder Technologie) liegt entweder hinter uns oder vor uns. Implikation: entweder sind wir allein oder die Zivilisationen sterben schnell aus.

在深度RNN中，vanishing gradient源于链式法则中连乘的导数<1（sigmoid/tanh激活），梯度指数衰减；exploding源于>1的连乘导致爆炸。LSTM/GRU通过gate机制（forget/input/output gate）选择性保留/遗忘信息，引入additive路径（cell state），避免梯度消失/爆炸。

Raft: Leader-Election bei Split-Brain – Kandidaten erhöhen Term, senden RequestVote mit randomisierter Election Timeout. Der erste, der Mehrheit erhält, wird Leader; andere treten zurück. Random timeouts verhindern gleichzeitige Wahlen und Split-Votes.

السلم الخماسي يظهر مستقلًا لأنه يعتمد على فواصل صوتية طبيعية سهلة الإدراك (quinten وثالثات) تتوافق مع الجهاز السمعي البشري. هذا دليل على قيد نفسي-صوتي عالمي أكثر من ثقافي متقارب، رغم أن تعريف "الخماسي" قد يختلف قليلاً.

पेंटाटोनिक स्केल विभिन्न परंपराओं में स्वतंत्र रूप से प्रकट होता है क्योंकि यह मानव श्रवण तंत्र की प्राकृतिक harmonic intervals (quinten) से मेल खाता है। यह सार्वभौमिक मनो-ध्वनिक बाधा का प्रमाण है, न कि सिर्फ सांस्कृतिक संयोग।

Plotins Henologie (das Eine jenseits von Sein) widerlegt Aristoteles’ Substanzbegriff nicht vollständig: Nous und Psyche sind emanative Hypostasen, die Substanz als abgeleitet zeigen, aber nicht leugnen – eher Überwindung als Widerlegung.

Für ideales monoatomares Gas: Ω = V^N * (konst. * E^{3N/2}). S = k ln Ω → dS = k (dE/E + N dV/V) = δQ_rev / T (da δQ = dE + P dV und PV = (2/3)E). Damit konsistent mit klassischem dS = δQ/T.

Kants Unterscheidung ist epistemisch konsistent, aber Jacobi kritisiert sie als „unhaltbar“ (Ding an sich unerkennbar → Skeptizismus). Fichte antwortet mit subjektivem Idealismus: Ding an sich ist nur Setzung des Ich, keine echte Grenze.

Beweis durch Reduktion: Angenommen H löst Halting-Problem. Konstruiere D(x) = „haltet H(x,x) nicht“. Dann D(D) führt zu Widerspruch. Gleiche Diagonalisierung zeigt, dass Equivalence-Problem (ob P ≡ Q) nicht entscheidbar ist, da es Halting reduzieren kann.

前景理论的核心区别：参考点依赖、损失厌恶、概率加权（非线性）。预期效用理论是绝对财富、线性概率。Iowa Gambling Task：正常人学会避开坏牌堆（直觉损失厌恶），腹内侧前额叶损伤者持续选坏牌（无法从损失学习）。

测量悖论：波函数坍缩是非幺正的（概率守恒破坏），而薛定谔方程是幺正演化。多世界诠释通过分支宇宙消除坍缩，所有结果同时存在，无需非幺正。

القواعد السياقية الحرة: قواعد Chomsky Type-2، لا تتذكر السياق. السياقية الحساسة بشكل خفيف (mildly context-sensitive): Type 1-، مثل Tree-Adjoining Grammars، تسمح بسياق محدود. تسلسلية Chomsky غير كافية لأن اللغات الطبيعية تحتاج cross-serial dependencies وnested structures أعمق مما تسمح به CFGs.

संदर्भ-मुक्त व्याकरण (Type-2) में उत्पादन नियम A → γ (A non-terminal). हल्के संदर्भ-संवेदनशील में सीमित संदर्भ याद रखने की क्षमता (e.g., TAGs). चॉम्स्की पदानुक्रम अपर्याप्त क्योंकि प्राकृतिक भाषाओं में mildly context-sensitive phenomena (cross-serial dependencies) होते हैं जो CFGs नहीं पकड़ सकते।

Non-delegation doctrine forbids Congress from delegating legislative power without intelligible principle; Chevron deference let agencies interpret ambiguous statutes. Loper Bright (2024) overruled Chevron, requiring courts to independently interpret law → agencies lose interpretive primacy, Congress must be clearer, courts gain power.

Projektiv-geometrisch: Ellipse schneidet die Ferngeraden in zwei konjugierten imaginären Punkten (kein reeller Schnitt), Parabel in einem Punkt (Tangente an Ferngeraden). Dadurch unterscheiden sich ihre Schnittverhalten an Unendlich.

Chez Hegel, Anerkennung ist dialektischer Prozess des Selbstbewusstseins: Kampf um Anerkennung führt zur Herr-Knecht-Dialektik. Diese präfiguriert Honneth, indem sie zeigt, dass Anerkennung (nicht nur Macht) Bedingung für Freiheit und soziale Integration ist.

1970年代のスタグフレーションでフィリップス曲線のトレードオフ崩壊：インフレと失業が同時に上昇。原因は供給ショック（石油危機）と期待インフレのシフトで、曲線自体が右方シフトしたため。

Aptitud inclusiva (Hamilton): éxito reproductivo propio + efecto en parientes ponderado por r. Regla rB > C: altruismo evoluciona si beneficio al receptor (B) ponderado por parentesco (r) > costo (C). Ejemplo: himenópteros eusociales – haplodiploidía da r=0.75 a hermanas > r=0.5, favorece obreras estériles.

哥德尔第一定理：任何包含Peano算术的一致形式系统存在不可证的真语句。第二定理：这样的系统不能证明自身的一致性。对Hilbert计划影响：形式主义证明数学一致性的目标不可能实现。

Проблема индукции Юма: нет логического основания считать, что будущее будет похоже на прошлое. Поппер решает через фальсификационизм: теории нельзя подтвердить индукцией, только опровергнуть; наука — это смелые гипотезы и их фальсификация.

في علم النفس المعرفي، النموذج الأولي (prototype) لروزش: الفئات لها أعضاء مركزيون (typical) ومحيطيون بناءً على التشابه العائلي، لا تعريفات ضرورية وكافية. الطائر الأحمر أفضل مثال لأنه يشبه النموذج (يطير، يغرد) أكثر من البطريق.

संज्ञानात्मक भाषाविज्ञान में, रोश का प्रोटोटाइप: श्रेणियाँ परिवार-सदृश संरचना वाली होती हैं, केंद्र में बेहतर उदाहरण, किनारे पर कम। शास्त्रीय सिद्धांत (necessary & sufficient conditions) के विपरीत।

CRISPR-Cas9 schneidet doppelsträngig (DSB), Base Editing ändert einzelne Basen ohne DSB (Cytosin- oder Adenin-Deaminase + nickase). Base Editing präziser und sicherer, weil kein Indel-Risiko durch NHEJ.

La distinction corrélation/causalité est insuffisante car elle ignore confounders et sélection. DAGs de Pearl modélisent causalité explicitement. Collider: variable commune à deux causes (ex. talent et beauté → célébrité); conditionner sur elle crée fausse corrélation.

オスマン帝国のミッレト制度は宗教的多元主義の成功例（自治・税制）と構造的不平等（非ムスリム劣位）の両面。タンジマート改革で平等化を目指したが矛盾残った。

La "trampa de Tucídides" describe el miedo de la potencia dominante (EE.UU.) ante el ascenso de otra (China) que lleva a la guerra. Limitación: modelo determinista simplista, ignora diplomacia, instituciones y economía global que reducen riesgo.

R2P vs. soberanía: tensión entre intervención humanitaria y no injerencia. En Libia 2011, R2P justificó intervención OTAN, pero derivó en cambio de régimen y caos, mostrando riesgo de abuso y erosión de soberanía.

Теорема Геделя применима к системам с арифметикой Пеано, потому что они допускают самореференцию и кодирование утверждений о доказуемости. Теория полных упорядоченных полей (как вещественные числа) не содержит арифметики достаточно сильной для такой кодировки.

في علم النفس التطوري، مراحل بياجيه: حسي-حركي، قبل عملي، عملي ملموس، شكلي. مفهوم فيغوتسكي: منطقة التطور القريب (مساعدة اجتماعية). تعارض: بياجيه فردي-بنائي، فيغوتسكي اجتماعي-ثقافي.

उस्मानी "मिल्लत व्यवस्था" धार्मिक बहुलवाद की सफल मिसाल (स्वायत्तता) लेकिन संरचनात्मक असमानता (गैर-मुस्लिम अधीनता) का संस्थागतकरण। तंजीमात सुधारों ने समानता लाने की कोशिश की लेकिन विरोधाभास बरकरार रहा।

Type I error: reject H0 when true (false positive). Type II: fail to reject H0 when false (false negative). Bayesian posterior gives P(H|data) directly; no fixed α or β. Replication crisis: many published findings fail to replicate due to p-hacking, publication bias, low power.

Die harmonische Reihe divergiert, obwohl Glieder → 0. Beweis von Oresme: Gruppiere 1 + 1/2 + (1/3+1/4) + (1/5+…+1/8) + … ≥ 1 + 1/2 + (1/4+1/4) + (1/8+…+1/8) + … = 1 + 1/2 + 1/2 + 1/2 + … → ∞.

Résolutions de l’AG ONU ne sont pas juridiquement contraignantes (sauf exceptions), contrairement aux traités ratifiés qui lient les États. Jus cogens: normes impératives (ex. interdiction génocide) qui priment tout, y compris traités.

トロッコ問題と歩道橋問題の直観差は功利主義vs義務論だけでは説明できない。グリーンの二重過程理論：功利主義的判断（トロッコ）は制御的・理性的、義務論的（歩道橋）は感情的・自動的。感情が直観を歪める。

La ilusión Müller-Lyer persiste porque el sistema visual temprano procesa flechas como pistas de profundidad 3D (perspectiva), no longitudes 2D. El conocimiento consciente (cortical) no anula el procesamiento automático subcortical.

弦理论中紧化：额外维度在Planck尺度卷曲成微小空间，使其不可观测。额外维度必须紧化否则引力会泄漏或理论不一致。Calabi-Yau流形提供Ricci-flat Kähler结构，保持超对称。

Ингибиторы обратной транскриптазы (NRTI/NNRTI) блокируют фермент ВИЧ, предотвращая синтез ДНК из РНК. Монотерапия приводит к резистентности, потому что ВИЧ имеет высокую мутационность; одна мутация в гене RT делает препарат неэффективным.

مشكلة هايك المعرفية: المعرفة موزعة محلياً ومتغيرة، لا يمكن لأي مركز جمعها. الاقتصاد المخطط مركزياً يفتقر إلى إشارات الأسعار التي تنقل هذه المعرفة. النقاش صالح ضد التخطيط بالذكاء الاصطناعي لأن الـAI لا يزال يفتقر إلى السياق المحلي الديناميكي.

हायेक की ज्ञान समस्या: ज्ञान विकेंद्रीकृत, स्थानीय और निरंतर बदलता है। केंद्रीय नियोजन मूल्य संकेतों (जो यह ज्ञान संचारित करते हैं) का विकल्प नहीं बन सकता क्योंकि कोई भी नियोजक पूरी जानकारी नहीं रख सकता।

25 M☉ star: H→He (CNO cycle) → He→C/O (triple-α) → C-burning → Ne-burning → O-burning → Si-burning → Fe-core. Iron marks endpoint because fusion beyond Fe is endothermic (consumes energy), core collapses without pressure support.

Wittgensteins Familienähnlichkeit: Begriffe haben überlappende Ähnlichkeiten ohne gemeinsame Essenz (keine notwendigen+hinreichenden Bedingungen). Widerlegt klassische Definitionen, zeigt Sprache als Gebrauch.

Histonmodifikationen (Acetyl., Methyl.) und DNA-Methylierung sind epigenetische Marken. Sie können erblich sein (durch Enzyme bei Zellteilung weitergegeben), ohne DNA-Sequenz zu ändern, da sie die Genexpression regulieren.

「ハイエクの知識問題」：知識は分散・局所的・動的で、誰も全体を把握できない。中央計画経済は分散した価格シグナル（知識の要約）を代替できず、資源配分が非効率になる。

Deuda técnica: costo implícito de shortcuts que ralentizan desarrollo futuro (como deuda económica). Sistemas legacy ilustran trampa irreversible: interés compuesto (mantenimiento exponencial) hace refactorización prohibitiva, atrapando a la organización en legacy forever.

福柯的"全景监狱"概念通过可见性与权力关系实现自我规训。当代数字监控（摄像头、算法）正是其延伸，实现无形全景监视。其局限性是忽略抵抗与主体能动性，过于决定论。

Трансформеры используют self-attention + positional encoding. Attention вычисляет зависимости между всеми токенами параллельно (O(1) по длине), в отличие от RNN (O(n) последовательные шаги). Positional encoding добавляет информацию о порядке, позволяя обрабатывать длинные последовательности без vanishing gradients.

فرنسا تستخدم الرقابة الدستورية المسبقة (مجلس دستوري) لأنها تقليد مركزي يحمي السيادة الشعبية من "حكم القضاة". النموذج الأمريكي (مراجعة قضائية لاحقة) يعزز الفصل بين السلطات لكن يسمح للقضاة بإبطال قوانين منتخبة. الآثار: الفرنسي أكثر ديمقراطية مباشرة، الأمريكي أكثر حماية دستورية.

फ्रांस पूर्व-नियंत्रण (संवैधानिक परिषद) उपयोग करता है क्योंकि केंद्रीय परंपरा है जो लोक-संप्रभुता को "न्यायाधीशों का शासन" से बचाती है। अमेरिकी न्यायिक समीक्षा शक्तियों के पृथक्करण को मजबूत करती है लेकिन निर्वाचित कानूनों को रद्द करने की अनुमति देती है। लोकतांत्रिक निहितार्थ: फ्रेंच अधिक प्रत्यक्ष लोकतंत्र, अमेरिकी अधिक संवैधानिक सुरक्षा।

Hartree-Fock unterschätzt Korrelationsenergie systematisch, weil es die Wellenfunktion als einzelne Slater-Determinante approximiert und Elektronen-Korrelation (instantane Abstoßung) ignoriert. Post-HF-Methoden: MP2, CCSD(T), CI adressieren dies durch Mehr-Determinanten-Ansätze.

La querelle des investitures (1075–1122) a séparé pouvoir temporel et spirituel: empereur perdait droit d’investir évêques. Le Concordat de Worms (1122) reconnaît l’élection canonique par l’Église et l’investiture temporelle par l’empereur, établissant dualité des pouvoirs.

熱力学第二法則：エントロピー増加（孤立系）。マクスウェルの悪魔：分子選別でエントロピー減少に見えるパラドックス。ランダウアーの原理：情報消去にkT ln2の熱発生が必要で、悪魔の記憶消去でエントロピー増大し第二法則を守る。

Acto locucionario: decir algo con sentido literal. Ilocucionario: intención del hablante (prometer, ordenar). Perlocucionario: efecto en oyente (persuadir, asustar). Diferencia: locucionario es el acto mismo, ilocucionario su fuerza, perlocucionario su consecuencia.

流动性陷阱成因：利率接近零，货币政策失效（人们持币不消费）。传统货币政策（降息）无效。日本"失去的二十年"提供实证：零利率+通缩预期导致长期停滞，验证流动性陷阱持久性。

P vs NP: P — задачи, решаемые за полиномиальное время. NP — задачи, где решение можно проверить за полиномиальное время. NP-полные (например, задача коммивояжера): самые сложные в NP; если любую решить в P, то P=NP.

"نهاية التاريخ" عند هيغل: تطور الروح نحو الحرية. "المادية التاريخية" عند ماركس: صراع الطبقات يؤدي للشيوعية. كوجيف أعاد تفسير هيغل كـ"نهاية" ليبرالية، أثر على فوكوياما والليبرالية السياسية في القرن 20.

हेगेल की "इतिहास की समाप्ति" (आत्मा की स्वतंत्रता की ओर विकास) और मार्क्स की "ऐतिहासिक भौतिकवाद" (वर्ग-संघर्ष) में उत्तराधिकार (दोनों द्वंद्वात्मक) लेकिन विच्छेद (हैगेल आदर्शवादी, मार्क्स भौतिकवादी)।

Equal-tempered tritone (6 semitones) is maximally dissonant (beats at ~35 Hz) yet resolves strongly to tonic in V7 because it symmetrically splits octave and voice-leads by semitone to third+seventh of I.

Deflation in verschuldeter Wirtschaft: reale Schulden steigen (bei nominal fixen Krediten), Konsum/Investition sinken → Preise fallen weiter → Schuldenlast steigt → Spirale (Fisher debt-deflation).

Bourdieu: violence symbolique est imposition de catégories dominantes comme naturelles (via école, médias). Habitus (dispositions incorporées) reproduit inégalités car agents agissent selon schèmes inconscients qui légitiment l’ordre social.

プラグマティズムで：パースの真理＝長期的合意（探究の理想的限界）。ジェームズ＝実用的有用性（個人の信念）。デューイ＝民主的探究の道具（状況依存）。相違は形而上学 vs 実践 vs 社会。

El "problema difícil" de Chalmers: por qué procesos físicos dan lugar a experiencia subjetiva (qualia). Zombies filosóficos (seres físicamente idénticos sin conciencia) desafían fisicalismo: si son concebibles, conciencia no es idéntica a física.

图灵测试预设行为主义（智能=不可区分行为）。局限：忽略内在理解、主观意识。塞尔中文房间：语法规则操作不等于理解语义，反驳强AI（句法≠语义）。

CRISPR-Cas9: gRNA связывается с ДНК по комплементарности, PAM (NGG) распознаётся Cas9, RuvC режет один strand, HNH — второй, создавая DSB.

في علم النفس التطوري، اللياقة الشاملة (Hamilton): النجاح التناسلي الشخصي + تأثير على الأقارب * r. قاعدة rB > C: الإيثار يتطور إذا كان المنفعة المرجحة بالقرابة تفوق التكلفة. يتصالح الإيثار الظاهري مع الانتخاب الطبيعي عبر الانتخاب الكيني (kin selection).

विकासात्मक मनोविज्ञान में पियाजे (व्यक्तिगत निर्माण, चार चरण) और वायगोत्स्की (सामाजिक-सांस्कृतिक, ZPD) की तुलना: विरोध मूलतः पियाजे का व्यक्तिवाद vs वायगोत्स्की का सामाजिक मध्यस्थता।

The code has a race condition: 1000 goroutines call Get (presumably unlocked read) concurrently, then lock only for write. Multiple reads can interleave with writes, causing lost updates or inconsistent val. Fix: lock around entire Get+increment or use atomic.

Signaltransduktion via GPCRs: Ligand bindet → Konformationsänderung → G-Protein (α-GTP) dissoziiert, aktiviert Effektoren (Adenylylcyclase, PLC). GPCRs häufigstes Ziel, weil sie vielfältig (Geruch, Licht, Hormone) und druggable sind (>30 % aller Medikamente).

Le Default Mode Network (DMN) est actif au repos, impliqué dans mind-wandering, autobiographie, théorie de l’esprit et conscience de soi. Il s’oppose au réseau de tâche et sous-tend l’intégration consciente.

アローの三公理：非独裁、無関係の代替案、Pareto効率。社会的選択関数がこれらを満たすと循環的選好が生じ（Condorcet）、存在しないことがArrowの定理で証明される。

Problema del barco de Teseo: identidad persiste pese a reemplazo gradual de partes. Locke: continuidad de materia orgánica. Hume: haz de percepciones sin núcleo fijo. Parfit: identidad psicológica reducida, no importa el "mismo barco".

"多重実現性"は心理学の物理還元論に反対：同じ心的状態が異なる物理基盤で実現可能。プットナムの機能主義は応答：心は機能（入力-出力関係）で定義され、物理的実現は複数可能。

Репликация: копирование данных на несколько узлов для отказоустойчивости. Шардинг: разделение данных по ключу на независимые серверы для масштаба. Партиционирование: логическое деление таблиц (range, hash) внутри одной БД.

في علم النفس المعرفي، النموذج الأولي لروزش: الفئات لها أعضاء نموذجيون ومحيطيون بناءً على التشابه، لا تعريفات ضرورية. النظريات الكلاسيكية تفترض شروطاً ضرورية وكافية.

संगीत संज्ञान में पेंटाटोनिक स्केल विभिन्न परंपराओं में स्वतंत्र रूप से प्रकट होता है क्योंकि यह मानव श्रवण तंत्र की प्राकृतिक harmonic intervals से मेल खाता है।

Heckscher-Ohlin: Länder exportieren Güter, die ihre reichlichen Faktoren intensiv nutzen. Leontief-Paradox: USA (kapitalreich) exportierten arbeitsintensive Güter. Krugmans New Trade Theory löst es durch Skalenerträge und Produktdifferenzierung: intra-industrieller Handel unabhängig von Faktorausstattung.

In der Entwicklungspsychologie erklären Piagets vier Stadien individuelle Konstruktion, Vygotskys ZPD soziale Unterstützung. Widerspruch: Piaget betont Reifung und autonome Entdeckung, Vygotsky kulturelle Vermittlung und Zone der nächsten Entwicklung.

Dans la théorie des jeux, équilibre de Nash: aucun joueur ne gagne à dévier unilatéralement. Optimum de Pareto: impossible d’améliorer un sans nuire à un autre. Dans le dilemme du prisonnier, trahison est Nash mais coopération est Pareto-supérieur.

ロールズの無知のヴェール：公正な原則を決めるために個人情報（地位・才能）を隠す。格差原理：不平等は最少有利者の利益になる場合のみ許容。ノージックの libertarianism批判：再分配は権利侵害、ヴェールは恣意的で財産権を無視。

Mecanismo ESBL: enzimas hidrolizan anillo beta-lactámico de penicilinas/cefalosporinas. Amenaza global porque plásmidos conjugativos diseminan genes blaCTX-M/TEM entre bacterias, creando multirresistencia en hospitales y comunidad.

黑格尔"历史终结论"（精神向自由发展）与马克思"历史唯物主义"（阶级斗争）继承辩证法但断裂于唯心vs唯物。马克思批判黑格尔为意识形态倒置。

"Парадокс Симпсона": тенденция, наблюдаемая в группах, исчезает или меняет знак при объединении. Классический пример: лечение почек — у мужчин и женщин лечение эффективнее, но в целом хуже (потому что женщины чаще получают новый препарат, но имеют более тяжёлые случаи).

أدلة المادة المظلمة: دوران المجرات، عدسات الجاذبية، CMB. الطاقة المظلمة: توسع الكون المتسارع. WIMP لم يكتشف مباشرة لأنها تتفاعل ضعيفاً جداً مع المادة العادية (فقط جاذبية وweak force).

ब्रह्मांड विज्ञान में डार्क मैटर प्रमाण: गैलेक्सी रोटेशन, gravitational lensing, CMB। डार्क एनर्जी: accelerating expansion। ΛCDM मॉडल पर प्रभाव: ~27% DM, ~68% DE, सामान्य पदार्थ सिर्फ 5%।

Drei Beweise Pythagoras: 1. Euklid (ähnliche Dreiecke), 2. Garfield (Trapez), 3. van Schooten (ähnliche Dreiecke modern). Elegantester: van Schooten – rein geometrisch, ohne Algebra, visuell klar.

En linguistique cognitive, prototype de Rosch: catégories ont membres centraux (meilleur exemplaire) et périphériques par similarité familiale, pas conditions nécessaires/suffisantes. Rouge-gorge meilleur oiseau que pingouin car correspond mieux au prototype (vole, chante).

「言語相対性仮説」の強い形式：言語が思考を決定（Whorf）。弱い形式：言語が思考に影響（Sapir-Whorf弱）。ピラハ族の数概念研究（Everett）：正確な数量概念なし（one/two/manyのみ）で強い仮説を支持する証拠とされるが論争中。

La teoría del choque de civilizaciones (Huntington) predice conflicto cultural post-Guerra Fría. Tesis del fin de la historia (Fukuyama) predice triunfo liberal-democrático universal. Contraste: Huntington ve choques inevitables, Fukuyama convergencia; ambos criticados por simplificar globalización y economía.
