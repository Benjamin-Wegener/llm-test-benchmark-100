**Git rebase -i HEAD~15000 (Deutsch)**
Git versucht, den 15.000sten Vorfahren von HEAD zu finden, aber nach dem 10.000sten Commit ist Schluss — Git erreicht den Root-Commit und gibt den Fehler „fatal: invalid upstream 'HEAD~15000'" aus. Der klassische Fehler: man überschätzt die Commit-Tiefe und verwendet eine Zahl größer als die tatsächliche History-Länge.

***

**Python [] == [] vs [] is []**
`==` ruft `__eq__` auf und vergleicht Werte — beide Listen sind leer, also True. `is` vergleicht CPython-interne Objektidentitäten (Pointer auf PyObject-Strukturen im Heap). Jedes `[]` alloziert ein neues PyListObject mit eigener Adresse und eigenem Referenzzähler (ob_refcnt), daher False. Für kleine Integers und interned Strings optimiert CPython durch Caching, aber für Listen nie.

***

**Cohérence forte vs éventuelle, CAP (Français)**
Cohérence forte garantit que toute lecture retourne la dernière valeur écrite, au prix de la disponibilité en cas de partition (CP). La cohérence éventuelle (AP) accepte des lectures temporairement obsolètes mais reste disponible. DynamoDB choisit AP par défaut — les réplicas convergent éventuellement — mais offre la lecture fortement cohérente en option explicite, avec latence plus élevée.

***

**@lru_cache manuell implementieren (日本語)**

```python
import time
from functools import wraps

def my_lru_cache(maxsize=128):
    def decorator(func):
        cache = {}
        order = []
        @wraps(func)
        def wrapper(*args):
            if args in cache:
                return cache[args]
            result = func(*args)
            cache[args] = result
            order.append(args)
            if len(order) > maxsize:
                del cache[order.pop(0)]
            return result
        return wrapper
    return decorator

def fib_plain(n):
    return n if n <= 1 else fib_plain(n-1) + fib_plain(n-2)

@my_lru_cache(maxsize=256)
def fib_cached(n):
    return n if n <= 1 else fib_cached(n-1) + fib_cached(n-2)

t0 = time.time(); fib_plain(35); print("plain:", time.time()-t0)
t0 = time.time(); fib_cached(35); print("cached:", time.time()-t0)
```

キャッシュなしはO(2^n)、キャッシュありはO(n)。fib(35)でplainは数秒、cachedはマイクロ秒単位になる。

***

**Vector Clocks (Español)**

```
init: VC[i] = [0, 0, ..., 0]  (un entero por nodo)

send(msg, nodo_i):
    VC[i][i] += 1
    msg.vc = copy(VC[i])
    transmit(msg)

receive(msg, nodo_j):
    VC[j][k] = max(VC[j][k], msg.vc[k])  para todo k
    VC[j][j] += 1

causal_before(a, b):  # a -> b
    return all(a.vc[k] <= b.vc[k]) and any(a.vc[k] < b.vc[k])

conflicto(a, b):  # ni a->b ni b->a
    return not causal_before(a,b) and not causal_before(b,a)
```

Si dos eventos son concurrentes (conflicto), hay inconsistencia causal potencial que el sistema debe resolver (merge, CRDT, etc.).

***

**量子纠缠与经典关联，贝尔不等式 (中文)**
经典关联可以用隐变量解释（两个手套分开后知道对方）。量子纠缠的关联无法用任何局域隐变量模型复现——测量一个粒子会瞬间影响另一个的统计结果，无论距离多远。贝尔不等式给出了局域隐变量理论的数学上界；实验（Aspect 1982、Delft 2015等）持续违反该不等式，证明自然界是非局域的，不存在局域实在性。

***

**SQL Median ohne PERCENTILE_CONT (Russisch)**

```sql
WITH ranked AS (
    SELECT department_id, salary,
           ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary) AS rn,
           COUNT(*) OVER (PARTITION BY department_id) AS cnt
    FROM employees
)
SELECT department_id,
       AVG(salary) AS median_salary
FROM ranked
WHERE rn IN (FLOOR((cnt + 1) / 2.0), CEIL((cnt + 1) / 2.0))
GROUP BY department_id;
```

При чётном количестве строк берётся среднее двух центральных значений, при нечётном — одно центральное.

***

**توازن ناش و الأمثل باريتو، معضلة السجين، تغير المناخ (عربي)**
توازن ناش هو وضع لا يمكن لأي لاعب تحسين وضعه بتغيير استراتيجيته منفرداً. الأمثل باريتو هو وضع لا يمكن تحسين وضع أحد دون الإضرار بآخر. في معضلة السجين، التوازن هو الاعتراف المتبادل (وضع أسوأ للجميع)، بينما الأمثل باريتو هو الصمت المتبادل. في مفاوضات المناخ: كل دولة تميل للتلوث (سيطرة الاستراتيجية المهيمنة) حتى لو التعاون أفضل للجميع، مما يجعل الاتفاقيات الملزمة ضرورة لتجاوز فخ التوازن.

***

**पियाजे और वायगोत्स्की ZPD (हिंदी)**
पियाजे के चार चरण: संवेदी-गतिज (0-2 वर्ष), पूर्व-संक्रियात्मक (2-7), ठोस संक्रियात्मक (7-11), औपचारिक संक्रियात्मक (11+)। विकास जैविक परिपक्वता से संचालित होता है। वायगोत्स्की का ZPD वह अंतर है जो बच्चा अकेले और सहायता से कर सकता है — सामाजिक अंतःक्रिया विकास को आगे ले जाती है। मूल विरोध: पियाजे के लिए पहले विकास, फिर शिक्षा; वायगोत्स्की के लिए शिक्षा विकास का नेतृत्व करती है।

***

**Thread-safe Singleton in Rust using Once (English)**

```rust
use std::sync::Once;

static INIT: Once = Once::new();
static mut INSTANCE: Option<MyService> = None;

fn get_instance() -> &'static MyService {
    unsafe {
        INIT.call_once(|| { INSTANCE = Some(MyService::new()); });
        INSTANCE.as_ref().unwrap()
    }
}
```

`Once` guarantees the closure runs exactly once across all threads with no overhead after initialization. A plain `Mutex<Option<T>>` adds locking cost on every access, even after init. `OnceLock<T>` (stable since 1.70) and `lazy_static!` are preferred because they're safe (no `unsafe`), ergonomic, and `OnceLock` is built into std without macros.

***

**Byzantine Fault vs Crash Fault, PBFT (Deutsch)**
Ein Crash Fault bedeutet, ein Knoten hört einfach auf zu antworten — er verhält sich nicht falsch, er fällt nur aus. Ein Byzantine Fault bedeutet, der Knoten kann beliebig fehlerhafte oder böswillige Nachrichten senden. Für Crash Faults reichen 2f+1 Knoten (Paxos/Raft). PBFT braucht 3f+1, weil ein fehlerhafter Knoten f ehrliche Knoten belügen kann, während er anderen f gegenüber anders agiert — ohne das dritte Drittel fehlt die Möglichkeit, eine korrekte Quorummehrheit zu bilden, die beides erkennt.

***

**Saga vs Two-Phase Commit (Français)**
Le 2PC bloque les ressources jusqu'à validation — robuste mais fragile en cas de panne du coordinateur, couplage fort, inadapté aux microservices. Le Saga décompose la transaction en étapes locales avec transactions compensatrices en cas d'échec. Orchestration : un coordinateur central pilote les étapes (plus simple à déboguer). Chorégraphie : chaque service réagit à des événements (plus découplé mais difficile à tracer). Choisir 2PC pour des bases ACID locales fortement couplées ; Saga pour des systèmes distribués avec tolérance à la cohérence éventuelle.

***

**Rustの二重解放とOwnership (日本語)**
Rustでは値はただ一つのOwnerを持ち、Ownerがスコープを外れると自動的にdropされる。`move`セマンティクスにより所有権の移転後は元の変数が無効化されるため、同じメモリを二度dropしようとするコードはコンパイルエラーになる。借用（`&`/`&mut`）はOwnershipを移さないが、ライフタイムがコンパイラによって検証され、参照がOwnerより長生きすることを防ぐ。つまり二重解放はコンパイル時の所有権追跡によって構造的に不可能。

***

**Paradoja de Fermi, Gran Filtro (Español)**
La ecuación de Drake estima el número de civilizaciones detectables combinando tasa de formación estelar, fracción con planetas, surgimiento de vida, inteligencia, tecnología y longevidad. Si los valores son plausibles, deberían existir miles de civilizaciones — pero el silencio radioeléctrico es total (Paradoja de Fermi). El Gran Filtro propone que hay una barrera evolutiva extremadamente improbable. Si está detrás de nosotros (el origen de la vida, quizás), hay esperanza; si está adelante (autodestrucción tecnológica), el silencio es una señal ominosa para nuestra civilización.

***

**梯度消失与爆炸，LSTM/GRU (中文)**
RNN反向传播时梯度通过时间步连乘雅可比矩阵。若最大奇异值<1，指数级衰减→梯度消失；>1则指数级增长→梯度爆炸。LSTM通过遗忘门、输入门、输出门控制细胞状态，梯度沿细胞状态路径流动时只做加法（不是乘法），避免连乘衰减。GRU用更新门和重置门简化同样原理。梯度爆炸可用梯度裁剪解决，梯度消失则需门控机制从根本上改变信息流结构。

***

**Raft Konsensus, Split-Brain (Russisch)**
Каждый узел запускает таймер с случайным значением (150–300 мс). Кто истёк первым — становится кандидатом и запрашивает голоса. Набравший большинство (⌊n/2⌋+1) становится лидером. При split-brain разделённая сеть не позволяет ни одной группе набрать кворум, если она меньше большинства — оба сегмента просто ждут. Случайные таймауты снижают вероятность одновременного выдвижения нескольких кандидатов, что предотвращает бесконечные ничьи в голосовании.

***

**السلم الخماسي عبر الثقافات (عربي)**
هناك ثلاثة تفسيرات: أولاً، القيد النفسي-الصوتي — نسب التردد البسيطة (4:3، 3:2) تنتج فترات يسهل معالجتها سمعياً، مما يدفع المقاييس نحو بنى متشابهة. ثانياً، المنطق الثقافي المتقارب — القيود العملية للآلات الموسيقية المبكرة (الناي، الأوتار). ثالثاً، تحيز تعريفي — الباحثون يُصنِّفون مقاييس متباينة تحت مسمى "خماسية" مرناً. الأرجح أن الأمر مزيج من الثلاثة، مع ميل نحو القيد الصوتي كمرساة تطورية.

***

**पेंटाटोनिक स्केल (हिंदी)**
सबसे संभावित कारण मनो-ध्वनिक: सरल आवृत्ति अनुपात (3:2, 4:3) श्रवण तंत्र को स्वाभाविक रूप से सुखद लगते हैं। प्रारंभिक वाद्ययंत्रों की व्यावहारिक सीमाएं भी इस दिशा में धकेलती हैं। यह पूर्ण सार्वभौमिक बाधा नहीं, बल्कि जैविक और सांस्कृतिक कारकों का संयोजन है।

***

**Plotin Henologie vs. aristotelischer Substanzbegriff (Deutsch)**
Der Aristotelische Substanzbegriff setzt ein bestimmtes, prädikationsfähiges Etwas voraus — Substanz als Träger von Eigenschaften, definiert durch Form und Materie. Plotins Eines ist dagegen jenseits jeder Prädikation, jenseits von Sein und Denken — es kann nicht Substanz sein, weil Substanz Bestimmtheit und damit Vielheit impliziert. Nous und Psyche sind Emanationen, die zunehmende Komplexität und Vielheit einführen. Die These stimmt insofern, als Plotin den Substanzbegriff nicht widerlegt, sondern unterlauft: er erklärt ihn für eine Stufe der Realität, die das Eine transzendiert — kein direkter Widerspruch, sondern eine andere ontologische Schicht.

***

**Entropie de Boltzmann et dS = δQ/T (Français)**
Pour un gaz parfait monoatomique, le nombre de micro-états Ω est proportionnel à V^N · U^(3N/2). On calcule S = k ln Ω, puis dS à volume constant donne dS = (3Nk/2U)dU. Comme dU = δQ et T = 2U/3Nk (énergie cinétique moyenne), on obtient dS = δQ/T, cohérent avec la définition thermodynamique classique. La correspondance tient parce que l'entropie de Boltzmann est extensive et reproduit toutes les relations thermodynamiques via les dérivées partielles de S(U,V,N).

***

**カントの物自体とヤコービ・フィヒテ (日本語)**
カントは感性的直観の形式（時空間）を通じてのみ物が認識可能と主張し、物自体は不可知とした。ヤコービの批判：因果性カテゴリは経験内にしか適用できないはずなのに、カントは物自体を「触発する原因」として使っている——内的不整合だ。フィヒテの応答：物自体という概念を完全に廃棄し、自我の自己定立から実在を導く（主観的観念論）。認識論的一貫性という観点では、ヤコービの批判は鋭く、フィヒテの解決は一貫性を保つが超越論的実在論を断念する代償を払う。

***

**Halting Problem, Unentscheidbarkeit (Español)**
Supone que existe H(P, x) que decide si P(x) termina. Construye D(P): si H(P,P)="termina", entra en bucle infinito; si H(P,P)="no termina", termina. Al ejecutar D(D): si termina, no termina; si no termina, termina — contradicción. Por tanto H no puede existir. El problema de equivalencia (¿P ≡ Q para todo x?) es indecidible porque si pudieras decidirlo, podrías reducir el Halting Problem a él: P termina en x ↔ Q_x siempre termina, con Q_x construida apropiadamente.

***

**前景理论与期望效用理论，爱荷华赌博任务 (中文)**
期望效用理论假设人理性地最大化加权效用。前景理论（卡尼曼-特沃斯基）发现：人以参考点为基准评估得失而非绝对财富；损失厌恶（损失约是同等收益痛苦的2倍）；概率权重函数非线性（高估小概率，低估大概率）。爱荷华赌博任务中，正常受试者逐渐偏向低风险牌堆，即使无法明确解释原因——情绪性躯体标记先于理性认知。前额叶损伤患者因缺乏这种躯体标记而持续选择高风险牌堆，支持前景理论中的情绪因素。

***

**Коллапс волновой функции, Many Worlds (Russisch)**
Уравнение Шрёдингера — унитарное, детерминированное, обратимое во времени. Но измерение якобы "коллапсирует" суперпозицию в одно состояние — необратимо и случайно. Это противоречие и есть проблема измерения. Интерпретация многих миров Эверетта устраняет коллапс: при измерении Вселенная расщепляется на ветви для каждого исхода, унитарная эволюция сохраняется везде. Наблюдатель видит один исход, потому что сам входит в суперпозицию. Проблема — объяснение правила Борна (вероятностей) из чисто детерминистской структуры.

***

**القواعد السياقية الحرة مقابل الحساسة بشكل خفيف (عربي)**
القواعد السياقية الحرة (CFG) تولّد تراكيب شجرية — كافية للبنى المتداخلة البسيطة. لكن ظواهر مثل التبعية المتشابكة (cross-serial dependencies) في الهولندية وظاهرة Scrambling لا يمكن وصفها بـCFG. القواعد الخفيفة السياقية الحساسة (MILDLY context-sensitive) مثل TAG و CCG تتسع لهذه الظواهر. تسلسلية تشومسكي تضع اللغات الطبيعية في المستوى 2 (CFG) نظرياً، لكن الأدلة التجريبية تشير إلى أنها تحتاج مستوى أعلى قليلاً — مما يجعل التسلسلية وصفاً غير كافٍ لبنية اللغة الطبيعية الكاملة.

***

**औपचारिक भाषाविज्ञान, CFG vs. MCSG (हिंदी)**
संदर्भ-मुक्त व्याकरण (CFG) नेस्टेड संरचनाएं बना सकता है लेकिन क्रॉस-सीरियल निर्भरताएं नहीं (जैसे डच में)। हल्के संदर्भ-संवेदनशील व्याकरण (TAG, CCG) इन्हें संभाल सकते हैं। चॉम्स्की पदानुक्रम प्राकृतिक भाषा को स्तर 2 पर रखता है, लेकिन वास्तविक भाषाई डेटा थोड़ा ऊपर की शक्ति दिखाता है — इसलिए पदानुक्रम अपर्याप्त है।

***

**Non-delegation, Chevron, Loper Bright (English)**
The non-delegation doctrine holds that Congress cannot transfer its core legislative power to agencies without an intelligible principle. Chevron deference (1984) had courts defer to agencies' "reasonable" statutory interpretations, effectively letting agencies fill legislative gaps. Loper Bright (2024) overruled Chevron: courts must now exercise independent judgment on statutory meaning rather than defer to agency interpretation. This shifts power from the executive branch back to the judiciary, narrows agency rulemaking authority, and creates pressure on Congress to legislate with more precision rather than leaving ambiguity for agencies to exploit.

***

**Ellipse vs. Parabel projektiv-geometrisch (Deutsch)**
Im projektiven Raum hat jede Gerade eine Fernlinie (Gerade im Unendlichen). Eine Ellipse schneidet die Fernlinie in zwei konjugiert-komplexen Punkten — sie hat keine reellen Fernpunkte, bleibt also "geschlossen". Eine Parabel ist tangential zur Fernlinie — sie berührt sie in genau einem reellen Punkt, weshalb sie genau eine Richtung im Unendlichen hat. Die Hyperbel schneidet die Fernlinie in zwei reellen Punkten (die Asymptoten). Diese projektive Klassifikation ist eleganter als die algebraische, weil sie die Kegelschnitte durch ihre Relation zur Ferngeraden unterscheidet.

***

**Hegel Anerkennung, Honneth (Français)**
Dans la Phénoménologie, Hegel montre que la conscience de soi ne peut se constituer qu'en se faisant reconnaître par une autre conscience — d'où la dialectique maître-esclave : chacun cherche la reconnaissance, mais seul l'esclave, en travaillant et transformant le monde, développe une véritable conscience autonome, tandis que le maître dépend de la reconnaissance d'un être qu'il nie. Honneth en tire une théorie sociale : la reconnaissance (amour, droit, estime sociale) est le fondement normatif des luttes sociales. Le mépris — déni de reconnaissance — est moteur du conflit politique et moral.

***

**フィリップス曲線とスタグフレーション (日本語)**
1970年代の石油ショックは供給側の負のショックであり、フィリップス曲線が想定しない状況——高インフレと高失業の同時発生（スタグフレーション）——をもたらした。フリードマンとフェルプスは事前に、期待インフレを考慮すると長期フィリップス曲線は垂直になると論じていた。人々がインフレを期待に織り込めば、短期トレードオフは持続せず、政策当局は失業率を自然率より恒久的に下げられない——この理論的予測が1970年代に実証された。

***

**Aptitud inclusiva de Hamilton (Español)**
La aptitud inclusiva incorpora no solo la reproducción propia sino la de parientes, ponderada por el coeficiente de parentesco r. La regla de Hamilton: el altruismo es favorecido por la selección si rB > C (r = parentesco, B = beneficio al receptor, C = costo al donante). En himenópteros (abejas, avispas, hormigas), las hembras son diploides y los machos haploides, lo que produce r=3/4 entre hermanas — más alto que r=1/2 madre-hija. Esto hace que ayudar a criar hermanas sea genéticamente más rentable que reproducirse directamente, explicando la eusocialidad extrema.

***

**哥德尔不完备定理 (中文)**
第一定理：任何包含Peano算术的一致形式系统都包含既无法证明也无法证伪的命题（不完备）。第二定理：该系统无法在自身内部证明自身一致性。对希尔伯特计划的打击是毁灭性的：计划要求用有限手段证明数学的完备性和一致性，哥德尔证明两者都不可能同时实现。

***

**Humes Induktionsproblem, Popper (Russisch)**
Юм показал: наблюдение повторяющихся событий не даёт логического основания заключать о будущем — индукция не может быть обоснована без порочного круга. Поппер предложил асимметрию: хотя тысячи подтверждений не доказывают теорию, один контрпример её опровергает. Наука движется не подтверждением, а выдвижением смелых гипотез и попытками их фальсифицировать. Проблема: фальсификационизм сам не фальсифицируем, а на практике учёные спасают теории вспомогательными гипотезами (Lakatos).

***

**نموذج روزش الأولي (عربي)**
النظريات الكلاسيكية تعرّف الفئات بشروط ضرورية وكافية (كل طائر له ريش وجناحان). روزش أثبتت أن الفئات تتمحور حول نماذج أولية — أعضاء أكثر تمثيلاً. الحمامة والعصفور نماذج أفضل للطائر من البطريق والنعامة، رغم أنها تستوفي شروط "الطائر". العضوية تدريجية وليست ثنائية. هذا يفسر تأثيرات التمثيلية في التجارب المعرفية (زمن رد الفعل أسرع للنماذج الأولية).

***

**रोश प्रोटोटाइप (हिंदी)**
शास्त्रीय सिद्धांत श्रेणियों को आवश्यक और पर्याप्त शर्तों से परिभाषित करता है। रोश ने दिखाया कि मानव श्रेणीकरण प्रोटोटाइप-केंद्रित है — कुछ सदस्य अधिक "विशिष्ट" होते हैं। रॉबिन गौरैया से बेहतर पक्षी का उदाहरण है, पेंगुइन से बेहतर नहीं, हालांकि दोनों पक्षी हैं। श्रेणी सदस्यता द्विआधारी नहीं, क्रमिक है।

***

**CRISPR-Cas9 vs Base Editing (Deutsch)**
CRISPR-Cas9 erzeugt einen Doppelstrangbruch (DSB) an der Zielsequenz, der durch NHEJ (fehleranfällig) oder HDR (präzise aber selten) repariert wird — kann unbeabsichtigte Insertionen/Deletionen erzeugen. Base Editing (David Liu, 2016) fused Cas9-Nickase mit einer Deaminase: kein DSB, direkte chemische Umwandlung einzelner Basen (C→T oder A→G). Präziser, weil keine Doppelstrangunterbrechung entsteht und kein Donor-Template nötig ist — allerdings begrenzt auf Punktmutationen.

***

**Corrélation, causalité, DAG, collider (Français)**
La distinction corrélation/causalité dit simplement "association ≠ cause". Les DAG de Pearl (graphes orientés acycliques) permettent d'identifier structurellement ce qu'on peut ou ne peut pas estimer à partir de données observationnelles. Un collider est un nœud avec deux flèches entrantes (A→C←B) : contrairement aux confondeurs, conditionner sur un collider crée une association spurieuse entre A et B. Sans ce cadre, même des statisticiens expérimentés commettent des erreurs d'interprétation causale (biais de sélection, paradoxe de Berkson).

***

**オスマン帝国のミッレト制度 (日本語)**
ミッレト制は宗教共同体に法的自治（家族法、教育、宗教行政）を与えた点で宗教的多元主義の実例とされる。しかし同時に構造的不平等を制度化していた：イスラム教徒は最上位に置かれ、非ムスリムはジズヤ（人頭税）を納め、軍事的・官僚的昇進に制限があった。タンジマート改革（1839年以降）が法的平等を宣言したとき、ミッレト制の排他的共同体主義とは根本的に矛盾した——これが後のナショナリズム紛争の一因となる。

***

**Trampa de Tucídides, límites (Español)**
La trampa de Tucídides (Graham Allison) sostiene que cuando una potencia emergente amenaza a la dominante, la guerra es estructuralmente probable — de 16 casos históricos, 12 resultaron en guerra. Limitaciones: selección de casos sesgada, variables confundentes ignoradas (armas nucleares, interdependencia económica), determinismo que subestima la agencia de los actores. Para EE.UU.-China: la interdependencia económica y la disuasión nuclear crean incentivos ausentes en el caso Esparta-Atenas. El modelo es útil como alerta, no como predicción.

***

**保护责任 (R2P) 与主权，利比亚 (中文)**
R2P原则（2005年世界首脑会议）确立：当国家无法或不愿保护本国公民免遭种族灭绝和反人类罪时，国际社会有权干预。这与《联合国宪章》第2(7)条的主权不干涉原则存在张力。利比亚2011年：安理会1973号决议授权"一切必要手段"保护平民。北约将其扩展为政权更迭，俄罗斯和中国认为这超越授权。此案损害了R2P的共识基础，直接导致叙利亚问题上俄中否决权的使用。

***

**Гёдель и арифметика Пеано (Russisch)**
Теорема Гёделя применима к формальным системам, достаточно сильным для арифметики Пеано, потому что именно она позволяет кодировать синтаксические утверждения о самой системе (гёделевская нумерация). Теория полных упорядоченных полей (Tarski's real closed fields) — полна и разрешима, поскольку в ней невозможно закодировать арифметику натуральных чисел: умножение двух переменных не определено в полной мере, что лишает систему достаточной выразительности для самореференции.

***

**پیاجه و ویگوتسکی (عربي)**
المراحل الأربع لبياجيه: الحسية-الحركية (0-2)، ما قبل العملياتية (2-7)، العمليات الملموسة (7-11)، العمليات الشكلية (11+) — النضج البيولوجي يقود التطور. منطقة التطور القريب لفيغوتسكي هي المسافة بين ما يستطيع الطفل عمله وحده وبمساعدة خبير. التعارض الجوهري: بياجيه — التطور يسبق التعلم؛ فيغوتسكي — التعلم الاجتماعي يقود التطور.

***

**मिल्लत व्यवस्था (हिंदी)**
मिल्लत व्यवस्था ने गैर-मुस्लिमों को अपने धार्मिक कानून और शिक्षा की स्वायत्तता दी — बहुलवाद का एक रूप। किन्तु यह संरचनात्मक असमानता भी थी: मुसलमान उच्च थे, अन्य जजिया देते थे, सैन्य और प्रशासनिक पद सीमित थे। तंजीमात सुधारों (1839+) ने कानूनी समानता की घोषणा की, जो मिल्लत की सामुदायिक विशिष्टता से सीधे टकराई और बाद के राष्ट्रवादी संघर्षों की जमीन तैयार की।

***

**Type I/II Error, Bayesian, Replication Crisis (English)**
Type I error (false positive): rejecting a true null hypothesis (probability = α). Type II error (false negative): failing to reject a false null (probability = β, power = 1-β). These don't map onto Bayesian inference because Bayesian posteriors update a prior probability distribution over hypotheses — there's no fixed null to "reject," no α threshold, and evidence is continuous rather than binary. The replication crisis (2010s onward) showed that many published findings in psychology and medicine fail to replicate, largely due to p-hacking, underpowered studies, publication bias, and misinterpretation of p-values as posterior probabilities.

***

**Harmonische Reihe divergiert (Deutsch)**
Oresmes Beweis (14. Jh.): Fasse die Terme in Gruppen zusammen: 1, 1/2, (1/3+1/4), (1/5+…+1/8), … Jede Gruppe ≥ 1/2, weil die k-te Gruppe 2^(k-1) Terme enthält, die alle ≥ 1/2^k sind — Summe ≥ 1/2. Unendlich viele solcher Gruppen à 1/2 ergeben eine divergierende Summe. Die Glieder gehen zwar gegen null (notwendige, nicht hinreichende Bedingung für Konvergenz), aber nicht schnell genug.

***

**Résolution ONU vs Traité, jus cogens (Français)**
Un traité ratifié est juridiquement contraignant pour les États signataires (droit conventionnel). Une résolution de l'Assemblée générale de l'ONU est une recommandation non contraignante, sauf les résolutions du Conseil de sécurité sous Chapitre VII. Le jus cogens désigne les normes impératives du droit international général (interdiction du génocide, torture, esclavage) auxquelles aucun État ne peut déroger, même par traité — elles s'imposent à tous indépendamment du consentement.

***

**トロッコ問題とグリーンの二重過程理論 (日本語)**
トロッコ問題（スイッチ操作）では功利的判断（5人救助）が直感的に受け入れられやすい。歩道橋問題（1人を突き落とす）では同じ計算でも強い道徳的拒絶が生じる。グリーンはこれをデュアルプロセス理論で説明：System 1（感情的・直観的）は身体的接触を含む暴力に強く反応し、System 2（推論的・功利的）は数的計算を行う。義務論/功利主義の対立だけでは、なぜ数学的に同じ状況で直観が異なるかが説明できず、神経科学的基盤（腹内側前頭前皮質活性化）が違いを説明する。

***

**Ilusión de Müller-Lyer (Español)**
Las líneas con puntas hacia afuera parecen más largas que las de puntas hacia adentro, aunque son iguales. Persiste incluso con conocimiento explícito porque el sistema visual opera en dos vías semi-independientes: la vía que detecta longitud (dorsal) no tiene acceso directo a la creencia consciente (ventral). Greg Pepperberg y otros mostraron que la ilusión es menor en culturas sin arquitectura rectangular ("hipótesis del mundo carpintero"), sugiriendo componente cultural, pero la base perceptiva de bajo nivel es el factor dominante — el sistema visual usa ángulos como señales de perspectiva 3D y las "corrige" automáticamente.

***

**弦理论紧化，卡拉比-丘流形 (中文)**
弦理论需要10或11维时空才能自洽，但我们只感知4维。额外6维必须"卷曲"到普朗克尺度（~10^-35 m），即紧化。卡拉比-丘流形是满足里奇平坦条件（SU(3)完整结构）的6维复流形，其拓扑结构决定了低能物理中的粒子质量和耦合常数。问题：存在约10^500种卡拉比-丘流形（"景观"），使弦理论难以作出独特的可检验预测。

***

**Ингибиторы обратной транскриптазы при ВИЧ (Russisch)**
Обратная транскриптаза ВИЧ копирует РНК-геном вируса в ДНК. НИОТ (нуклеозидные ингибиторы) встраиваются в цепь ДНК как ложные субстраты, не имея 3'-OH группы, и обрывают синтез. ННИОТ связываются аллостерически и блокируют конформационные изменения фермента. Монотерапия ведёт к резистентности, потому что обратная транскриптаза крайне ошибочна (~1 ошибка на 10 000 нуклеотидов), генерируя весь возможный мутантный спектр каждый день. Один препарат создаёт сильное давление отбора на единственную мутацию резистентности; комбинации из 3+ препаратов (АРВТ) требуют одновременных несвязанных мутаций, что статистически маловероятно.

***

**مشكلة هايك المعرفية وتخطيط الذكاء الاصطناعي (عربي)**
هايك يرى أن المعرفة الاقتصادية الحيوية موزعة، ضمنية، ومحلية — لا يمتلكها أحد بالكامل. إشارات الأسعار تجمع هذه المعرفة اللامركزية بكفاءة لا يمكن لأي مخطط مركزي محاكاتها. هل يغير الذكاء الاصطناعي هذا؟ جزئياً: يمكنه معالجة بيانات ضخمة، لكن المعرفة الضمنية (التفضيلات، النوايا المستقبلية، السياق المحلي) لا تزال غير قابلة للترميز الكامل. كما أن التخطيط بالذكاء الاصطناعي يخضع لحوافز سياسية تشوه المدخلات. الحجة تبقى ذات صلة، وإن كانت أقل إطلاقاً مع الأنظمة التكيفية.

***

**हायेक की ज्ञान समस्या (हिंदी)**
हायेक के अनुसार आर्थिक ज्ञान विकेंद्रीकृत, स्थानीय और मौन है। मूल्य संकेत इस बिखरी जानकारी को एकत्रित करते हैं। कोई भी केंद्रीय न
